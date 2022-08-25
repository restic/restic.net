---
title: "Restic 0.14.0 Released"
date: 2022-08-25T20:16:02+02:00
---

We today happily announce the release of [restic 0.14.0](https://github.com/restic/restic/releases/v0.14.0)! \o/

This release contains a highly anticipated feature in restic - compression! To start using it, either [initialize a new repository](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html#preparing-a-new-repository) (with the option `--repository-version 2`) or [upgrade your current repository](https://restic.readthedocs.io/en/stable/045_working_with_repos.html#upgrading-the-repository-format-version). The [compression level](https://restic.readthedocs.io/en/stable/047_tuning_backup_parameters.html#compression) can be changed from the default `auto` (fast and efficient) to `max` or `off` using the `--compression` option (or the corresponding [environment variable](https://restic.readthedocs.io/en/stable/040_backup.html#environment-variables)). Please note that the new/upgraded repository can only be accessed by restic version 0.14 or higher.

Another feature that can be highly useful is the `--unsafe-recover-no-free-space` option to the `prune` command, which allows you to save the day when the repository storage has been completely saturated. Just make sure to [read the corresponding documentation](https://restic.readthedocs.io/en/stable/060_forget.html#recovering-from-no-free-space-errors) first!

Besides the above, here are some other notable fixes and improvements in this release:

 * Ability to [tune the number of concurrent backend connections](https://restic.readthedocs.io/en/stable/047_tuning_backup_parameters.html#backend-connections), increasing backup performance.
 * Ability to [increase pack size](https://restic.readthedocs.io/en/stable/047_tuning_backup_parameters.html#pack-size), increasing backup performance and using less storage resources.
 * The `check --read-data` and `prune` commands no longer uses temporary files when reading from the repository, resulting in less disk I/O.
 * The `copy` command's performance has been greatly improved.
 * The `init` and `copy` commands have a new `--from-repo` option, replacing the now deprecated `--repo2` option.
 * The `backup` command now uses around 30% less memory when backing up directories with hundreds of thousands (or more) files.

For downloading the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.14.0). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! Or just write a post in [the forum](https://forum.restic.net).
