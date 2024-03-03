---
layout: page
title: About
permalink: /about/
weight: 3
---

## **About me**

Hi there,<br>
<br>
Welcome you guys to my site, and I'm **{{ site.author.name }}** :wave:!<br>
<br>

<div class="row d-none">
{% include about/skills.html title="Programming Skills" source=site.data.programming-skills %}
{% include about/skills.html title="Other Skills" source=site.data.other-skills %}
</div>

<div class="row">
{% include about/timeline.html %}
</div>


