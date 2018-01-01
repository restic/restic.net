---
layout: post
title: "Please upgrade to restic 0.7.1!"
---

TL;DR:

 * Under some circumstances, data in the repo was not stored properly
 * This could have caused incomplete backups
 * **Upgrade restic to at least 0.7.1 and run `restic rebuild-index` before the next backup**
 * It is good practice to run `restic check` regularly

## Background

We always knew that at some point there may be a more serious bug in restic
that warrants a broader announcement. This is the story of how a small
improvement that makes the code even more conservative lead to a small error
which then caused a couple of unintended consequences. In the end, under some
specific circumstances restic wrote incomplete snapshots to the repository.

But let's start at the beginning. Restic stores all data for backups in a
repository (repo). There are several different classes of files in there, the
most important ones are:

 * `data` files (in the directory `data/`) contain parts of files (blobs) and
   meta data (trees) about backups.
 * `index` files (in the directory `index/`) contain a list of `data` files and
   their content, which blobs and meta data is stored in which `data` file and
   where exactly.

So when restic starts a backup, it first loads all index files to learn which
data is already stored in the repo. This is much cheaper than going through the
complete list of `data` files and read the header of each `data` file to find
out what's in it. Then restic walks the list of files/directories to save,
reads files, splits them into blobs and bundles the blobs not already present
in the repo to `data` files and uploads those files. Regularly during the run
and at the end of the backup process it saves new index files to the repo that
cover the newly uploaded `data` files so that the next restic instance
discovers the new data. Whenever something is wrong with the index (or we even
guess that the index may be inaccurate) we advise users to run `restic
rebuild-index`, which simply lists all `data` files, read the header to list
the contents of each file, writes a new index and removes all old `index` files
from the repo.

When [snapshots are removed](https://restic.github.io/blog/2016-08-22/removing-snapshots) with the
`forget` command, the data in the repo needs to be reorganized so that files
containing data which is not needed any more can be removed. This is what the
`prune` command does. As we've seen above, the `data` files stored in the repo
consists of many different blobs from different files. When all blobs in a file
are not needed any more, the file can be simply removed. That's the easy case.
Oftentimes, restic will detect that some blobs of a `data` file are still
needed and some others are not. In this case, restic downloads the `data` file,
bundles the still needed blobs into a new `data` file and uploads that. At the
end of the process, restic removes the now unneeded `data` files and creates a
new index that covers the current set of `data` files in the repo. The last
step is to remove the old, now superseded index files.

## The First Bug

The procedure we've just described in the previous paragraph contains a bug.
Did you notice it? What happens when after the (now unneeded) files have been
removed the index could not be saved to the repo?

This was reported (thankfully) by a user as [GitHub issue #1032](https://github.com/restic/restic/issues/1032).
The next time restic starts, it reads the old index files (remember, the new
index could not be saved, so restic returned an error), and then thinks
the repo still has the `data` files which have been removed during `prune`
earlier. This is bad because on the next `backup` run, restic may not save
blobs to the repo because it thinks they are already stored there, except these
blobs may have been removed by prune!

So, we thought this issue could easily be resolved: The user can just run
`rebuild-index` to correct the index for his particular repo, and we slightly
modify the order of things that prune does: First create a new index, and only
after it has successfully been saved to the repo remove the old `data` files.
This is what was introduced in [Pull Request #1035](https://github.com/restic/restic/pull/1035).
Unfortunately, the change increased the code complexity for the `prune`
command.

This is what's included in restic 0.7.0, specifically commit
[91a24e8](https://github.com/restic/restic/commit/91a24e822911c48ae00760dd70d2d55a40d8aff6).

## The Second Bug

As you may have guessed, the new code for the `prune` command contained a new
bug: The `data` files to be deleted are not excluded from the new index
created, so after `prune` was completed successfully we had a repo containing a
single index file which covers both the newly created files and some files that
are not stored in the repo any more. But the data is safe, because all blobs
that are still needed have been copied to other `data` files during `prune`,
right?

That's unfortunately not always the case. Suppose we have a file called
`foo.txt` that is included in a snapshot, so its content is saved in some `data`
file in the repo. The file just present during one snapshot, and this snapshot
is removed via the `forget` command. The next run of `prune` will then repack
all blobs in the same `data` file, and remove the file. Except the old, removed
file is still present in the index. So the next time a new snapshot is created
that contains the file again, its content is not uploaded because restic thinks
(that's what the index says) the data is still present in the repo.

The next time `prune` or `rebuild-index` are run, restic notices that the index
contains files that are not there any more and creates a new index without the
obsolete files.

Now we have a problem: There's a snapshot in the repo which contains our
`foo.txt` file, but its content is not saved in the repo! Thankfully, this is
detected by the `check` command. Unfortunately, if we don't have the content of the
file handy anywhere, it is lost.

This bug was corrected in [Pull Request #1115](https://github.com/restic/restic/pull/1115)
and is included in restic 0.7.1. The PR states (incorrectly) that restic will
detect this situation during backup and just re-upload the data, but after
several users reported errors returned by the `check` command it became clear
that this is not the case.

The bug was only included in restic 0.7.0 released on 2017-07-01, in commit
[91a24e8](https://github.com/restic/restic/commit/91a24e822911c48ae00760dd70d2d55a40d8aff6).
It was corrected in the `master` branch on 2017-07-18, which was released as
restic 0.7.1 on 2017-07-22. So the time frame is rather narrow.

## FAQ

### How Do I Know If I'm Affected?

Did you run `prune` with restic 0.7.0, or more specifically commit [91a24e8](https://github.com/restic/restic/commit/91a24e822911c48ae00760dd70d2d55a40d8aff6)
(in case you're tracking the master branch)? If you didn't, you're probably not
affected.

You can check as follows: Run the `rebuild-index` followed by the
`check` command. If it complains about `tree 69691d30, blob 6b9a777b: not found
in index`, then there's a blob missing. The IDs will be different for you.

### What does that mean?

Probably that a file in one or more of your snapshots is missing a piece and
cannot be restored. When the same piece is found during a subsequent backup,
restic will store the blob in the repo and the file can be restored again.

### What data is affected?

For this bug to occur there needs to be a data blob that is present in a
snapshot that is then removed by the `forget` command. It may not be present in
any other, still referenced snapshot. Then `prune` needs to be run. In a
subsequent backup, the data blob is found but not saved again. It needs to be
exactly this order.

## Lessons Learned

We're sorry this bug made it into a release of restic.

Software has bugs, so always make sure you have enough safety checks to detect
important bugs within a short amount of time. In this case, the `check` command
would've detected the error right after `prune` ran, but it seems nobody
(including the restic authors) did run a `check` right after `prune`.

We'll add a prominent recommendation to the manual to regularly run `check`. In
addition, we're planning to have restic print a message when `check` hasn't
been run for some time. This is tracked in
[GitHub issue #1137](https://github.com/restic/restic/issues/1137).
We've also added a test which creates a few snapshots, runs `forget`, `prune`
and finally `check` to see if there's any error.
