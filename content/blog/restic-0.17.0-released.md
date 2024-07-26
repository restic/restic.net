---
title: "Restic 0.17.0 Released"
date: 2024-07-26T15:08:00+02:00
---

We are happy to announce the release of [restic 0.17.0](https://github.com/restic/restic/releases/tag/v0.17.0)!

After almost a year of hard work, this release brings a lot of improvements, of which the main highlights are:

- Highly improved [`restore`](https://restic.readthedocs.io/en/stable/050_restore.html) command, now supporting [dry-run](https://restic.readthedocs.io/en/stable/050_restore.html#dry-run), [in-place restore](https://restic.readthedocs.io/en/stable/050_restore.html#restoring-in-place) with different overwrite modes and a [delete](https://restic.readthedocs.io/en/stable/050_restore.html#delete-files-not-in-snapshot) option.
- Resumable and optimized [`prune`](https://restic.readthedocs.io/en/stable/060_forget.html) command, now using up to 60% less memory and being less write-intensive.
- Ability to [skip snapshot creation if nothing changed](https://restic.readthedocs.io/en/stable/040_backup.html#skip-creating-snapshots-if-unchanged).
- Support for [repositories with empty password](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html#repositories-with-empty-password).
- Support for [FUSE-T on macOS](https://restic.readthedocs.io/en/stable/050_restore.html#restore-using-mount).
- Support for [size](https://restic.readthedocs.io/en/stable/045_working_with_repos.html#listing-all-snapshots) and [summary](https://restic.readthedocs.io/en/stable/075_scripting.html#summary) statistics in snapshots.

Other notable mentions are:

- The `repair packs` command can now also handle truncated pack files.
- Improved and more resilient backend error handling.
- [Bitrot detection](https://restic.readthedocs.io/en/stable/040_backup.html#comparing-snapshots) in the `diff` command.
- Improved [SFTP](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html#sftp) upload performance.
- Improved [`dump`](https://restic.readthedocs.io/en/stable/050_restore.html#printing-files-to-stdout) performance for large files.
- Additional [exit codes](https://restic.readthedocs.io/en/stable/075_scripting.html#exit-codes) 10 and 11 for when repository does not exist or cannot be locked.
- Support for reading the backup source [from a command's standard output](https://restic.readthedocs.io/en/stable/040_backup.html#reading-data-from-a-command).
- Support for backing up to [rest-server](https://github.com/restic/rest-server) over a unix socket.
- Support for AWS Assume Role with the S3 backend using the `RESTIC_AWS_ASSUME_*` [environment variables](https://restic.readthedocs.io/en/stable/040_backup.html#environment-variables).
- Support for generating [NCDU file listings](https://restic.readthedocs.io/en/stable/045_working_with_repos.html#listing-files-in-a-snapshot).
- Support for [feature flags](https://restic.readthedocs.io/en/stable/047_tuning_backup_parameters.html#feature-flags).
- Support for specifying hostname via [environment variable](https://restic.readthedocs.io/en/stable/040_backup.html#environment-variables) `RESTIC_HOST`.
- Support for specifying a custom HTTP `User-Agent` using `--http-user-agent` or the [`RESTIC_HTTP_USER_AGENT`](https://restic.readthedocs.io/en/stable/040_backup.html#environment-variables).
- Support for reading [include and exclude patterns from files](https://restic.readthedocs.io/en/stable/050_restore.html#restoring-from-a-snapshot) with the `restore` command.
- Support for [rewriting host and time metadata](https://restic.readthedocs.io/en/stable/045_working_with_repos.html#modifying-metadata-of-snapshots) in snapshots.
- Support for backing up and restoring Extended Attributes and [SecurityDescriptors](https://restic.readthedocs.io/en/stable/040_backup.html#backing-up-special-items-and-metadata) on Windows.
- Support for [forgetting all snapshots using](https://restic.readthedocs.io/en/stable/060_forget.html#removing-all-snapshots) `--unsafe-allow-remove-all`.
- [Extended options](https://restic.readthedocs.io/en/stable/040_backup.html#backing-up) to configure Windows Shadow Copy Service during backup.
- The [official restic release binaries](https://github.com/restic/restic/releases) now require at least ARMv6 support on ARM platforms.

Besides this and various minor fixes and improvements, parts of the code base has been refactored to be more modular and easier to work with for the upcoming changes which will become the next restic version.

We would like to thank all of the wonderful users that have been testing and providing input during the development of this release, as well as helping out in the forum with support and feature discussions! Without your valuable insights, restic would not have become what it is today.

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/tag/v0.17.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
