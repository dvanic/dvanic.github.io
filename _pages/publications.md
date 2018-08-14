---
title: publications
layout: page_no_title
css: assets/css/academicons.css
---

<!-- My [Google Scholar](https://scholar.google.com/citations?user=rqfVsosAAAAJ) page.
 -->
<div style="text-align: right">
<h4 id="academic">My academic profiles</h4> <a target="_blank" href="https://scholar.google.com/citations?user=rTMDV6UAAAAJ&hl"><span class="ai ai-google-scholar ai-lg" style="color:#00B4A1" aria-hidden="true"></span></a> <a target="_blank" href="https://www.researchgate.net/profile/Darya_Vanichkina"><span class="ai ai-researchgate ai-lg" style="color:#00B4A1" aria-hidden="true"></span></a> <a target="_blank" href="https://www.biorxiv.org/search/author1%3ADarya%2BVanichkina"><span class="ai ai-biorxiv ai-lg" style="color:#00B4A1" aria-hidden="true"></span></a> <a target="_blank" href="https://www.ncbi.nlm.nih.gov/pubmed/?term=Vanichkina%20DP"><span class="ai ai-pubmed ai-lg" style="color:#00B4A1" aria-hidden="true"></span></a>
</div>

#### Journal articles

{% bibliography --query @article[status!=editorial && status!=other] %}

#### Conference presentations

{% bibliography --query @inproceedings[status!=editorial && status!=other] %}

#### PhD

{% bibliography --query @thesis %}
