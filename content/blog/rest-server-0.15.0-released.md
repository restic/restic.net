---
title: "REST-server 0.15.0 released"
date: 2026-11-21T18:35:00+01:00
---

We have just released [REST-server 0.15.0](https://github.com/restic/rest-server/releases/tag/v0.15.0).

REST-server now has a `/healthz` endpoint that can be used as a liveness probe for the server. The Docker container now also honors the `TZ` environment variable to configure a timezone. When REST-server runs behind a reverse proxy, it now interprets the forwarded IP if the `--proxy-headers` option is enabled. When using `--proxy-auth-username`, the usernames must now conform to the same validation as in the `.htpasswd` file (unicode letters, digits and `@`, `.`, `_`, `-`).

Besides several bugfixes, REST-server now requires Go 1.26 to build. This raises the macOS minimum to macOS 12 Monterey.

Please see the [changelog](https://github.com/restic/rest-server/releases/tag/v0.15.0) for more specific information!

Please let us know what you think in the forum post linked below!
