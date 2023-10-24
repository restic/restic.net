---
title: "Restic 0.16.1 Released"
date: 2023-10-24T00:00:00+02:00
---

We are happy to announce the release of [restic 0.16.1](https://github.com/restic/restic/releases/v0.16.1)!

This release contains a few usage and cosmetic improvements, but also one bugfix for a very unlikely but still possible data corruption issue when using the `max` compression level. Please read the changelog for more details and information on how to verify your repository if you used `max` compression. Please also note that the `auto` compression level (which is the default) was never affected.

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.16.1). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
