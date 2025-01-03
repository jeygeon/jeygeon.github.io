---
title: "Data Structure"
layout: archive
permalink: categories/data-structure
author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories['data-structure']%} {% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}