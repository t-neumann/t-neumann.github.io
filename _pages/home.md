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

<div class="card-columns">
{% for post in site.posts %}
  <div class="card">
  {% if post.image.path %}
    <div class="card-header bg-light"><img class="card-img-top img-fluid" src="{{ post.image.path }}" alt="{{ post.title }}" title="{{ post.title }}"></div>
  {% endif %}
    <div class="card-body">
  {% if post.title %}
      <a href="{{ post.url }}"><h4 class="card-title">{{ post.title }}</h4></a>
  {% endif %}
  {% if post.description %}
      <p class="card-text">{{ post.description }}</p>
  {% endif %}
  {% if post.url %}
      <p class="text-center"><a href="{{ post.url }}" class="btn btn-outline-primary">Read more</a></p>
  {% endif %}
    </div>
    <div class="card-footer">
  {% if post.date and post.tags %}
      <small class="text-muted"><i class="fa fa-calendar" aria-hidden="true"></i> {{ post.date | date: "%Y-%m-%d" }}
      {% if site.data.comments[post.slug] %}
        <div class="float-right">
          <i class="fa fa-comments" aria-hidden="true"> </i> <a href="{{ post.url }}#comments" data-proofer-ignore="true"><span class="badge badge-secondary">{{ site.data.comments[post.slug] | size }}</span></a>
        </div>
      {% endif %}
      </small>
      <br>
      {% assign tags = post.tags | sort %}
      <small class="text-muted"><i class="fa fa-tags" aria-hidden="true"></i>
      {% for tag in tags %}
        <a href="/tags#{{ tag | downcase }}" data-proofer-ignore="true">{{ tag }}</a>
        {% if forloop.last == false %} • {% endif %}
      {% endfor %}
      </small>
  {% else if post.date and !post.tags %}
      <small class="text-muted"><i class="fa fa-calendar" aria-hidden="true"></i> {{ post.date | date: "%Y-%m-%d" }}</small>
  {% else if !post.date and post.tags %}
    {% assign tags = post.tags | sort %}
    <small class="text-muted"><i class="fa fa-tags" aria-hidden="true"></i>
    {% for tag in tags %}
      <a href="/tags#{{ tag | downcase }}" data-proofer-ignore="true">{{ tag }}</a>
      {% if forloop.last == false %} • {% endif %}
    {% endfor %}
    </small>
  {% endif %}
    </div>
  </div>
{% endfor %}
</div>
<!--
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
--!>
