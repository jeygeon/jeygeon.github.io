---
title: "Java"
layout: archive
permalink: categories/java
author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories['java']%} {% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}