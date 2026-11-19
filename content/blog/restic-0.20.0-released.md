---
title: "Restic 0.20.0 Released"
date: 2026-11-19T13:00:00+01:00
---

We are happy to announce the release of [restic 0.20.0](https://github.com/restic/restic/releases/v0.20.0)!

This release contains several different enhancements and bugfixes along with significant internal cleanups.
A lot of these come from efforts in the community, and as usual we send our regards to those helping out with improving restic!

This release includes two security related fixes:

- When restoring from a manipulated repository, an attacker could trick restic into writing files with attacker controlled location and content. Thanks to Mario Ramos (@marrionesa), calif.io, Zachariah Mithani (AploSec) and Manuel Riel (@m3nu) for reporting this.
- When restoring with `--ownership-by-name`, if the user/group name does not exist on the restore host, this resulted in the file being owned by root. This could be a problem if the file permissions contained the `setuid` or `setgid` bit. Thanks to Manuel Riel (@m3nu) for fixing this.

Other notable changes in this release are:

- The in-memory index now requires roughly 12% less memory.
- Several improvements in the `repair` command – `repair packs` can now handle both missing and damaged pack files, and `repair snapshots --forget` now supports deleting broken snapshots.
- Snapshots containing empty directories that were rewritten using `repair snapshots` with restic before 0.19.0 can now be loaded again.
- The `restore` command now correctly restores hardlinked files that already partially match the snapshot content.
- The `restore --delete` command now never deletes files if a directory in the snapshot cannot be read, to avoid deleting the existing contents of that directory.
- The `prune` command now supports [JSON output](https://restic.readthedocs.io/en/stable/075_scripting.html#prune).
- The `backup -vv` command now shows excluded items.
- The `list packs` command can now print which pack files are used by a specified snapshot.
- The `B2` backend now supports multi-bucket application keys.
- The `S3` backend now has an option to disable dual-stack endpoints.
- Several crash fixes and other CLI improvements have been implemented.

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.20.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
