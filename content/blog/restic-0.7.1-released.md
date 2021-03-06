---
title: "restic 0.7.1 released"
date: 2017-07-22T00:00:00Z
---

We're proud to present [restic 0.7.1](https://github.com/restic/restic/releases/tag/v0.7.1)!
For downloading the code, head over to [GitHub](https://github.com/restic/restic/releases/tag/v0.7.1).
As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new) you encounter!

The binaries released with each restic version starting are [reproducible](https://reproducible-builds.org/), which means that you can easily reproduce a byte identical version from the source code for that release. Instructions on how to do that are contained in the [builder repository](https://github.com/restic/builder).

Important Changes in 0.7.1
==========================

 * The `migrate` command for changing the `s3legacy` layout to the `default` layout for s3 backends has been improved: It can now be restarted with `restic migrate --force s3_layout` and automatically retries operations on error. [#1073](https://github.com/restic/restic/issues/1073) [#1075](https://github.com/restic/restic/pull/1075)

Small changes
-------------

 * The local and sftp backends now create the subdirs below `data/` on open/init. This way, restic makes sure that they always exist. This is connected to an issue for the sftp server: [#11](https://github.com/restic/rest-server/pull/11#issuecomment-309879710) [#1055](https://github.com/restic/restic/issues/1055) [#1077](https://github.com/restic/restic/pull/1077) [#1105](https://github.com/restic/restic/pull/1105)

 * When no S3 credentials are specified in the environment variables, restic now tries to load credentials from an IAM instance profile when the s3 backend is used. [#1067](https://github.com/restic/restic/issues/1067) [#1086](https://github.com/restic/restic/pull/1086)

 * On Darwin and FreeBSD, restic now prints stats when SIGINFO is received (usually when ctrl+t is pressed). [#1082](https://github.com/restic/restic/pull/1082)

 * The dependencies have been updated. [#1108](https://github.com/restic/restic/pull/1108) [#1124](https://github.com/restic/restic/pull/1124)

 * A bug was found (and corrected) in the index rebuilding after prune, which led to indexes which include files that were not present in the repo any more. There were already checks in place which detected this situation and aborted with an error message. A new run of either `prune` or `rebuild-index` corrected the index files. This is now fixed and a test has been added to detect this. [#1115](https://github.com/restic/restic/pull/1115)

 * Errors for chmod() on Unix for filesystems which do not support it (e.g. smb mounted via gvfs) are now ignored. [#1080](https://github.com/restic/restic/pull/1080) [#1112](https://github.com/restic/restic/pull/1112)

 * The semantic for the `--tags` option to `forget` and `snapshots` was clarified: [#1081](https://github.com/restic/restic/issues/1081) [#1090](https://github.com/restic/restic/pull/1090)
