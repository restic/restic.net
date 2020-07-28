---
title: "Configuring GitHub Issue Templates"
date: 2018-09-09T00:00:00Z
---

Like many other Open Source projects, restic uses GitHub for hosting the source
code and managing issues. Over time, we discovered that oftentimes users open
an issue on GitHub which just consists of a question, for example on how to use
restic in a certain situation. While such issues can be answered quickly,
sometimes there's no consent on when the issue can be seen as resolved and
therefore closed (the issue is not actionable). The GitHub issue tracker is
then cluttered with issues for which there is no clear next action and no
clear "resolved" state. This blog post describes how we set up the issue
templates on GitHub to resolve this situation nicely.

A year ago in August 2017 we [decided to install]({{< relref "discourse-forum.md" >}})
a forum based on [Discourse](https://discourse.org) at
[https://forum.restic.net](https://forum.restic.net). It developed nicely and
over time attracted a stable base of core users and developers who are ready to
help each other and new restic users.

This leaves the question: how do users who have a question find the forum?
Sure, it's linked on the website, but that did not seem enough. Thankfully,
GitHub supports templates for all newly created issues, so we added a bit of
text which encourages users to use the forum if they are unsure:

> Welcome! If you have a question or are unsure if you should open an issue,
> please use the forum instead!
>
>    https://forum.restic.net
>
> The forum is a better place for questions about restic or general suggestions
> and topics, e.g. usage or documentation questions! This issue tracker is mainly
> for tracking bugs and feature requests directly relating to the development of
> the software itself, rather than the project.
>
> Thanks for understanding, and for contributing to the project!

We also added a few questions users should answer in order to make debugging
easier, but it turned out that the set of questions we ask for bugs is not
helpful when suggesting a new feature. We then discovered (almost
accidentally), that GitHub also [supports several different types](https://help.github.com/articles/about-issue-and-pull-request-templates/)
of issue templates, so now when you [create a new issue for restic](https://github.com/restic/restic/issues/new/choose),
you're greeted with the following dialogue:

![Opening a new Issue on GitHub](/blog/github-new-issue.png)

The "Bug report" and "Feature request" templates are stored in the files
`Bug.md` and `Feature.md` in the directory `.github/ISSUE_TEMPLATE` of the
restic repository. The headline and description can be set in the header
of the file, for example the ["Bug report" template](https://github.com/restic/restic/blob/master/.github/ISSUE_TEMPLATE/Bug.md) starts with the following
text:

```markdown
---
name: Bug report
about: Report a problem with restic to help us resolve it and improve
---

<!--

Welcome! - We kindly ask that you:

  1. Fill out the issue template below - not doing so needs a good reason.
  2. Use the forum if you have a question rather than a bug or feature request.

[...]
-->
```

The ["Feature request" template](https://github.com/restic/restic/blob/master/.github/ISSUE_TEMPLATE/Feature.md)
is very similar, just with other questions. We're using a combination of
Markdown headlines to structure the issue text, and HTML comments for a bit of
background explanation. The HTML comments are not shown when displaying an
issue in GitHub, so they don't distract from the content.

This leaves us with the third kind of issue users can create by clicking on the
link titled "Open a regular issue". For this, the file
[`.github/ISSUE_TEMPLATE.md`](https://github.com/restic/restic/blob/master/.github/ISSUE_TEMPLATE.md) is used.

This allows users to select the type of issue they would like to create, and in
all cases there's a hint to use the forum if they are unsure what to do. This
works very well for us.

If you're a user and submitted an issue before (or maybe you've been redirected
to the forum), please tell us about the experience? Did you like it? Was it
awkward? How do other projects handle nudging users into submitting
high-quality issues that are actionable? Please leave a comment in the forum
linked below this article!
