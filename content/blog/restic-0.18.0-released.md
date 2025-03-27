---
title: "Restic 0.18.0 Released"
date: 2025-03-27T20:41:00+01:00
---

We are happy to announce the release of [restic 0.18.0](https://github.com/restic/restic/releases/tag/v0.18.0)!

This release brings a large number of improvements, many of which have been contributed by our community. Thank you to everyone who contributed to this release, whether through providing input, testing, contributing code changes, or helping with support in the forum!

The most important changes are:

- Mitigation for a potential attack on the content-defined chunking algorithm used by restic. The attack does not let an attacker decrypt data, but could in very specific circumstances allow an attacker to determine whether a large file, fully known to the attacker, is present in the repository. We consider the chance of a practical attack to be very low, but nonetheless implement protection against it. See this [post](https://github.com/restic/restic/issues/5291#issuecomment-2746146193) for details on the attack and mitigation.
- On Windows and macOS, restic now requires at least Windows 10, Windows Server 2016 or macOS 11 Big Sur. In addition, restic no longer supports TLS versions before TLS 1.2. This is due to dependencies needing Go 1.23 which is now used to build restic.
- JSON support for the `check` and `tag` commands. Exit codes are now also included in JSON output. See the [JSON output](https://restic.readthedocs.io/en/stable/075_scripting.html#json-output) documentation for more information.

Other notable changes include:

- The `deprecate-legacy-index`, `deprecate-s3-legacy-layout`, `explicit-s3-anonymous-auth` and `safe-forget-keep-tags` features are now always enabled and will be removed in restic 0.19.0.
- The `backup` command no longer reports a "lstat [...]: no such file or directory" warning if a file disappears during the backup.
- The `rewrite` command can now [create snapshot summaries](https://restic.readthedocs.io/en/stable/045_working_with_repos.html#removing-files-from-snapshots) for snapshots created before restic 0.17.0.
- Images in the GitHub Container Registry are now built with [SLSA provenance](https://restic.readthedocs.io/en/stable/developer_information.html#verifying-slsa-provenance-for-docker-images).
- Experimental, partial support for [cold storage on S3 backends](https://restic.readthedocs.io/en/stable/faq.html#are-cold-storages-supported).
- Support for [sorting the output](https://restic.readthedocs.io/en/stable/045_working_with_repos.html#listing-files-in-a-snapshot) for the `ls` command.
- Sort `find` output from latest to oldest by default.
- Support for letting `prune` repack all [pack files below a specified size](https://restic.readthedocs.io/en/stable/060_forget.html#customize-pruning).
- Support for excluding [online-only cloud files](https://restic.readthedocs.io/en/stable/040_backup.html#excluding-files) (e.g., from OneDrive) during backups on Windows.
- Enable compression for ZIP archives in the `dump` command.
- Show count of deleted files and directories during `restore`.
- Allow [including/excluding extended file attributes](https://restic.readthedocs.io/en/stable/050_restore.html#restoring-extended-file-attributes) during `restore`.
- Add DragonFlyBSD build support and add xattr support for NetBSD 10+.

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/tag/v0.18.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).
