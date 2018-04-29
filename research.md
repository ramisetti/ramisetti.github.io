---
layout: page
title: Research
subtitle: My Research Interests
---

<div class="container">
<div class="row">
<div class="col-xs-12 col-md-8">
<div class="row isotope projects-container js-layout-masonry">
{% assign research_pages = site.research | sort: 'nav_ord' %}
{% for item in research_pages %}

  <div class="col-xs-12 col-sm-6 col-md-4 col-lg-6 project-item isotope-item {{ item.title }} ">
  <div class="card">
   <!-- <a href="{{ item.url }}" title="" class="card-image hover-overlay">
           <img src="{{ item.bigimg }}" alt="" class="img-responsive">
          </a> -->
  <center><img src="{{ item.bigimg }}" alt="" class="img-responsive"></center>
  <div class="card-text">
  <p>{{ item.description }}</p>
  <!-- <h1><p><a href="{{ item.url }}" >{{ item.title }}</a></p></h1> -->
  {{ item.abstract }}
  </div>
  </div>
  </div>
{% endfor %}
</div>
</div>
 </div>
</div>