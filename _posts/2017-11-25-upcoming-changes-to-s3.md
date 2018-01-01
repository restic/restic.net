---
layout: post
title: "Upcoming changes for the S3 backend"
---

TL;DR: If you're using the `s3` backend and cannot access your repo any more, append the path `/restic` to the repository location.

We've just merged [Pull Request #1437](https://github.com/restic/restic/pull/1437) which resolves an issue with the S3 backend reported in [#1292](https://github.com/restic/restic/issues/1292). When the next restic version is released (or you're using the master branch directly), this may require modifying the repository location you're passing to restic.

In order to access a repository stored on S3, restic requires the following information:
 * The endpoint address: For Amazon S3, it's `s3.amazonaws.com`, for a custom hosted [Minio](https://minio.io/) server it may be `https://minio.example.com`.
 * The bucket name (e.g. `backup-servers`)
 * The path within the bucket (e.g. `/repo-server1`)

The endpoint address and bucket name are required for operation, but the path is optional. When the path is not set or empty, restic used to use the default path `/restic` within the bucket. This has now changed: When the path is not set or empty, the repository is put at the root of the bucket, so the path is `/`.

This change was made so that it is possible to have the repository directly in the root of the bucket. The default path `/restic` within the bucket only existed for historical reasons and was never properly documented.

So, if you did not specify a path within the bucket, you'll now get an error that the repo cannot be found:
```
$ restic -r s3:s3.amazonaws.com/backup-servers snapshots
unable to open config file: Stat: The specified key does not exist.
Is there a repository at the following location?
s3:s3.amazonaws.com/backup-servers
```

Then you'll need to add the path `/restic` now in order to access an existing repo, like this:
```
$ restic -r s3:s3.amazonaws.com/backup-servers/restic snapshots
```

As always, if you have feedback for us, please use [the forum](https://forum.restic.net) embedded below or (if you experience a bug) [create a new issue on GitHub](https://github.com/restic/restic/issues/new). Thanks!
