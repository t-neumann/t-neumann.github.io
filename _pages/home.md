---
layout: splash
permalink: /
header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/mm-home-page-feature.jpg
  actions:
    - label: "<i class='fas fa-download'></i> Install now"
      url: "/docs/quick-start-guide/"
excerpt: >
  SLA flexible two-column Jekyll theme. Perfect for building personal sites, blogs, and portfolios.<br />
  <small><a href="https://github.com/mmistakes/minimal-mistakes/releases/tag/4.14.1">Latest release v4.14.1</a></small>
---

<div class="feature__wrapper">

  {% for post in site.posts %}

    {% if post.url contains "://" %}
      {% capture f_url %}{{ post.url }}{% endcapture %}
    {% else %}
      {% capture f_url %}{{ post.url | relative_url }}{% endcapture %}
    {% endif %}

    <div class="feature__item">
      <div class="archive__item">
        <div class="archive__item-teaser">
          <img src=
            {% if post.header.teaser %}
              {% if post.header.teaser contains "://" %}
                "{{ post.header.teaser }}"
              {% else %}
                "{{ post.header.teaser | relative_url }}"
              {% endif %}
            {% else %}
              "assets/images/mm-customizable-feature.png"
            {% endif %}
          alt="{% if post.header.alt %}{{ post.header.alt }}{% endif %}">
          {% if post.header.image_caption %}
            <span class="archive__item-caption">{{ post.header.image_caption | markdownify | remove: "<p>" | remove: "</p>" }}</span>
          {% endif %}
        </div>

        <div class="archive__item-body">
          {% if post.title %}
            <h2 class="archive__item-title">{{ post.title }}</h2>
          {% endif %}

          {% if post.excerpt %}
            <div class="archive__item-excerpt">
              {{ post.excerpt | markdownify }}
            </div>
          {% endif %}

          {% if post.url %}
            <p><a href="{{ f_url }}" class="btn {{ post.header.btn_class | default: btn--primary }}">{{ post.header.btn_label | default: site.data.ui-text[site.locale].more_label | default: "Learn More" }}</a></p>
          {% endif %}
        </div>
      </div>
    </div>
  {% endfor %}

</div>
