---
title: "Restic 0.18.1 Released"
date: 2025-09-21T20:36:00+02:00
---

We are happy to announce the release of [restic 0.18.1](https://github.com/restic/restic/releases/v0.18.1)!

This is a bug fix release. Most notably, it resolves a crash when using `backup --stdin-filename` with directory paths and another crash that could occur when directories are removed during backup operations. Support for local repositories stored on a filesystem that does not support `chmod` (such as CIFS or WebDAV mounted via FUSE) has been restored.

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.18.1). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
