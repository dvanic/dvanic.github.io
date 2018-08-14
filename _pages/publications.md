---
title: publications
layout: bib_no_title
permalink: /publications
---

#### Journal articles

{% bibliography --query @article[status!=editorial && status!=other] %}

#### Conference presentations

{% bibliography --query @inproceedings[status!=editorial && status!=other] %}

#### PhD

{% bibliography --query @thesis %}
