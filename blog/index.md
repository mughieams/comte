---
layout: default
title: "Blog"
description: I sometimes post articles of interest.
main: true
project-header: true
header-img: img/about.jpg
---

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
    {% if page.blog == true %}
        {% include post-list.html %}
    {% endif %}
{% endfor %}
</ul>