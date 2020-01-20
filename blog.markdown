---
layout: default
title: blog
---

<link rel="stylesheet" type="text/css" href="/assets/blog.css">

{%- for post in site.posts -%}
  {%- include post-preview.html title=post.title authors=post.authors
        date=post.date preview="" permalink=post.permalink -%}
{%- endfor -%}
