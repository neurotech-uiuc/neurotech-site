---
layout: default
title: members
---

<link rel="stylesheet" type="text/css" href="/assets/members.css">

<div class="members">
{%- for name in site.data.members["member_list"] -%}
    {%- include profile.html member_name=name -%}
{%- endfor -%}
</div>
