---
title: 'Markdown Tables in Sphinx'
---

When using Sphinx for documentation, you can make it do Markdown instead of just rST. Unfortunately, the CommonMark library doesn't do GFM tables. It turns out that if you install [https://pypi.org/project/sphinx-markdown-tables/](https://pypi.org/project/sphinx-markdown-tables/) you can actually use GFM tables and get them properly rendered.

Tables in rST are more robust, but if you write in markdown, and rely on rendering the way it does in Github, you need tables. Now, you can have them.

You can see how I integrate this into a quick install in [Documentation Tools](/tech/documentation-tools)