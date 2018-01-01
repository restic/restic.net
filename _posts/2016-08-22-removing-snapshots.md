---
layout: post
title: "Removing Snapshots"
---

The feature that was most often requested feature for restic is the ability to
remove snapshots from the repository. Sometimes, restic was even (rightfully)
criticised for not having such a function.

After about three months of work, [PR #518](https://github.com/restic/restic/pull/518)
was merged into the master branch a few days ago. This pull request brings two
new commands to restic: `forget` and `prune`, which allows you to not only
remove a single snapshot manually, but rather specify a policy according to
which restic should automatically remove snapshots (so you don't have to bother
with them). The remainder of this post will give a short introduction on how
you can use the new commands to implement your own strategy for limiting growth
of the restic repository.

For all of the following commands the repository location and password have
been written to the environment variables `RESTIC_REPOSITORY` and
`RESTIC_PASSWORD` so that the commands can be run directly. This is how to do
it:
{% highlight shell %}
$ export RESTIC_REPOSITORY=/tmp RESTIC_PASSWORD=geheim
{% endhighlight %}

Please note that this feature is not yet contained in any released version of
restic, you need to [compile](https://github.com/restic/restic#build-restic)
the code from the current (as of 22 August 2016) master branch yourself.

## Removing a single snapshot

Let's suppose you have a restic repository and ran a backup at 5:00 o'clock in the morning each day this year.
Running the `snapshots` command shows you around 235 snapshots:

{% highlight shell %}
$ restic snapshots
ID        Date                 Host        Directory
----------------------------------------------------------------------
6e001a58  2016-01-01 04:00:00  mopped      /home/fd0/tmp/data
62a86121  2016-01-02 04:00:00  mopped      /home/fd0/tmp/data
ee891602  2016-01-03 04:00:00  mopped      /home/fd0/tmp/data
[...]
d0267dbb  2016-08-20 05:00:00  mopped      /home/fd0/tmp/data
6261b96f  2016-08-21 05:00:00  mopped      /home/fd0/tmp/data
5116cfdc  2016-08-22 05:00:00  mopped      /home/fd0/tmp/data
{% endhighlight %}

The `forget` command allows removing snapshots. When a snapshot ID like
`6e001a58` for the first snapshot made on 1 January 2016 is specified as the
argument of the command, that snapshot is deleted from the repository:

{% highlight shell %}
$ restic forget 6e001a58
removed snapshot 6e001a58

$ restic snapshots
ID        Date                 Host        Directory
----------------------------------------------------------------------
62a86121  2016-01-02 04:00:00  mopped      /home/fd0/tmp/data
ee891602  2016-01-03 04:00:00  mopped      /home/fd0/tmp/data
a07ffe20  2016-01-04 04:00:00  mopped      /home/fd0/tmp/data
[...]
{% endhighlight %}

The snapshot in a restic repository is really just a pointer to the data that
was present when the snapshot was made. Removing a snapshot does not remove the
data from the repository, only when the command `prune` is run, unreferenced
(and therefore unneeded) data is removed:

{% highlight shell %}
$ restic prune
counting files in repo
building new index for repo
[0:00] 100.00%  1 / 1 files
repository contains 1 packs (8 blobs) with 3.003 MiB bytes
processed 8 blobs: 0 duplicate blobs, 0B duplicate
load all snapshots
find data that is still in use for 234 snapshots
[0:00] 100.00%  234 / 234 snapshots
found 8 of 8 data blobs still in use
will rewrite 0 packs
creating new index
[0:00] 100.00%  2 / 2 files
saved new index as 14a7838d
done
{% endhighlight %}

In this example `prune` was finished quickly, but it can take a longer time to
check the references for each blob of data. Restic combines several blobs of
data into so-called "pack" files. When a pack file is found to contain some
data that is still referenced and other data that isn't needed any more, it
will create a new pack file and write the needed data to it, then remove the
original pack file. This process can also take some time.

## Applying an expire policy

Removing a single snapshot is useful, but not very convenient. Let's check out
the specific parameters of the `forget` command:

{% highlight shell %}
$ restic forget --help
[...]
Help Options:
  -h, --help              Show this help message

[forget command options]
      -l, --keep-last=    keep the last n snapshots
      -H, --keep-hourly=  keep the last n hourly snapshots
      -d, --keep-daily=   keep the last n daily snapshots
      -w, --keep-weekly=  keep the last n weekly snapshots
      -m, --keep-monthly= keep the last n monthly snapshots
      -y, --keep-yearly=  keep the last n yearly snapshots
          --hostname=     only forget snapshots for the given hostname
      -n, --dry-run       do not delete anything, just print what would be done
{% endhighlight %}

The most important parameter is `--dry-run`, which will only print the
snapshots that would be removed according to the policy set by the other
parameters.

The basic idea is that you run `forget` by specifying the right parameters tell
restic which snapshots you want to keep. Restic then goes through the list of
snapshots and removes those that do not match the policy.

Let's try this with a simple policy: Restic should keep the last seven daily
snapshots, eight weekly backups and only a monthly backup for 24 months:

{% highlight shell %}
$ restic forget --dry-run --keep-daily 7 --keep-weekly 8 --keep-monthly 24
keep 21 snapshots:
ID        Date                 Host        Directory
----------------------------------------------------------------------
5116cfdc  2016-08-22 05:00:00  mopped      /home/fd0/tmp/data
6261b96f  2016-08-21 05:00:00  mopped      /home/fd0/tmp/data
d0267dbb  2016-08-20 05:00:00  mopped      /home/fd0/tmp/data
e7e18480  2016-08-19 05:00:00  mopped      /home/fd0/tmp/data
b2fd97b2  2016-08-18 05:00:00  mopped      /home/fd0/tmp/data
9743b40d  2016-08-17 05:00:00  mopped      /home/fd0/tmp/data
3ef3007b  2016-08-16 05:00:00  mopped      /home/fd0/tmp/data
3c3f7ad4  2016-08-15 05:00:00  mopped      /home/fd0/tmp/data
b471d6eb  2016-08-14 05:00:00  mopped      /home/fd0/tmp/data
0f2f3b55  2016-08-07 05:00:00  mopped      /home/fd0/tmp/data
47fe0a0f  2016-07-31 05:00:00  mopped      /home/fd0/tmp/data
0d7b57eb  2016-07-24 05:00:00  mopped      /home/fd0/tmp/data
c94ee5ac  2016-07-17 05:00:00  mopped      /home/fd0/tmp/data
fc48f6b6  2016-07-10 05:00:00  mopped      /home/fd0/tmp/data
5e9fe6d2  2016-07-03 05:00:00  mopped      /home/fd0/tmp/data
774c5721  2016-06-26 05:00:00  mopped      /home/fd0/tmp/data
d9b9c5b2  2016-05-31 05:00:00  mopped      /home/fd0/tmp/data
446e6030  2016-04-30 05:00:00  mopped      /home/fd0/tmp/data
6f86935a  2016-03-31 05:00:00  mopped      /home/fd0/tmp/data
1722682f  2016-02-29 04:00:00  mopped      /home/fd0/tmp/data
dd2bbbf9  2016-01-31 04:00:00  mopped      /home/fd0/tmp/data

remove 213 snapshots:
ID        Date                 Host        Directory
----------------------------------------------------------------------
f3da855f  2016-08-13 05:00:00  mopped      /home/fd0/tmp/data
347274f4  2016-08-12 05:00:00  mopped      /home/fd0/tmp/data
f314dd1f  2016-08-11 05:00:00  mopped      /home/fd0/tmp/data
[...]
a07ffe20  2016-01-04 04:00:00  mopped      /home/fd0/tmp/data
ee891602  2016-01-03 04:00:00  mopped      /home/fd0/tmp/data
62a86121  2016-01-02 04:00:00  mopped      /home/fd0/tmp/data
{% endhighlight %}

You can see that when this command is run without `--dry-run`, restic will
remove a lot of snapshots (213 of 235):

{% highlight shell %}
$ restic forget --keep-daily 7 --keep-weekly 8 --keep-monthly 24
snapshots for host mopped, directories /home/fd0/tmp/data:

keep 21 snapshots:
ID        Date                 Host        Directory
----------------------------------------------------------------------
5116cfdc  2016-08-22 05:00:00  mopped      /home/fd0/tmp/data
6261b96f  2016-08-21 05:00:00  mopped      /home/fd0/tmp/data
d0267dbb  2016-08-20 05:00:00  mopped      /home/fd0/tmp/data
[...]

remove 213 snapshots:
[...]

$ restic prune
counting files in repo
building new index for repo
[0:00] 100.00%  1 / 1 files
repository contains 1 packs (8 blobs) with 3.003 MiB bytes
processed 8 blobs: 0 duplicate blobs, 0B duplicate
load all snapshots
find data that is still in use for 21 snapshots
[0:00] 100.00%  21 / 21 snapshots
found 8 of 8 data blobs still in use
will rewrite 0 packs
creating new index
[0:00] 50.00%  1 / 2 files
saved new index as 504caa39
done
{% endhighlight %}

Afterwards, the list of snapshots is a lot shorter:

{% highlight shell %}
$ restic snapshots
ID        Date                 Host        Directory
----------------------------------------------------------------------
dd2bbbf9  2016-01-31 04:00:00  mopped      /home/fd0/tmp/data
1722682f  2016-02-29 04:00:00  mopped      /home/fd0/tmp/data
6f86935a  2016-03-31 05:00:00  mopped      /home/fd0/tmp/data
446e6030  2016-04-30 05:00:00  mopped      /home/fd0/tmp/data
d9b9c5b2  2016-05-31 05:00:00  mopped      /home/fd0/tmp/data
774c5721  2016-06-26 05:00:00  mopped      /home/fd0/tmp/data
5e9fe6d2  2016-07-03 05:00:00  mopped      /home/fd0/tmp/data
fc48f6b6  2016-07-10 05:00:00  mopped      /home/fd0/tmp/data
c94ee5ac  2016-07-17 05:00:00  mopped      /home/fd0/tmp/data
0d7b57eb  2016-07-24 05:00:00  mopped      /home/fd0/tmp/data
47fe0a0f  2016-07-31 05:00:00  mopped      /home/fd0/tmp/data
0f2f3b55  2016-08-07 05:00:00  mopped      /home/fd0/tmp/data
b471d6eb  2016-08-14 05:00:00  mopped      /home/fd0/tmp/data
3c3f7ad4  2016-08-15 05:00:00  mopped      /home/fd0/tmp/data
3ef3007b  2016-08-16 05:00:00  mopped      /home/fd0/tmp/data
9743b40d  2016-08-17 05:00:00  mopped      /home/fd0/tmp/data
b2fd97b2  2016-08-18 05:00:00  mopped      /home/fd0/tmp/data
e7e18480  2016-08-19 05:00:00  mopped      /home/fd0/tmp/data
d0267dbb  2016-08-20 05:00:00  mopped      /home/fd0/tmp/data
6261b96f  2016-08-21 05:00:00  mopped      /home/fd0/tmp/data
5116cfdc  2016-08-22 05:00:00  mopped      /home/fd0/tmp/data
{% endhighlight %}

## How does restic find the snapshots to remove?

It is important to know how `forget` filters the list of snapshots, so we'll go
through this in detail now. First, restic lists all snapshots and splits the
list into separate lists, one for each combination of host name and directories
that have been saved. In our example above, just one host name (`mopped`) and
directory (`/home/fd0/tmp/data`) was saved, so that makes just one list to go
through.

Restic will then sort the list from newest to oldest snapshot and does the
following, in exactly this order:

When `--keep-last` is set, e.g. to the value 10, the newest ten snapshots are
kept and removed from the list.

When `--keep-hourly` is set, e.g. to the value 4, then restic will find the
four most recent hours in which a snapshot was created. For each of those
hours, it marks the last snapshot as to be kept, and flags the others for
removal. It will then remove all the snapshots for these hours from the list.

It's easier than it sounds. Consider the following snapshots in a repo:

{% highlight shell %}
$ restic snapshots
ID        Date                 Host        Directory
----------------------------------------------------------------------
dbd30e0e  2016-08-22 03:00:00  mopped      /home/fd0/tmp/data
45e789ca  2016-08-22 03:53:08  mopped      /home/fd0/tmp/data
c0411b71  2016-08-22 04:00:00  mopped      /home/fd0/tmp/data
1f782cb4  2016-08-22 04:13:23  mopped      /home/fd0/tmp/data
62df5e1e  2016-08-22 04:18:23  mopped      /home/fd0/tmp/data
0b9fe168  2016-08-22 05:23:00  mopped      /home/fd0/tmp/data
0fe0dcfe  2016-08-22 18:08:17  mopped      /home/fd0/tmp/data
d221a465  2016-08-22 19:24:00  mopped      /home/fd0/tmp/data
98fb9f00  2016-08-22 19:53:23  mopped      /home/fd0/tmp/data
{% endhighlight %}

Running `forget --keep-hourly 4`, restic will find the two snapshots at
19:24:00 and 19:53:23. This is one hour (starting at 19:00:00 and ending at
19:59:59) and restic will only keep the last snapshot for this hour. This
means that `98fb9f00` is kept, and `d221a465` is removed. The next hour that
has a snapshot starts at 18:00:00, the one after that at 05:00:00, and so on.
This is the result of running `forget --keep-hourly 4`:

{% highlight shell %}
$ restic forget --dry-run --keep-hourly 4
keep 21 snapshots:
ID        Date                 Host        Directory
----------------------------------------------------------------------
62df5e1e  2016-08-22 04:18:23  mopped      /home/fd0/tmp/data
0b9fe168  2016-08-22 05:23:00  mopped      /home/fd0/tmp/data
0fe0dcfe  2016-08-22 18:08:17  mopped      /home/fd0/tmp/data
98fb9f00  2016-08-22 19:53:23  mopped      /home/fd0/tmp/data

remove 213 snapshots:
ID        Date                 Host        Directory
----------------------------------------------------------------------
dbd30e0e  2016-08-22 03:00:00  mopped      /home/fd0/tmp/data
45e789ca  2016-08-22 03:53:08  mopped      /home/fd0/tmp/data
c0411b71  2016-08-22 04:00:00  mopped      /home/fd0/tmp/data
1f782cb4  2016-08-22 04:13:23  mopped      /home/fd0/tmp/data
d221a465  2016-08-22 19:24:00  mopped      /home/fd0/tmp/data
{% endhighlight %}

When `--keep-daily` is set, e.g. to the value 7, then restic will apply a similar
approach to `--keep-hourly`: Go through the list, find the last seven days in
which at least one snapshot was made. For each day, keep the last snapshot made
on that day, flag the others for removal, and delete all snapshots from the
list.

The options `--keep-weekly`, `--keep-monthly` and `--keep-yearly` are applied
in the same way.

## Conclusion

This article described an easy way to remove a single snapshot and also
explained how to apply an expire policy for snapshots. This allows regularly
removing snapshots from the repository to limit its growth.

The functions to remove snapshots and unneeded data from the repository are
new. Please [report an issue](https://github.com/restic/restic/issues/new) if
you notice any odd behavior or find bugs.
