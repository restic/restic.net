---
layout: post
title: "Verifying Code Archive Integrity"
---

The restic project and all the source code, even this website is hosted by
GitHub, and they provide an awesome service! This post describes how we sign
releases of restic by using [GnuPG](https://www.gnupg.org) so you can
independently verify the integrity of the source code. In addition it is show
how the automatically generated `tar.gz` files can be recreated with from the
git repository.

### Signing Git Tags

In a git repository, "tags" can be created which basically are just named
pointers to a commit, optionally annotated with other data. You can read about
tags in detail [here](https://git-scm.com/book/en/v2/Git-Basics-Tagging).
Creating a signed tag is really easy: just call `git tag` with `--annotate` and
`--sign`, e.g. to sign the current commit as version 0.1.0 run:

{% highlight shell %}
$ git tag --annotate --sign --message "v0.1.0" v0.1.0
{% endhighlight %}

You can then use `git tag --verify` to check the signature:

{% highlight shell %}
$ git tag --verify v0.1.0
object 16e87d72087a2550b24b413ffc46e9d1fa50dd68
type commit
tag v0.1.0
tagger Alexander Neumann <alexander@bumpern.de> 1440188457 +0200

v0.1.0
gpg: Signature made Fr 21 Aug 2015 22:20:57 CEST using RSA key ID D3F7A907
gpg: Good signature from "Alexander Neumann <alexander@bumpern.de>"
{% endhighlight %}

At the moment, we're in an early stage of the project and haven't setup a
dedicated signing key so all releases are signed by
[fd0](https://github.com/fd0). In order to verify the integrity against this
key you need to import it, e.g. from the public key servers:

{% highlight shell %}
$ gpg --recv-key D3F7A907
{% endhighlight %}

Afterwards make sure to verify the fingerprint:

{% highlight shell %}
$ gpg --fingerprint D3F7A907
pub   rsa4096/D3F7A907 2014-11-01
      Key fingerprint = CF8F 18F2 8445 7597 3F79  D4E1 91A6 868B D3F7 A907
uid         [ultimate] Alexander Neumann <alexander@bumpern.de>
sub   rsa4096/4043FDF1 2014-11-01
{% endhighlight %}

### Recreating Automatically Generated Release Archives

For each tag, we create a release on the [restic GitHub project
page](https://github.com/restic/restic). You can see a list of all releases
[here](https://github.com/restic/restic/releases). For each release, GitHub
offers an automatically generated `tar.gz` file containing the code for that
release.

For restic version 0.1.0, the `tar.gz` file can be recreated from a checkout of
the repository as follows:

{% highlight shell %}
$ git archive --format=tar --prefix=restic-0.1.0/ v0.1.0 | gzip -n > restic-0.1.0.tar.gz
{% endhighlight %}

Comparing the SHA-256 hash against the hash of the automatically generated file
shows that indeed the same file has been generated:

{% highlight shell %}
$ sha256sum restic-0.1.0.tar.gz
df7842cb690a56ce5371013a958d9f324072429897511d4bbfc092d76303f198  restic-0.1.0.tar.gz

$ curl -s -L https://github.com/restic/restic/archive/v0.1.0.tar.gz | sha256sum
df7842cb690a56ce5371013a958d9f324072429897511d4bbfc092d76303f198  -
{% endhighlight %}

### Signing and Verifying Release Files

The file generated via `git archive` as described above is then signed with GPG:

{% highlight shell %}
$ gpg --armor --detach-sign restic-0.1.0.tar.gz
{% endhighlight %}

And both files `restic-0.1.0.tar.gz` and `restic-0.1.0.tar.gz.asc` are uploaded
and attached to the release on GitHub.

When you download the source code and the signature from the [GitHub Releases
Page](https://github.com/restic/restic/releases), you can afterwards verify the
signature as follows (provided the necessary GPG key has been retrieved):

{% highlight shell %}
$ gpg --verify restic-0.1.0.tar.gz.asc 
gpg: assuming signed data in 'restic-0.1.0.tar.gz'
gpg: Signature made Mi 16 Sep 2015 23:33:11 CEST using RSA key ID D3F7A907
gpg: Good signature from "Alexander Neumann <alexander@bumpern.de>"
{% endhighlight %}

### Conclusion

While GitHub provides a great service, it's always a good idea to independently
verify the integrity of the source code before using it. Especially for package
maintainers of the different Linux distributions, this is very important. We've
seen that all releases of restic are tagged with as signed commit and in
addition a GPG signature is provided for the `tar.gz` files for each release.
