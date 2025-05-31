---
title: "REST-server 0.14.0 released"
date: 2025-05-31T22:35:00+02:00
---

We have just released [REST-server 0.14.0](https://github.com/restic/rest-server/releases/tag/v0.14.0).

This release corrects permissions of the `.htpasswd` file created by the Docker container when one does not already exist. The permissions of existing `.htpasswd` files, which were previously created world-readable, must be manually changed if relevant in your setup.

In addition, REST-server adds support for proxy-based authentication and uses hardened TLS settings. Due to dependency updates, REST-server now requires Go 1.23 to build.

Please see the [changelog](https://github.com/restic/rest-server/releases/tag/v0.14.0) for more specific information!

Please let us know what you think in the forum post linked below!
