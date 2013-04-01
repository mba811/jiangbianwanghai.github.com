---
layout: page
title: 江边望海的博客!
tagline: 关注我：weibo.com/jiangbianwanghai
---
{% include JB/setup %}

{% for post in site.posts %}
<div>
<div class="span10"><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></div>
<div class="span2"><span>{{ post.date | date_to_string }}</span></div>
</div>
{% endfor %}