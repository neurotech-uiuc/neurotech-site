---
layout: default
title: members
---

<link rel="stylesheet" type="text/css" href="/assets/members.css">

<div class="members">
{%- assign list = site.data.members["member_list"] | sort -%}
{%- for name in list -%}
    {%- include profile.html member_name=name -%}
{%- endfor -%}
</div>
