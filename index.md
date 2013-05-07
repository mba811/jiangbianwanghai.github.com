---
layout: page
title: 江边望海的博客!
tagline: 久在江边站，必有望海心
---
{% include JB/setup %}

{% for post in site.posts %}
<div>
<div class="span10"><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></div>
<div class="span2"><span>{{ post.date | date: "%Y-%m-%d" }}</span></div>
</div>
{% endfor %}