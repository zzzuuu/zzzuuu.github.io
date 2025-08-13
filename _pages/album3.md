---
layout: page
title: Album3
permalink: /album3/
nav: true
---

<div id="album-page">
  {%- assign album_root = "assets/album1" -%}

  {%- comment -%}
  收集 album 根下所有图片文件
  {%- endcomment -%}
  {%- assign all_imgs = site.static_files
    | where_exp:"f","f.path contains album_root"
    | where_exp:"f","f.extname != '.json'"
    | where_exp:"f","f.extname != '.md'"
    | where_exp:"f","f.extname != '.txt'"
  -%}

  {%- comment -%}
  按子目录分组：/assets/album1/<group>/<file>
  path split 示例：["","assets","album1","animals","frog.jpg"]
  取 index 3 作为组名
  {%- endcomment -%}
  {%- assign groups = all_imgs | group_by_exp: "f", "f.path | split:'/' | slice:3,1 | first" -%}
  {%- assign sorted_groups = groups | sort: "name" -%}

  {%- for g in sorted_groups -%}
    {%- assign cat = g.name -%}
    {%- if cat and cat != "" -%}
      <h2 class="album-section-title">{{ cat | replace:"-"," " | replace:"_"," " | capitalize }}</h2>
      <div class="jg" data-album="{{ cat }}">
        {%- assign imgs = g.items | sort: "name" -%}
        {%- for f in imgs -%}
          {%- assign base = f.name | split:'.' | first -%}
          <a href="{{ f.path }}" data-lightbox="{{ cat }}" data-title="{{ base }}">
            <img src="{{ f.path }}" alt="{{ base }}" loading="lazy">
          </a>
        {%- endfor -%}
      </div>
    {%- endif -%}
  {%- endfor -%}
</div>

<!-- 兜底 CSS：即使 JS 未加载成功，缩略图也不会巨大 -->
<style>
  .album-section-title {
    margin: 1.75rem 0 0.75rem;
    font-size: 1.35rem;
  }
  /* 相册缩略图兜底样式（在 JS 生效前/失败时生效） */
  .jg a img {
    max-height: 180px;   /* 统一缩略图高度上限，可按需调整 120~200px */
    width: auto;
    height: auto;
    object-fit: cover;
    border-radius: 6px;
    display: inline-block;
    vertical-align: middle;
  }
  .jg { margin-bottom: 18px; }
</style>

<!-- 样式 -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/css/justifiedGallery.min.css">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/css/lightbox.min.css">

<!-- 脚本顺序：先 jQuery → Justified Gallery → Lightbox2 -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/js/jquery.justifiedGallery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/js/lightbox.min.js"></script>

<script>
  (function initAlbum3(){
    function run() {
      if (!window.jQuery || !jQuery.fn.justifiedGallery) return;

      // 初始化每个分区的 Justified Gallery
      jQuery('.jg').each(function(){
        jQuery(this).justifiedGallery({
          rowHeight: 160,       // 缩略图行高（建议 140~200）
          maxRowHeight: 200,    // 行高上限
          margins: 6,
          captions: true,       // 显示 caption（使用 <img> 的 alt）
          lastRow: 'nojustify'
        });
      });

      // Lightbox2 设置
      if (window.lightbox) {
        lightbox.option({
          wrapAround: true,
          showImageNumberLabel: false,
          fadeDuration: 150,
          resizeDuration: 150,
          disableScrolling: false
        });
      }

      // 点击大图本体也可关闭回到相册
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
