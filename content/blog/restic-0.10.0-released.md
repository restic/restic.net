---
title: "restic 0.10.0 released"
date: 2020-09-19T17:44:59+02:00
---

After ten months of work we've just released [restic 0.10.0](https://github.com/restic/restic/releases/v0.10.0)! For downloading the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.10.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

This release contains a lot of improvements, both smaller and bigger ones, and the restic project wishes to send a big thank you to everyone who contributed to make this happen. Every contribution, be it a small documentation change or deeply technical code changes and optimizations, are always welcome. An extra huge thanks goes out to [MichaelEischer](https://github.com/MichaelEischer) for tying it all together with countless hours of tireless reviewing and carefully considered decisions as part of this delicate process.

Below are some highlights of the changes in this release (in no particular order):

 * Change [#2546](https://github.com/restic/restic/pull/2546): Restic now returns exit code 3 when failing to backup all source data, primarily useful when automating backups
 * Fix [#2592](https://github.com/restic/restic/pull/2592): The SFTP backend now supports IPv6 addresses
 * Enh [#2277](https://github.com/restic/restic/issues/2277): Support for the ppc64le architecture has been added
 * Enh [#2569](https://github.com/restic/restic/issues/2569): Support for excluding files by their size has been added
 * Enh [#323](https://github.com/restic/restic/issues/323): You can now copy snapshots from one repository to another using the new `copy` command. In relation to this, please also see the new `--copy-chunk-params` option to the `init` command
 * Enh [#2328](https://github.com/restic/restic/pull/2328): Performance of the `check` command has been much improved
 * Enh [#2195](https://github.com/restic/restic/pull/2195): Performance of the `restore` command has been much improved
 * Enh [#1680](https://github.com/restic/restic/issues/1680) and [#2790](https://github.com/restic/restic/pull/2790): Performance of the `mount` command has been much improved

Please note that this is only a manual selection of the improvements in restic 0.10.0 - there are numerous other optimizations, fixes and improvements to be found in [the changelog](https://github.com/restic/restic/releases/tag/v0.10.0)!

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! Or just write a post in [the forum](https://forum.restic.net).
