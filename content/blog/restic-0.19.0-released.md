---
title: "Restic 0.19.0 Released"
date: 2026-06-09T21:00:00+02:00
---

We are happy to announce the release of [restic 0.19.0](https://github.com/restic/restic/releases/v0.19.0)!

As with previous releases, this one contains many different enhancements, some optimizations as well as a few bugfixes. We are grateful to all the contributors who spent their time and effort to help improve restic yet another time - thank you! Also a huge thanks to everyone in the restic community, who answers technical and non-technical questions tirelessly for both existing and new users - this is very helpful and much appreciated.

Some of the most important changes in this release are:

- Restic now supports the zstd compression modes `fastest` and `better`, allowing for more tailored choice of compression.
- Loading the index for a large repository is now significantly faster, and the `mount` command loads new index files on demand.
- The `check`, `copy`, `diff` and `stats` commands now use less memory when handling large snapshots.
- The `restore` command now supports restoring file ownership on UNIX systems by user and group name with the `--ownership-by-name` option.
- The `backup` command now supports `--exclude-cloud-files` (previously only available on Windows) on supported macOS versions to skip cloud-only files.

Other notable changes include:

- API transaction costs for files up to 256 MiB are reduced by about half in the Azure backend.
- The `copy` command now copies multiple snapshots together so that small pack files are avoided where possible.
- The `backup` command no longer applies exclude rules to paths given on the command line itself (excludes still apply to contents within those paths).
- The `check` command now supports snapshot filters (`--tag`, `--host`, `--path`, or explicit snapshot IDs).
- The `rewrite` command now supports include filters (`--include`, `--include-file`, `--iinclude`, `--iinclude-file`).
- Serving files from a Windows backup in a mounted restic repository using Samba is now possible on a POSIX system.
- Access Control Entry inheritance on Windows is now restored correctly.
- The official restic Docker image now supports `nice` and `ionice` scheduling hints.
- Exit codes have been improved for certain situations and signals, improving scripting reliability.
- Several UX improvements have been made in the CLI and restic's output.

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.19.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
