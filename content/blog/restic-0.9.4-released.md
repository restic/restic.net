---
title: "restic 0.9.4 released"
date: 2019-01-06T00:00:00Z
---

We've just released [restic 0.9.4](https://github.com/restic/restic/releases/v0.9.4). Besides several fixes for bugs it contains a new restorer implementation which downloads data concurrently, so it is much faster than the old one.

For downloading the new release and see a more detailed list of important changes, head over to [GitHub](https://github.com/restic/restic/releases/v0.9.4). If you already have restic 0.9.3, you can use the `self-update` command to automatically download and verify the new release. Due to a [bug](https://github.com/restic/restic/issues/2041) you need to specify where restic should write the new file, e.g. like this:

{{< highlight shell >}}
$ restic self-update --output $(which restic)
{{< / highlight >}}

You can also just specify a path:

{{< highlight shell >}}
$ restic self-update --output /usr/local/bin/restic
{{< / highlight >}}

As always, thanks for [reporting any issues](https://github.com/restic/restic/issues/new/choose) you encounter! Or just write a post in [the forum](https://forum.restic.net).
