---
layout: page
permalink: /outputs/
title: Outputs
description: Here you can find a list of outputs related to AI auditing from the aiaudit.org network or its contributors.
years: [2021, 2020]
nav: false
nav-order: b
---
# Standardization
<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f standards -q @*[year={{y}}]* %}
{% endfor %}

</div>

# Conferences and journals
<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>

# Preprints
<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f preprints -q @*[year={{y}}]* %}
{% endfor %}

</div>
