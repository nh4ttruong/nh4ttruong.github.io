---
layout: page
title: About
permalink: /about/
weight: 3
---

# **About me**

Hi I am **{{ site.author.name }}** :wave:, welcome to my space!<br>
The site just store my study and experirences on the University. Welcome to my site ^^<br>

<div class="row d-none">
{% include about/skills.html title="Programming Skills" source=site.data.programming-skills %}
{% include about/skills.html title="Other Skills" source=site.data.other-skills %}
</div>

<div class="row">
{% include about/timeline.html %}
</div>


