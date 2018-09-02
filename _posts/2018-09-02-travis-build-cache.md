---
layout: post
title: "Enabling the Go Build Cache on Travis/AppVeyor"
---

The restic project runs the unit and integration tests for each pushed commit
and each pull request. We're using [Travis CI](https://travis-ci.com) for Linux
and OS X, and [AppVeyor](https://www.appveyor.com/) for Windows. We're very
grateful that they provide their services free of charge for Open Source
projects!

Our project not only runs the tests on Travis, but also compiles restic for
each supported architecture, operating system, and version of Go. Go is known
for its fast compilation times, but still, compilation takes its time. Roughly,
for each version of Go, running all the tests takes between 13 and 25 minutes:

![Build Times on Travis CI Before Caching Build Cache](/assets/travis-ci-before-build-cache.png) 

With the release of Go 1.10, a build cache was
[introduced](https://golang.org/doc/go1.10#build), which saves compiled
artifacts in a cache directory. On Linux, that's usually located in
`~/.cache/go-build`. So when the same package (with the same build tags and
architecture and so on) is to be compiled again, the Go compiler can just use
the cached version.

Travis executes the tests in a fresh container each time they are run, but we
can [tell it](https://docs.travis-ci.com/user/caching/) to keep the build cache
directory and extract it before the tests are run next time. The entry in the
configuration file `.travis.yml` looks like this:

```yaml
cache:
  directories:
    - $HOME/.cache/go-build
    - $HOME/gopath/pkg/mod
```

This also caches another directory in the GOPATH called `pkg/mod`, which contains
information about released modules. This is a feature introduced with Go 1.11.

The directories listed above work for Linux, but the Go build cache directory
is somewhere else on OS X, so we need to add the cache entries to our build
matrix:

```yaml
matrix:
  include:
    - os: linux
      go: "1.9.x"
      cache:
        directories:
          - $HOME/.cache/go-build
          - $HOME/gopath/pkg/mod

    - os: linux
      go: "1.10.x"
      cache:
        directories:
          - $HOME/.cache/go-build
          - $HOME/gopath/pkg/mod

    - os: linux
      go: "1.11.x"
      sudo: true
      cache:
        directories:
          - $HOME/.cache/go-build
          - $HOME/gopath/pkg/mod

    - os: osx
      go: "1.11.x"
      cache:
        directories:
          - $HOME/Library/Caches/go-build
          - $HOME/gopath/pkg/mod
```

After a successful build, Travis will then archive both directories, and
extract them again for the next test run. Indeed, this speeds up running the
tests a great deal (depending on how much code has changed):

![Build Times on Travis CI With Caching Build Cache](/assets/travis-ci-with-build-cache.png)

You can see in the screenshot above that the build time for Go 1.9 wasn't
reduced at all since it does not have the build cache feature.

AppVeyor also has a [build cache
feature](https://www.appveyor.com/docs/build-cache/), it can be enabled by
specifying the list of directories to cache in the `appveyor.yml` like this:

```yaml
cache:
  - '%LocalAppData%\go-build'
```

For restic, the build cache is not so relevant on AppVeyor, since we're only
using it to run our integration tests on Windows and don't run any
cross-compilation.

We hope that the build cache does not become too large, it is limited for both
Travis and AppVeyor. We'll keep you posted when we discover any problems with
this approach.

Let us know what you think in the comments (which are hosted in [our
forum](https://forum.restic.net))!
