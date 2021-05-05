---
title: "운영체제"
layout: archive
permalink: categories/os
author_profile: true
sidebar_main: true
---

## 운영체제 공부한 것을 올리는 카테고리입니다.

{% assign posts = site.categories.os %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}