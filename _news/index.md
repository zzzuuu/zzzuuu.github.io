---
layout: page
title: News
permalink: /news/
---

<h1>All News</h1>
<ul class="news-archive">
  {% assign all_news = site.posts | where_exp: "p", "p.categories contains 'news' 'sample-posts" | sort: "date" | reverse %}
  {% for post in all_news %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span class="date"> — {{ post.date | date: "%Y-%m-%d" }}</span>
      {% if post.summary %}
        <div class="summary">{{ post.summary }}</div>
      {% endif %}
    </li>
  {% endfor %}
</ul>
