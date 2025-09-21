---
layout: page
title: Album3
permalink: /album3/
nav: false
---

<style>
  /* 兜底：即便 JS 未加载，也保证缩略图不巨大 */
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

  /* 分隔符样式（整行） */
  .section-sep {
    border: none;
    border-top: 1px solid #e5e7eb; /* Tailwind slate-200 风格 */
    margin: 2rem 0 1rem 0;
    width: 100%;
  }

  /* 分区标题样式 */
  .album-heading {
    margin: 0 0 0.75rem 0;
    font-size: 1.2rem;
    font-weight: 300;
  }

  /* 让 Justified Gallery 的 caption 永久可见（而非仅悬停时） */
  .justified-gallery > a .caption {
    opacity: 1 !important;
    background: rgba(0,0,0,0.2);
    padding: 3px 6px;
    font-size: 2rem;
  }
</style>

<div id="album-page">
  {%- assign album_root = "assets/album1" -%}

{%- assign all_imgs = site.static_files
    | where_exp:"f","f.path contains album_root"
    | where_exp:"f","f.extname == '.jpg' or f.extname == '.jpeg' or f.extname == '.png' or f.extname == '.gif'"
  -%}

{%- comment -%}
分组：/assets/album1/<cat>/<file>
e.g. path split: ["", "assets", "album1", "<cat>", "file.jpg"]
取 index 3 为组名
{%- endcomment -%}
{%- assign groups = all_imgs | group_by_exp: "f", "f.path | split:'/' | slice:3,1 | first" -%}
{%- assign sorted_groups = groups | sort: "name" -%}

{%- for g in sorted_groups -%}
{%- assign cat = g.name -%}
{%- if cat and cat != "" -%}

<hr class="section-sep">
<h2 class="album-heading">
{{ cat | replace:"-"," " | replace:"_"," " | capitalize }}
</h2>

      <div class="jg" data-album="{{ cat }}">
        {%- assign imgs = g.items | sort: "name" -%}
        {%- for f in imgs -%}
          {%- assign basename = f.name | remove: f.extname -%}
          <a href="{{ f.path }}" data-lightbox="{{ cat }}" data-title="{{ basename }}">
            <img src="{{ f.path }}" alt="{{ basename }}" loading="lazy">
          </a>
        {%- endfor -%}
      </div>
    {%- endif -%}

{%- endfor -%}

</div>

<!-- 样式 -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/css/justifiedGallery.min.css">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/css/lightbox.min.css">

<!-- 脚本：先 jQuery，再 Justified Gallery，再 Lightbox2 -->
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
          rowHeight: 160,       // 缩略图行高（可改 140~200）
          maxRowHeight: 200,
          margins: 6,
          captions: true,       // 读取 <img alt> 作为标题
          lastRow: 'nojustify'
        });
      });

      // Lightbox2 配置：显示去后缀的文件名；允许点击大图关闭
      if (window.lightbox) {
        lightbox.option({
          wrapAround: true,
          showImageNumberLabel: false,
          fadeDuration: 150,
          resizeDuration: 150,
          disableScrolling: false
        });

        // 点击大图本体也可关闭
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
