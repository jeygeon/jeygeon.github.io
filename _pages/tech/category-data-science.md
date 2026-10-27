---
title: "Data Science"
layout: archive
permalink: categories/data-science
author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories['data-science']%} {% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
