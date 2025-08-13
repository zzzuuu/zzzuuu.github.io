---
layout: page
title: Album2
permalink: /album2/
nav: false
---

<style>
  /* 1) 响应式网格 + 缩略图统一尺寸（兜底样式） */
  #album-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
    gap: 12px;
    align-items: start;
  }
  #album-grid a {
    display: block;
  }
  #album-grid img {
    width: 100%;
    height: 160px;          /* 控制缩略图高度 */
    object-fit: cover;      /* 居中裁剪，统一画面 */
    border-radius: 8px;
  }
</style>

<div id="album-grid">
  {% assign album_path = "assets/album1" %}
  {% for file in site.static_files %}
    {% if file.path contains album_path and file.extname != ".json" %}
      <a href="{{ file.path }}" data-lightbox="album" data-title="{{ file.name }}">
        <img src="{{ file.path }}" alt="{{ file.name }}" loading="lazy">
      </a>
    {% endif %}
  {% endfor %}
</div>

<!-- Lightbox2 -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/css/lightbox.min.css" rel="stylesheet"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/js/lightbox.min.js"></script>

<script>
  // 3) Lightbox2 配置 + 点击大图本体也能关闭
  (function () {
    function setupLightbox() {
      if (!window.lightbox) return;
      lightbox.option({
        wrapAround: true,
        showImageNumberLabel: false,
        fadeDuration: 150,
