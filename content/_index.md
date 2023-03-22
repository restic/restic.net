---
title: "Backups done right!"
lastmod: 2020-10-06T00:00:00Z
---

# Introduction

Restic is a modern backup program that can back up your files:

 * from **Linux, BSD, Mac and Windows**
 
 * to **many different storage types**, including self-hosted and online services

 * **easily**, being a single executable that you can run without a server or complex setup

 * **effectively**, only transferring the parts that actually changed in the files you back up

 * **securely**, by careful use of cryptography in every part of the process

 * **verifiably**, enabling you to make sure that your files can be restored when needed

 * **freely** - restic is entirely free to use and completely open source

## Quickstart

A short recorded demo of restic:

<script type="text/javascript" src="https://asciinema.org/a/anro1vq39k80ae7c46s9nj6bs.js" id="asciicast-23554" async></script>

To learn more about restic, checkout the user manual:

 * [Manual for restic (released version)](https://restic.readthedocs.io/en/stable/)
 * [Manual for restic (latest development version)](https://restic.readthedocs.io/en/latest/)

## Installation

To install, please follow the [Installation Instructions Page](https://restic.readthedocs.io/en/stable/020_installation.html) in the manual or download the latest native binary on the [GitHub Download Page](https://github.com/restic/restic/releases/latest).

## Compatibility

Backward compatibility for backups is important so that our users are always able to restore saved data. Therefore restic follows [Semantic Versioning](http://semver.org) to clearly define which versions are compatible. The repository and data structures contained therein are considered the "Public API" in the sense of Semantic Versioning.

Once version 1.0.0 is released, we guarantee backward compatibility of all repositories within one major version; as long as we do not increment the major version, data can be read and restored. We strive to be fully backward compatible to all prior versions.

During initial development (versions prior to 1.0.0), maintainers and developers will do their utmost to keep backwards compatibility and stability, although there might be breaking changes without increasing the major version.

## Contributing

Contributions are welcome! More information can be found in [the restic contribution guidelines](https://github.com/restic/restic/blob/master/CONTRIBUTING.md). A document describing the design of restic and the data structures stored on disc is contained in [the design document](http://restic.readthedocs.io/en/latest/100_references.html#design).

## Contact

There are several ways to contact the restic project and its community:

 * If you have **usage or support questions**, please post in [the **forum**](https://forum.restic.net/).
 * If you discover a **bug** or have a **feature** suggestion, feel free to [open a **GitHub issue**](https://github.com/restic/restic/issues/new/choose). Please make sure to fill out the issue template you are presented with when doing so.
 * If you would like to **chat** about restic with other users there is also the IRC channel `#restic` on `irc.libera.chat`, which you can [access through your browser](https://kiwiirc.com/nextclient/#ircs://irc.libera.chat:6697/#restic) if you don't have an IRC client installed. Please note that support questions are preferably asked in the forum.
 * For **other project related inquiries** (**not** support requests), feel free to just write an e-mail to `alexander@bumpern.de` :)

**Important**: If you discover something that you believe to be a possible critical security problem, please do **not** open a GitHub issue but send an email directly to `alexander@bumpern.de`. If possible, please encrypt your email using PGP ([CF8F18F2844575973F79D4E191A6868BD3F7A907](gpg-key-alex.asc)).

## Talks

The following talks have been given about restic:

 * 2021-04-02: [The Changelog: Restic has your backup (Podcast)](https://changelog.com/podcast/434)
 * 2016-01-31: Lightning Talk at the Go Devroom at FOSDEM 2016, Brussels, Belgium
 * 2016-01-29: [restic - Backups mal richtig](https://media.ccc.de/v/c4.openchaos.2016.01.restic): Public lecture in German at [CCC Cologne e.V.](https://koeln.ccc.de) in Cologne, Germany
 * 2015-08-23: [A Solution to the Backup Inconvenience](https://media.ccc.de/browse/conferences/froscon/2015/froscon2015-1515-a_solution_to_the_backup_inconvenience.html): Lecture at [FROSCON 2015](https://www.froscon.de) in Bonn, Germany
 * 2015-02-01: [Lightning Talk at FOSDEM 2015](https://www.youtube.com/watch?v=oM-MfeflUZ8&t=11m40s): A short introduction (with slightly outdated command line)
 * 2015-01-27: [Talk about restic at CCC Aachen](https://videoag.fsmpi.rwth-aachen.de/?view=player&lectureid=4442) (in German)

## Blog

For more information regarding restic development, have a look at [our blog](/blog). The latest posts are:

{{% recent-pages 3 %}}

## License

Restic is licensed under "BSD 2-Clause License". You can find the complete text in the file `LICENSE`.
