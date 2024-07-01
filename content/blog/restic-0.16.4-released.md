---
title: "Restic 0.16.4 Released"
date: 2024-02-04T00:00:00+01:00
---

A new patch release of restic 0.16 has now been released, [restic 0.16.4](https://github.com/restic/restic/releases/v0.16.4)!

This release works around and improves detection of a bug in the compression library used by restic. The resulting issue only happens when using restic 0.16.3 and the `max` compression level (the default `auto` and `off` compression levels are not affected), and when the source files being backed up have specific data in them to trigger the bug.

Although rare and requiring specific circumstances to happen, this bug could result in data corruption in the repository. It is therefore important to upgrade to this new restic release and run the checks mentioned below to make sure you are not affected.

If you used the `max` compression level, run restic `check --read-data`. This will download and verify the whole repository and can be used at any time to completely verify the integrity of a repository. If the check command detects anomalies, follow the suggested steps.

As mentioned in the changelog, this version also adds additional verification of files generated by restic before they are uploaded to the repository. With the large scale and widespread usage of restic in the world, this can help detect even more hardware issues and software bugs than before. However, it will also increase CPU usage during backups. If necessary, the additional checks can be disabled using an option, as described in [the documentation](https://restic.readthedocs.io/en/stable/047_tuning_backup_parameters.html#data-verification).

To download the new release and to see a more detailed list of important changes, please head over to [GitHub](https://github.com/restic/restic/releases/v0.16.4). If you already have restic (0.9.4 or later), you can use the `self-update` command to automatically download and verify the new release.

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! To get help or provide feedback, please write a post in [the forum](https://forum.restic.net).