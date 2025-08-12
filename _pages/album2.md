---
layout: page
title: Album
permalink: /album2/
nav: true
---

<div id="album-grid">
  {% assign album_path = "assets/album1" %}
  {% for file in site.static_files %}
    {% if file.path contains album_path and file.extname != ".json" %}
      <a href="{{ file.path }}" data-lightbox="album" data-title="{{ file.name }}">
        <img src="{{ file.path }}" alt="{{ file.name }}" loading="lazy" style="max-width:220px;margin:6px;border-radius:8px;">
      </a>
    {% endif %}
  {% endfor %}
</div>

<!-- Lightbox2（al-folio 的示例也用到了它） -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/css/lightbox.min.css" rel="stylesheet"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/js/lightbox.min.js"></script>
