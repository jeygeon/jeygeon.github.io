---
title: "WEB"
layout: archive
permalink: categories/web
author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories['web']%} {% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}