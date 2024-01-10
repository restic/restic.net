|Build Status|

This is the repository for the website content at `restic.net <https://restic.net>`__, generated using `Hugo <https://gohugo.io>`__.

**NOTE:** The `"extended" version of Hugo <https://gohugo.io/troubleshooting/faq/#i-get--this-feature-is-not-available-in-your-current-hugo-version>`__ must be used when building this website, as it uses SCSS for some parts of the styling. When downloading/installing Hugo, use the `"hugo-extended" package <https://github.com/gohugoio/hugo/releases>`__.

The generated website can be checked for dangling links with `muffet <https://github.com/raviqqe/muffet>`__:

    $ muffet https://restic.net

To create a new blog post, do the following:

- Run ``hugo new blog/my-new-post.md`` where ``my-new-post`` will be the part of the URL that you control (besides the date, which is taken from the frontmatter in the file) - please keep it lowercase only!
- Edit the newly created file ``contents/blog/my-new-post.md`` - adjust the title and date if needed, and add the content in MarkDown format (supports inline HTML).
- To make the post a draft or future publish it, see `here <https://gohugo.io/getting-started/usage/#draft-future-and-expired-content>`__.
- To embed an image in the post, place the image file in the ``static/blog/`` directory and reference it in the post file like this ``![My fancy image](/blog/my-fancy-image.png)`` (the text will be the alt/description of the image).

To re-generate the website, run ``hugo`` - this will run Hugo with the proper settings (configured in `hugo.toml`) and the website will be generated in the ``public/`` directory.

.. |Build Status| image:: https://github.com/restic/restic.net/actions/workflows/checks.yml/badge.svg
   :target: https://github.com/restic/restic.net/actions/workflows/checks.yml
