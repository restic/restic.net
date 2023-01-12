---
title: "Restic 0.15.0 Released"
date: 2023-01-12T21:14:23+01:00
---

We are happy to announce the release of [restic 0.15.0](https://github.com/restic/restic/releases/v0.15.0)!

In this version, a new [`rewrite`](https://restic.readthedocs.io/en/stable/045_working_with_repos.html#removing-files-from-snapshots) command has been implemented. This allows for removing unwanted data from existing snapshots, for example if you discover that some files were unintentionally backed up, such as files with sensitive content or files that take up a lot of space and don't need to be backed up. Please see the [corresponding documentation](https://restic.readthedocs.io/en/stable/045_working_with_repos.html#removing-files-from-snapshots) for more information and usage examples.

Some other notable enhancements in this release are:

 * backup: Tuning the [number of concurrent file reads](https://restic.readthedocs.io/en/stable/047_tuning_backup_parameters.html#file-read-concurrency) using the new `--read-concurrency` option can improve performance when backing up from fast storage like NVMe.
 * backup: Using the new [`--no-scan`](https://restic.readthedocs.io/en/stable/047_tuning_backup_parameters.html#disabling-backup-progress-estimation) option you can now disable the file tree scanning used to produce an ETA, making the backup complete faster.
 * prune: Memory usage has been optimized to be up to 30% less than before.
 * restore: Using the new [`--sparse`](https://restic.readthedocs.io/en/stable/050_restore.html#restoring-from-a-snapshot) option enables restoring files with long runs of zeroes sparsely where possible.
 * restore: Support for restoring symbolic links on Windows (needs `SeCreateSymbolicLinkPrivilege` or administrative privileges).
 * mount: Support for [macFUSE](https://macfuse.io) 4.x enables mounting a repository as a FUSE filesystem on macOS.
 * b2: Support for application keys without the `deleteFiles` capability allows preventing an attacker from deleting backups.
 * s3: Support for `credential_process` in the AWS configuration allows for more flexible authentication.
 * cli: Restic can now [`generate`](https://restic.readthedocs.io/en/stable/020_installation.html#autocompletion) autocompletion files for PowerShell (alongside bash, fish and zsh).

Please also note that restic now requires Go version 1.18 or newer to build, because of updated dependencies needing more current language features.

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.15.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

**We wish to thank all of our wonderful contributors**, who help making restic even better with every release as well as giving support and guidance to other restic users in the community. Thank you all!

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
