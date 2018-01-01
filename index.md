---
layout: default
sitenav:
  - { anchor: "intro", title: "Introduction" }
  - { anchor: "quickstart", title: "Quickstart" }
  - { anchor: "installation", title: "Installation" }
  - { anchor: "compatibility", title: "Compatibility" }
  - { anchor: "contribute", title: "Contribute" }
  - { anchor: "contact", title: "Contact" }
  - { anchor: "talks", title: "Talks" }
  - { anchor: "license", title: "License" }
permalink: /
title: Backups done right!
---

# <a name="intro"></a>Introduction

restic is a program that does backups right. The design goals are:

 * **Easy:** Doing backups should be a frictionless process, otherwise you are tempted to skip it.  Restic should be easy to configure and use, so that in the unlikely event of a data loss you can just restore it. Likewise, restoring data should not be complicated.

 * **Fast:** Backing up your data with restic should only be limited by your network or hard disk bandwidth so that you can backup your files every day. Nobody does backups if it takes too much time. Restoring backups should only transfer data that is needed for the files that are to be restored, so that this process is also fast.

 * **Verifiable:** Much more important than backup is restore, so restic enables you to easily verify that all data can be restored.

 * **Secure:** Restic uses cryptography to guarantee confidentiality and integrity of your data. The location the backup data is stored is assumed not to be a trusted environment (e.g. a shared space where others like system administrators are able to access your backups). Restic is built to secure your data against such attackers.

 * **Efficient:** With the growth of data, additional snapshots should only take the storage of the actual increment. Even more, duplicate data should be de-duplicated before it is actually written to the storage backend to save precious backup space.

 * **Free:** restic is free software and licensed under the [BSD 2-Clause License](https://github.com/restic/restic/blob/master/LICENSE) and actively developed on [GitHub](https://github.com/restic/restic/).

## <a name="quickstart"></a>Quickstart

A short recorded demo of restic:

<script type="text/javascript" src="https://asciinema.org/a/anro1vq39k80ae7c46s9nj6bs.js" id="asciicast-23554" async></script>

To learn more about restic, checkout the user manual:

 * [Manual for restic (released version)](https://restic.readthedocs.io/en/stable/)
 * [Manual for restic (latest development version)](https://restic.readthedocs.io/en/latest/)

## <a name="installation"></a>Installation

To install, please follow the [Installation Instructions Page](https://restic.readthedocs.io/en/stable/020_installation.html) in the manual or download the latest native binary on the [GitHub Download Page](https://github.com/restic/restic/releases/latest).

## <a name="compatibility"></a>Compatibility

Backward compatibility for backups is important so that our users are always able to restore saved data. Therefore restic follows [Semantic Versioning](http://semver.org) to clearly define which versions are compatible. The repository and data structures contained therein are considered the "Public API" in the sense of Semantic Versioning.

We guarantee backward compatibility of all repositories within one major version; as long as we do not increment the major version, data can be read and restored. We strive to be fully backward compatible to all prior versions.


## <a name="contribute"></a>Contribute and Documentation

Contributions are welcome! More information can be found in [the restic contribution guidelines](https://github.com/restic/restic/blob/master/CONTRIBUTING.md). A document describing the design of restic and the data structures stored on disc is contained in [the design document](https://github.com/restic/restic/blob/master/doc/Design.md).

## <a name="contact"></a>Contact

If you discover a bug or find something surprising, please feel free to [open a github issue](https://github.com/restic/restic/issues/new). If you would like to chat about restic, there is also the IRC channel `#restic` on `irc.freenode.net`. Or just write an email :)

**Important**: If you discover something that you believe to be a possible critical security problem, please do *not* open a GitHub issue but send an email directly to alexander@bumpern.de. If possible, please encrypt your email using PGP ([0xD3F7A907](https://pgp.mit.edu/pks/lookup?op=get&search=0x91A6868BD3F7A907)).

## <a name="talks"></a>Talks

The following talks will be or have been given about restic:

 * 2016-01-31: Lightning Talk at the Go Devroom at FOSDEM 2016, Brussels, Belgium
 * 2016-01-29: [restic - Backups mal richtig](https://media.ccc.de/v/c4.openchaos.2016.01.restic): Public lecture in German at [CCC Cologne e.V.](https://koeln.ccc.de) in Cologne, Germany
 * 2015-08-23: [A Solution to the Backup Inconvenience](https://media.ccc.de/browse/conferences/froscon/2015/froscon2015-1515-a_solution_to_the_backup_inconvenience.html#video): Lecture at [FROSCON 2015](https://www.froscon.de) in Bonn, Germany
 * 2015-02-01: [Lightning Talk at FOSDEM 2015](https://www.youtube.com/watch?v=oM-MfeflUZ8&t=11m40s): A short introduction (with slightly outdated command line)
 * 2015-01-27: [Talk about restic at CCC Aachen](https://videoag.fsmpi.rwth-aachen.de/?view=player&lectureid=4442#content) (in German)

## Blog

For more information regarding restic development, have a look at [our blog](/blog). The latest posts are:

{% for post in site.posts limit:3 %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}

## <a name="license"></a>License

Restic is licensed under "BSD 2-Clause License". You can find the complete text in the file `LICENSE`.




