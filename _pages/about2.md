---
layout: about
title: About
permalink: /about2
# subtitle: <a href='#'>Affiliations</a>. Address. Contacts. Moto. Etc.

profile:
  align: right
  image: with_honey_glider.jpg #assets/img2024/about/with_honey_glider.jpg
  image_circular: false # crops the image to make it circular
  # more_info: >
  # <p>555 your office number</p>
  # <p>123 your address street</p>
  # <p>Your City, State 12345</p>

news: false # includes a list of news items
latest_posts: false # includes a list of the newest posts
selected_papers: true # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page
---

<!--![borneo cicada](assets/img/borneo_cicada.jpeg){:class="img-responsive"}{:height="200px"}-->
<!--{:width="25%"}-->

Hi, I'm **Yu Zeng** 曾昱, a biologist based in the University of South Florida at Tampa, Florida.

---

### Research

My research investigates the function and evolution of novel biomechanical systems (such as legs, wings, tails and tongues), trying to understand why they are efficient and how they are built, often at organism-environment interface and in predator-prey interactions.

- Flight, gliding, and controlled aerial behaviors (in insects and other arthropods)
- Feeding biomechanics (salamanders, chameleons, and tadpoles)
- Maneuverability (whole-organism, body parts, colonial)
- Biomaterial (genesis and evolution of hagfish slime thread)

---

### Education

I received my Ph.D. in Integrative Biology from University of California at Berkeley (advised by [Robert Dudley](https://berkeleyflightlab.org) & [David Wake](https://wakelab.berkeley.edu/)).

---

### Reviewed for

- Bioinspiration & Biomimetics
- Biotropica
- eLife
- Frontiers in Ecology & Evolution
- Integrative and Comparative Biology
- Integrative Zoology
- iScience
- Journal of Experimental Biology
- Journal of Insect Sciences
- Journal of Comparative Physiology
- PeerJ
- Physical Review Letters
- PLOS ONE
- Scientific Reports
- Zoomorphology

---

<h2>News</h2>
<ul class="news-list">
  {% assign recent_news = site.news | sort: "date" | reverse | slice: 0, 5 %}
  {% for post in recent_news %}
    <li>
      {% if post.external_url %}
        <a href="{{ post.external_url }}" target="_blank" rel="noopener noreferrer">{{ post.title }}</a>
      {% else %}
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      {% endif %}
      <span class="date"> — {{ post.date | date: "%Y-%m-%d" }}</span>
      {% if post.summary %}
        <div class="summary">{{ post.summary }}</div>
      {% endif %}
    </li>
  {% endfor %}
</ul>
<p><a href="{{ '/news/' | relative_url }}">More news →</a></p>

<!--
<h2>News</h2>
<ul class="news-list">
{% assign recent_news = site.news | where_exp: "p", "p.categories contains 'news'" | sort: "date" | reverse | slice: 0, 5 %}
  {% for post in recent_news %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span class="date"> — {{ post.date | date: "%Y-%m-%d" }}</span>
      {% if post.summary %}
        <div class="summary">{{ post.summary }}</div>
      {% endif %}
    </li>
  {% endfor %}
</ul>
<p><a href="{{ '/news/' | relative_url }}">More news →</a></p>
-->

<!--
### Email
zeng @ berkeley.edu
dreavoniz @ berkeley.edu
yuzeng @ usf.edu
(Note: yzeng7 @ ucmerced.edu is defunct)  -->

---

<!--How to pronounce my name? My first name Yu ([昱](https://chinese.yabla.com/chinese-english-pinyin-dictionary.php?define=%E6%98%B1)): it has a final "[-ü](https://resources.allsetlearning.com/chinese/pronunciation/-%C3%BC)", see details [here](https://resources.allsetlearning.com/chinese/pronunciation/Yu) and [this YouTube video](https://www.youtube.com/watch?v=XwG_jp42GhA).-->

<!-- > Write  **about** yourself. Link to your favorite [subreddit](http://reddit.com). You can put a picture in, too. The code is already in, just name your picture `prof_pic.jpg` and put it in the `img/` folder. -->

<!--more things-->

<!-- Put your address / P.O. box / other info right below your picture. You can also disable any of these elements by editing `profile` property of the YAML header of your `_pages/about.md`. Edit `_bibliography/papers.bib` and Jekyll will render your [publications page](/al-folio/publications/) automatically.-->

<!-- Link to your social media connections, too. This theme is set up to use [Font Awesome icons](https://fontawesome.com/) and [Academicons](https://jpswalsh.github.io/academicons/), like the ones below. Add your Facebook, Twitter, LinkedIn, Google Scholar, or just disable all of them.-->
