---
title: publications
layout: bib_no_title
css: assets/css/academicons.css
---

#### Journal articles

{% bibliography --query @article[status!=editorial && status!=other] %}

#### Conference presentations

{% bibliography --query @inproceedings[status!=editorial && status!=other] %}

#### PhD

{% bibliography --query @thesis %}
