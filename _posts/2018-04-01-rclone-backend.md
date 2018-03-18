---
layout: post
title: "Using rclone as a restic Backend"
---

Originally, restic started with just two backends: `local` and `sftp`. Over time, many new backends were added so that restic is now able to store a repository on a variety of different cloud-based services. However, it became clear that each new backend we add for a new cloud service works slightly different and requires almost constant maintenance.

A while ago, [Alex](https://github.com/fd0) was contacted by [Nick](https://github.com/ncw), who found restic by listening to the episode [#48](https://changelog.com/gotime/48) of the [GoTime podcast](https://changelog.com/gotime). He's the primary author of [rclone](https://github.com/ncw/rclone), a program which is "rsync for cloud storage". At the moment, it supports almost 20 different backends/services! They quickly agreed that it would be awesome if restic could just use clone as a backend, thereby supporting all the backends that rclone can talk to!

Over the last couple of weeks, we have added support for rclone in restic. First, rclone learned how to serve restic's the [REST protocol](https://restic.readthedocs.io/en/latest/100_references.html#rest-backend) ([rclone PR #2116](https://github.com/ncw/rclone/pull/2116)). It is now possible (using a binary of `rclone` compiled from the master branch) to start rclone in server mode like this:

    $ rclone serve restic --addr localhost:8889 --user foo --pass s3kr1t remote:bucket/path

Now, you can point restic (version 0.8.2 or later) to the REST server provided on the TCP port 8889 like this:

    $ restic --repository rest:http://foo:s3kr1t@localhost:8889 init
    [...]
    $ restic --repository rest:http://foo:s3kr1t@localhost:8889 backup $HOME

While this is already awesome, it requires starting and stopping rclone manually. In addition, the HTTP server on the TCP port 8889 can be contacted by all processes running on the local machine. On most systems, it is allow to list the command-lines for all running programs, so users on the same machine can find out the username and password needed to contact the REST server. This is not bad, but we can do better.

The [HTTP2 protocol](https://en.wikipedia.org/wiki/HTTP/2) allows "stream muxing", which means serving several independent requests/streams over the same TCP connection. Most browsers only implement HTTP2 over TLS, so usually TLS is strictly required to be able to use HTTP2. But in general, HTTP2 can also be spoken over a single TCP connection, or another bi-directional channel.

It turned out that it is also possible to run an HTTP2 connection over `stdin`/`stdout` of a newly started process. We've implemented this in restic and rclone. Internally, restic runs `rclone serve restic --stdio`, and it will serve HTTP requests via HTTP2 on `stdin`/`stdout`.

This is great because it does not need any network connections, all data is only available to the user running the restic and rclone processes (except for the super user `root`). There's also no need to coordinate local TCP ports with other users on the system, and restic can easily start and stop the rclone process. It is even possible to run multiple instances of restic/rclone in parallel without having to configure any TCP ports.

We've added the `rclone` backend in [PR #1657](https://github.com/restic/restic/pull/1657) to restic which automates starting and stopping rclone and makes it really easy to use. Suppose you've [configured a remote](https://rclone.org/b2/) with rclone called `b2prod`, which can then be used to access data stored at BackBlaze B2, and you've created a bucket called `yggdrasil`.  You should then be able to list the files in the bucket with the following command:

    $ rclone ls b2prod:yggdrasil

Once this works, you can then use restic to initialize a new repository in the path `/prod-serverA` in the bucket `yggdrasil` like this:

    $ restic --repo rclone:b2prod:yggdrasil/prod-serverA init
    created restic repository 7905a088a0 at rclone:b2prod:yggdrasil/prod-serverA
    [...]

Then you can check with rclone again and find new files in the bucket:

    $ rclone ls b2prod:yggdrasil
        155 prod-serverA/config
        448 prod-serverA/keys/598a405c64c1f878a7eeb6a4691747defdc4d9d4caaa01ae2c3caa02878d3d74

You can find a beta version of restic compiled from the master branch [here](https://beta.restic.net/?sort=time&order=desc).

We're very keen on getting feedback on this new backend! For experience reports, please just comment on the forum thread for this blog post (see below). If you think you've found a bug, please create [a new issue on GitHub](https://github.com/restic/restic/issues/new). Thanks!
