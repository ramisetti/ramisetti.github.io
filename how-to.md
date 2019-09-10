---
layout: page
title: How-to
subtitle: Know how-to..
---
<div class="list-filters">
  <a href="/how-to" class="list-filter filter-selected">All articles</a>
  <a href="/how-to/compile" class="list-filter">Build codes</a>
  <a href="/how-to/linux" class="list-filter">Install/Use Linux tools</a>
  <!--<a href="/tags" class="list-filter">Index</a>-->
</div>

<div class="how-to">
{% assign mCollection = "how-to" %}
{% assign grpNAME = "all" %}
{% include how-to.html %}
</div>