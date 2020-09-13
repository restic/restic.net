---
title: "Rest Server 0.10 Released"
date: 2020-09-13T11:47:00+02:00
---

We have just released [rest-server 0.10.0](https://github.com/restic/rest-server/releases/tag/v0.10.0) which includes a few potentially breaking changes. For details, please see the changes described [in the release](https://github.com/restic/rest-server/releases/tag/v0.10.0).

This release corrects a security issue. User [wojas](https://github.com/wojas) discovered that it was possible to access repositories belonging to other users. It was not possible to access directories outside of the path the repositories were stored in.

The `rest-server` has an option to allow users (identify by HTTP Basic authentication) to only access their own repository by specifying the `--private-repos` option. For example, the user `foo` can then only access the repository at `https://localhost:8000/foo`, but not the repo of the user `bar` at `http://localhost:8000/bar`.

Wojas discovered that by URL-encoding the slash character (`/`) to `%2F`, it was possible to specify a different path. The HTTP routing framework the `rest-server` uses then decodes the slash and passes it on to the code. This way, it was possible to specify a relative path like this: `http://localhost:8000/foo%2F..%2Fbar`, which allowed user `foo` to access the repository files of user `bar`. Please note that due to the way restic works, user `foo` was unable to decrypt any data of or add new data to the other repository unless they know a valid password.

Please let us know what you think in the forum post linked below!
