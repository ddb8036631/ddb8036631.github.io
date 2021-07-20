---
title: "linux"
layout: archive
permalink: categories/linux
author_profile: true
sidebar_main: true
---

## Linux 관련 정리한 내용을 올리는 카테고리입니다.

{% assign posts = site.categories.linux%}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
