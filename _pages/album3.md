---
layout: page
title: Album3
permalink: /album3/
nav: true
---

<div id="mygallery">
  {% assign album_path = "assets/album1" %}
  {% for file in site.static_files %}
    {% if file.path contains album_path and file.extname != ".json" %}
      <a href="{{ file.path }}" data-lightbox="album" data-title="{{ file.name }}">
        <img src="{{ file.path }}" alt="{{ file.name }}">
      </a>
    {% endif %}
  {% endfor %}
</div>

<!-- 样式 -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/css/justifiedGallery.min.css"/>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/css/lightbox.min.css"/>

<!-- 脚本：先 jQuery，再 Justified Gallery，再 Lightbox2 -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/js/jquery.justifiedGallery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/js/lightbox.min.js"></script>

<script>
  document.addEventListener('DOMContentLoaded', function () {
    $("#mygallery").justifiedGallery({
      rowHeight: 200,
      margins: 6,
      captions: true
    });
  });
</script>
