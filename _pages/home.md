---
layout: splash
permalink: /
title: t-neumann.github.io
header:
excerpt:
---

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/laythe.png" alt="AWS Services">

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
