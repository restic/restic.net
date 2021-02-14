---
title: "Restic 0.12.0 released"
date: 2021-02-14T11:02:36+01:00
---

We're proud to announce that [restic 0.12.0](https://github.com/restic/restic/releases/v0.12.0) has been released today! This release packs **many** speed improvements in several restic commands. For example, we expect that the garbage collection process (`restic prune`) will be an order of magnitude faster for almost all users. In addition, you can now configure how much data should be optimized (via `--max-repack-size`) and how much unused data is allowed to remain in the repo (via `--max-unused`). These flags gives you more control over how long `restic prune` will take overall, and also allow you to have several shorter runs of `restic prune` instead of a single long one. A special thanks goes out to [Alexander Weiss](https://github.com/aawsome) for these indeed awesome improvements in particular, but also to all the other contributors who work on restic!

For downloading the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.12.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! Or just write a post in [the forum](https://forum.restic.net). If you have the time, please let us know if the `CHANGELOG.md` file (also included in the release on GitHub) is helpful for you. We try hard to make it readable for users, so you do not have to follow the development in detail. Is it worth the effort? What do you think? Please answer in the forum below.
