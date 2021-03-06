---
title: "restic 0.7.0 released"
date: 2017-07-01T00:00:00Z
---

We've just released [restic 0.7.0](https://github.com/restic/restic/releases/tag/v0.7.0)!
For downloading the code, head over to [GitHub](https://github.com/restic/restic/releases/tag/v0.7.0).
As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new) you encounter!

Important Changes in 0.7.0
==========================

 * New "swift" backend: A new backend for the OpenStack Swift cloud storage
   protocol has been added, https://wiki.openstack.org/wiki/Swift
   https://github.com/restic/restic/pull/975
   https://github.com/restic/restic/pull/648

 * New "b2" backend: A new backend for Backblaze B2 cloud storage
   service has been added, https://www.backblaze.com
   https://github.com/restic/restic/issues/512
   https://github.com/restic/restic/pull/978

 * Improved performance for the `find` command: Restic recognizes paths it has
   already checked for the files in question, so the number of backend requests
   is reduced a lot.
   https://github.com/restic/restic/issues/989
   https://github.com/restic/restic/pull/993

 * Improved performance for the fuse mount: Listing directories which contain
   large files now is significantly faster.
   https://github.com/restic/restic/pull/998

 * The default layout for the s3 backend is now `default` (instead of
   `s3legacy`). Also, there's a new `migrate` command to convert an existing
   repo, it can be run like this: `restic migrate s3_layout`
   https://github.com/restic/restic/issues/965
   https://github.com/restic/restic/pull/1004

 * The fuse mount now has two more directories: `tags` contains a subdir for
   each tag, which in turn contains only the snapshots that have this tag. The
   subdir `hosts` contains a subdir for each host that has a snapshot, and the
   subdir contains the snapshots for that host.
   https://github.com/restic/restic/issues/636
   https://github.com/restic/restic/pull/1050

Small changes
-------------

 * For the s3 backend we're back to using the high-level API the s3 client
   library for uploading data, a few users reported dropped connections (which
   the library will automatically retry now).
   https://github.com/restic/restic/issues/1013
   https://github.com/restic/restic/issues/1023
   https://github.com/restic/restic/pull/1025

 * The `prune` command has been improved and will now remove invalid pack
   files, for example files that have not been uploaded completely because a
   backup was interrupted.
   https://github.com/restic/restic/issues/1029
   https://github.com/restic/restic/pull/1036

 * restic now tries to detect when an invalid/unknown backend is used and
   returns an error message.
   https://github.com/restic/restic/issues/1021
   https://github.com/restic/restic/pull/1070

