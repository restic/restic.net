---
title: "Restic 0.17.3 Released"
date: 2024-11-08T00:00:00+01:00
---

We are happy to announce the release of [restic 0.17.3](https://github.com/restic/restic/releases/v0.17.3)!

This patch release fixes a couple of bugs on Windows related to backing up metadata on removable disks and extended attributes with VSS. It also fixes handling of broken SFTP connections, enables `--no-lock` for `prune --dry-run` and works around a problem when browsing FUSE-T mounted repositories.

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.17.3). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
