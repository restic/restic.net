---
title: "Restic 0.13.0 Released"
date: 2022-03-26T19:47:44+01:00
---

We're proud to announce that [restic 0.13.0](https://github.com/restic/restic/releases/v0.13.0) has been released today!

This release contains several bug fixes and improvements worth mentioning:

 * It is now possible to add negative patterns to cancel previous exclusions (e.g. `--exclude '/home/user/*' --exclude '!/home/user/.config'`).
 * The `backup` command now has a `--dry-run` switch which (together with `--verbose`) prints the files that would be included in a backup.
 * We have added checksums for various backends so data uploaded to a backend can be checked there.
 * The `restore` command was optimized and is now twice as fast, the `copy` command was improved as well.

For downloading the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.13.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! Or just write a post in [the forum](https://forum.restic.net).
