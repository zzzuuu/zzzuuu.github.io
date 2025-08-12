---
layout: page
title: Album3
permalink: /album3/
nav: true
---

<!-- 给容器保留原来的 id，同时加上 class="jg" 以配合样式与初始化 -->
<div id="mygallery" class="jg">
  {% assign album_path = "assets/album1" %}
  {% for file in site.static_files %}
    {% if file.path contains album_path and file.extname != ".json" %}
      <a href="{{ file.path }}" data-lightbox="album" data-title="{{ file.name }}">
        <img src="{{ file.path }}" alt="{{ file.name }}" loading="lazy">
      </a>
    {% endif %}
  {% endfor %}
</div>

<!-- ② 兜底 CSS：即使 JS 未加载成功，缩略图也不会巨大 -->
<style>
  .jg a img {
    max-height: 180px;  /* 缩略图高度上限，可按需调整 120~200px */
    width: auto;
    height: auto;
    object-fit: cover;
    border-radius: 6px;
    display: inline-block;
    vertical-align: middle;
  }
  .jg { margin-bottom: 18px; }
</style>

<!-- 样式（顺序无硬性要求，但放在这里最直观） -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/css/justifiedGallery.min.css"/>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/css/lightbox.min.css"/>

<!-- ③ 脚本顺序：先 jQuery → 再 Justified Gallery → 再 Lightbox2 -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/js/jquery.justifiedGallery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/js/lightbox.min.js"></script>

<!-- 稳健初始化 + 更小的 rowHeight -->
<script>
  (function initGallery(){
    function run() {
      if (!window.jQuery || !jQuery.fn.justifiedGallery) return;

      // 初始化每个相册容器（这里只有一个 .jg，也兼容以后多分区）
      jQuery('.jg').each(function(){
        jQuery(this).justifiedGallery({
          rowHeight: 160,       // 调小缩略图行高（建议 140~200）
          maxRowHeight: 200,    // 行高上限，避免某些行过高
          margins: 6,
          captions: true,
          lastRow: 'nojustify'  // 最后一行不强制铺满，更自然
        });
      });

      // Lightbox2 可选项（可按需要微调）
      if (window.lightbox) {
        lightbox.option({
          wrapAround: true,
          showImageNumberLabel: false,
          fadeDuration: 150,
          resizeDuration: 150,
          disableScrolling: false
        });
      }

      // （可选小增强）点击大图本体也可关闭回到相册
      document.addEventListener('click', function(e) {
        if (e.target && e.target.classList && e.target.classList.contains('lb-image')) {
          var overlay = document.querySelector('.lightboxOverlay');
          if (overlay) overlay.click();
        }
      });
    }

    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', run);
    } else {
      run();
    }
  })();
</script>
