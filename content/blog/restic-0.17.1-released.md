---
title: "Restic 0.17.1 Released"
date: 2024-09-05T21:40:00+02:00
---

We are happy to announce the release of [restic 0.17.1](https://github.com/restic/restic/releases/v0.17.1)!

This release contains various bug fixes and small enhancements. The most important ones are the following:

- The backup command now also stores files and directories for which some metadata could not be retrieved.
- On Windows several backup errors have been fixed. In particular volumes and network shares that do not support extended attributes can be backed up again.
- The JSON output of the backup and restore command now properly include the error message.

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.17.1). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
