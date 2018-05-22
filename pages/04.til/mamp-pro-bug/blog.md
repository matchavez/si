---
title: 'MAMP Pro "Bug"'
published: true
date: '14:57 21-05-2018'
taxonomy:
    category:
        - tech
    tag:
        - webdev
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
content:
    items:
        - '@self.children'
    limit: 5
    order:
        by: date
        dir: desc
    pagination: true
    url_taxonomy_filters: true
---

Today I learned that in MAMP Pro, you can't click on the "show website" button unless you're running the SQL server. It doesn't matter if you don't need it. It's still working fine without it, but it is grayed out without SQL on.

Frankly, it's stupid. I'm guessing they believe it's better than running when it's off, but I disagree. It's poor design.