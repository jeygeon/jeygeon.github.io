---
title: "Blog"
layout: archive
permalink: /blog
author_profile: true
sidebar:
  nav: "sidebar-category"
---


{% assign posts = site.categories.categories %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}