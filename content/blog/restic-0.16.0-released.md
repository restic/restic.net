---
title: "Restic 0.16.0 Released"
date: 2023-07-28T21:14:23+02:00
---

We are happy to announce the release of [restic 0.16.0](https://github.com/restic/restic/releases/v0.16.0)!

In this version, the `restore` command now displays a progress bar and can also print the progress as [JSON](https://restic.readthedocs.io/en/stable/075_scripting.html#restore). The `diff`, `dump`, `ls` and `restore` now support the
`<snapshot>:<subfolder>` syntax which allows working with only a subfolder of a snapshot. Please see the documentation of each command for details.
Restic now includes a [`repair snapshots`](https://restic.readthedocs.io/en/stable/077_troubleshooting.html) command, which can be used to repair certain types of damaged repositories. For consistency, the `rebuild-index` command has been renamed to `repair index`.

Some other notable enhancements in this release are:

 * Multi-platform docker containers, also available at [ghcr.io](https://github.com/restic/restic/pkgs/container/restic).
 * Reduce overall memory usage by up to 25%.
 * Improved lock refresh handling when using standby / slow network connections.
 * Try locking a repository multiple times using the `--retry-lock` option.
 * backup: Correctly clean up status bar output.
 * backup: Support [`--group-by`](https://restic.readthedocs.io/en/stable/040_backup.html#file-change-detection) for backup parent selection.
 * forget: Keep oldest snapshot if a `--keep-*` policy could keep additional snapshots.
 * azure: Add support for [Managed / Workload Identity](https://restic.readthedocs.io/en/latest/030_preparing_a_new_repo.html#microsoft-azure-blob-storage).

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.16.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

**We wish to thank all of our wonderful contributors**, who help making restic even better with every release as well as giving support and guidance to other restic users in the community. Thank you all!

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
