---
title: "Restic 0.11.0"
date: 2020-11-05T10:00:51+01:00
---

[Restic 0.11.0](https://github.com/restic/restic/releases/v0.11.0) was released today! For downloading the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.11.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

This new release contains a highly anticipated feature on Windows: being able to automatically create a Volume Shadow Copy (VSS) using `restic backup --use-fs-snapshot`. We thought that this feature warrants a new release, just a few weeks after restic 0.10.0 was released. The code was contributed by user [fgma](https://github.com/fgma), thank you very much!

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! Or just write a post in [the forum](https://forum.restic.net).
