---
title: "Operating System"
layout: archive
permalink: categories/os
author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories['os']%} {% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}