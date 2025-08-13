---
layout: page
title: Album3
permalink: /album3/
nav: true
nav_order: 4
---

{%- assign album_root = "assets/album1" -%}

<style>
  /* 兜底：即便 JS/CDN 失效，缩略图也不会巨大 */
  .jg a img {
    max-height: 180px;
    width: auto;
    height: auto;
    object-fit: cover;
    border-radius: 6px;
    display: inline-block;
    vertical-align: middle;
  }
  .jg { margin-bottom: 22px; }

  /* 分区标题样式（含前置分隔符） */
  .album-section-title {
    margin: 1.6rem 0 0.8rem;
    font-weight: 600;
  }

  /* 让 Justified Gallery 的 caption 固定显示在图片下方（不是悬浮叠盖） */
  .justified-gallery .caption {
    position: static !important;
    display: block !important;
    width: 100%;
    text-align: center;
    background: none;
    color: #666;
    font-size: 0.85rem;
    padding-top: 4px;
    white-space: normal;      /* 文件名换行 */
    word-break: break-word;   /* 避免很长的文件名溢出 */
  }
</style>

<div id="album-page">

  {%- comment -%}
  收集 album_root 下的所有图片
  {%- endcomment -%}
  {%- assign all_imgs = site.static_files
      | where_exp:"f","f.path contains album_root"
      | where_exp:"f","f.extname != '.json'"
      | where_exp:"f","f.extname != '.md'"
      | where_exp:"f","f.extname != '.txt'"
  -%}

  {%- comment -%}
  按子目录分组：/assets/album1/<group>/<file>
  path split: ["","assets","album1","<group>","file.jpg"]
  取 index 3 作为组名
  {%- endcomment -%}
  {%- assign groups = all_imgs | group_by_exp: "f", "f.path | split:'/' | slice:3,1 | first" -%}
  {%- assign sorted_groups = groups | sort: "name" -%}

  {%- for g in sorted_groups -%}
    {%- assign cat = g.name -%}
    {%- if cat and cat != "" -%}
      <h2 class="album-section-title">------- {{ cat | replace:"-"," " | replace:"_"," " | capitalize }}</h2>

      <div class="jg" data-album="{{ cat }}">
        {%- assign imgs = g.items | sort: "name" -%}
        {%- for f in imgs -%}
          {%- assign caption = f.name | remove: f.extname -%}
          <a href="{{ f.path }}" data-lightbox="{{ cat }}" data-title="{{ caption }}">
            <!-- alt/title 提供给 Justified Gallery 生成 caption 文本 -->
            <img src="{{ f.path }}" alt="{{ caption }}" title="{{ caption }}" loading="lazy">
          </a>
        {%- endfor -%}
      </div>
    {%- endif -%}
  {%- endfor -%}

</div>

<!-- 样式 -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/css/justifiedGallery.min.css"/>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/css/lightbox.min.css"/>

<!-- 脚本顺序：jQuery → Justified Gallery → Lightbox2 -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/js/jquery.justifiedGallery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/js/lightbox.min.js"></script>

<script>
  (function initAlbum3(){
    function run() {
      if (!window.jQuery || !jQuery.fn.justifiedGallery) return;

      // 初始化每个分区的 Justified Gallery（caption 固定展示来自 img[alt]）
      jQuery('.jg').each(function(){
        jQuery(this).justifiedGallery({
          rowHeight: 160,       // 缩略图行高（可调：140~200）
          maxRowHeight: 200,
          margins: 8,
          captions: true,       // 让 JG 使用 alt/title 生成 .caption
          lastRow: 'nojustify'
        });
      });

      // Lightbox2：基础参数 + 点击大图本体也可关闭
      if (window.lightbox) {
        lightbox.option({
          wrapAround: true,
          showImageNumberLabel: false,
          fadeDuration: 150,
          resizeDuration: 150,
          disableScrolling: false
        });
        document.addEventListener('click', function(e) {
          if (e.target && e.target.classList && e.target.classList.contains('lb-image')) {
            var overlay = document.querySelector('.lightboxOverlay');
            if (overlay) overlay.click();
          }
        });
      }
    }

    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', run);
    } else {
      run();
    }
  })();
</script>
