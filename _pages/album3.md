---
layout: page
title: Album3
permalink: /album3/
nav: false
---

<style>
  /* 兜底缩略图样式（即使 JS 加载失败也不至于巨大） */
  .jg a img {
    max-height: 180px;
    width: auto;
    height: auto;
    object-fit: cover;
    border-radius: 6px;
    display: inline-block;
    vertical-align: middle;
  }
  .jg { margin-bottom: 18px; }

  /* 子目录分隔符与标题样式 */
  .album-section-sep {
    border: 0;
    border-top: 1px solid var(--global-divider-color, #e5e7eb);
    margin: 28px 0 12px 0;
  }
  .album-section-title {
    margin: 0 0 10px 0;
    font-size: 1rem;
    font-weight: 300;
  }
</style>

{%- comment -%}
收集 album 根下所有图片（assets/album1/*）
{%- endcomment -%}
{%- assign album_root = "assets/album1" -%}
{%- assign all_imgs = site.static_files
  | where_exp:"f","f.path contains album_root"
  | where_exp:"f","f.extname != '.json'"
  | where_exp:"f","f.extname != '.md'"
  | where_exp:"f","f.extname != '.txt'"
-%}

{%- comment -%}
区分：主目录图片（/assets/album1/xxx.jpg） 与 子目录图片（/assets/album1/<subdir>/xxx.jpg）
{%- endcomment -%}
{%- assign root_imgs = all_imgs | where_exp:"f","(f.path | split:'/' | size) == 4" -%}
{%- assign subdir_imgs = all_imgs | where_exp:"f","(f.path | split:'/' | size) >= 5" -%}

{%- comment -%}
先渲染主目录图片（缩略图不显示文件名：alt 置空；Lightbox 中仍显示 data-title）
{%- endcomment -%}
{%- if root_imgs and root_imgs.size > 0 -%}
<div id="album-root" class="jg">
  {%- assign imgs_sorted = root_imgs | sort: "name" -%}
  {%- for f in imgs_sorted -%}
    {%- assign base = f.name | split: "." | first -%}
    <a href="{{ f.path }}" data-lightbox="album" data-title="{{ base }}">
      <img src="{{ f.path }}" alt="" loading="lazy">
    </a>
  {%- endfor -%}
</div>
{%- endif -%}

{%- comment -%}
对子目录分组渲染：缩略图显示文件名（去后缀），Lightbox 也同名
{%- endcomment -%}
{%- assign groups = subdir_imgs | group_by_exp: "f", "f.path | split:'/' | slice:3,1 | first" -%}
{%- assign sorted_groups = groups | sort: "name" -%}

{%- for g in sorted_groups -%}
  {%- assign cat = g.name -%}
  {%- if cat and cat != "" -%}
    <hr class="album-section-sep">
    <h2 class="album-section-title">
      {{ cat | replace:"-"," " | replace:"_"," " | capitalize }}
    </h2>
    <div class="jg" data-album="{{ cat }}">
      {%- assign imgs = g.items | sort: "name" -%}
      {%- for f in imgs -%}
        {%- assign base = f.name | split: "." | first -%}
        <a href="{{ f.path }}" data-lightbox="{{ cat }}" data-title="{{ base }}">
          <img src="{{ f.path }}" alt="{{ base }}" loading="lazy">
        </a>
      {%- endfor -%}
    </div>
  {%- endif -%}
{%- endfor -%}

<!-- 依赖与脚本（Justified Gallery + Lightbox2） -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/css/justifiedGallery.min.css"/>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/css/lightbox.min.css"/>

<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/justifiedGallery/3.8.1/js/jquery.justifiedGallery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/js/lightbox.min.js"></script>

<!-- 放在 Lightbox2 CSS 之后，确保覆盖颜色：caption/number/链接全为白色 -->
<style>
  .lightbox .lb-data .lb-caption,
  .lb-data .lb-caption,
  .lb-data .lb-number,
  .lb-data .lb-caption a {
    color: #fff !important;
    text-shadow: 0 1px 2px rgba(0,0,0,0.6);
  }
</style>

<script>
  (function initAlbum() {
    function run() {
      if (!window.jQuery || !jQuery.fn.justifiedGallery) return;

      // 初始化每个相册容器（主目录 + 各子目录）
      jQuery('.jg').each(function(){
        jQuery(this).justifiedGallery({
          rowHeight: 160,
          maxRowHeight: 200,
          margins: 6,
          captions: true,
          captionsData: 'alt',    // 只用 <img alt> 作为标题来源
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

      // 点击大图本体关闭
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
