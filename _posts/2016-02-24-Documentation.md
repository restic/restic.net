---
layout: post
title: "Documentation and Manual"
---

Documentation in software projects is one of the things most developers hate.
It is no secret that the docs always seem to be out of date.

In order to improve this situation for restic, since a [few days
ago](https://github.com/restic/restic/commit/eaa2f899d5f7c1d6e21d058cd5f5b6dc65eb45e6)
the manual and documentation is part of the main restic repository. Having the
documentation in the main repository means that all users have instant access
to the exact version of the documentation they'll need and developers can
document new features in the same Pull Request they're submitting the code.

We chose [MkDocs](http://www.mkdocs.org/) so we can easily write the
documentation in Markdown and locally preview it before publishing. The
documentation can be viewed locally by running the following command from
within the repository:

{% highlight shell %}
$ mkdocs serve
INFO    -  Building documentation... 
INFO    -  Cleaning site directory 
[I 160224 21:04:49 server:271] Serving on http://127.0.0.1:8000
[I 160224 21:04:49 handlers:58] Start watching changes
[I 160224 21:04:49 handlers:60] Start detecting changes
{% endhighlight %}

Afterwards just visit the printed URL. If you pass the additional parameter
`--livereload`, the browser will automatically refresh after a files was
changed. Of course you can also just view the Markdown files in a text editor,
without any third-party software. We'll always try to make the documentation
readable in plain text.

Additionally we've enabled the awesome
[Read the Docs](https://readthedocs.org/) service, which allows reading the
documentation at <https://restic.readthedocs.io/>. In the bottom left corner
is a small menu which allows switching to a specific version of the
documentation. The URL <https://restic.readthedocs.io/en/latest/> will always
return the docs for the latest master branch.
