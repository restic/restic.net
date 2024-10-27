---
title: "Restic 0.17.2 Released"
date: 2024-10-27T16:55:00+01:00
---

We are happy to announce the release of [restic 0.17.2](https://github.com/restic/restic/releases/v0.17.2)!

This patch release is mainly to fix a problem on Windows where files with the type `irregular` could mistakenly be included in snapshots, causing error messages. Once having upgraded to restic 0.17.2, this can be fixed for existing snapshots by running `restic repair snapshots --forget`.

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.17.2). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
