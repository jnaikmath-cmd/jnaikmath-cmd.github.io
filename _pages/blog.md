---
layout: default
permalink: /blog/
title: blog
nav: true
nav_order: 1
pagination:
  enabled: true
  collection: posts
  permalink: /page/:num/
  per_page: 5
  sort_field: date
  sort_reverse: true
  trail:
    before: 1
    after: 3
---

<div class="post">

  {% assign blog_name_size = site.blog_name | size %}
  {% assign blog_description_size = site.blog_description | size %}

  {% if blog_name_size > 0 or blog_description_size > 0 %}
  <div class="header-bar text-center mb-5">
    <h1 class="display-4">{{ site.blog_name }}</h1>
    <p class="lead text-muted">{{ site.blog_description }}</p>
  </div>
  {% endif %}

  {% if site.display_tags.size > 0 or site.display_categories.size > 0 %}
  <div class="tag-category-list mb-4 text-center">
    <ul class="list-inline p-0 m-0">
      {% for tag in site.display_tags %}
        <li class="list-inline-item mx-2">
          <i class="fa-solid fa-hashtag fa-xs text-primary"></i> 
          <a href="{{ tag | slugify | prepend: '/blog/tag/' | relative_url }}" class="text-decoration-none">{{ tag }}</a>
        </li>
      {% endfor %}
      
      {% if site.display_categories.size > 0 and site.display_tags.size > 0 %}
        <li class="list-inline-item text-muted">|</li>
      {% endif %}

      {% for category in site.display_categories %}
        <li class="list-inline-item mx-2">
          <i class="fa-solid fa-tag fa-xs text-secondary"></i> 
          <a href="{{ category | slugify | prepend: '/blog/category/' | relative_url }}" class="text-decoration-none">{{ category }}</a>
        </li>
      {% endfor %}
    </ul>
  </div>
  <hr class="my-4">
  {% endif %}

  {% assign featured_posts = site.posts | where: "featured", "true" %}
  {% if featured_posts.size > 0 %}
  <div class="container featured-posts py-4">
    <h4 class="mb-4 font-weight-bold"><i class="fa-solid fa-star fa-xs"></i> Featured Posts</h4>
    {% assign is_even = featured_posts.size | modulo: 2 %}
    <div class="row row-cols-1 row-cols-md-2 g-4">
      {% for post in featured_posts %}
      <div class="col">
        <a href="{{ post.url | relative_url }}" class="text-decoration-none text-reset">
          <div class="card h-100 shadow-sm hoverable border-0 bg-light">
            <div class="card-body">
              <div class="float-end">
                <i class="fa-solid fa-thumbtack text-danger"></i>
              </div>
              <h5 class="card-title font-weight-bold">{{ post.title }}</h5>
              <p class="card-text small text-muted">{{ post.description | truncate: 120 }}</p>
              
              {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
              <div class="post-meta x-small text-muted mt-3">
                <span>{{ read_time }} min read</span> &nbsp;&middot;&nbsp; 
                <span>{{ post.date | date: '%b %d, %Y' }}</span>
              </div>
            </div>
          </div>
        </a>
      </div>
      {% endfor %}
    </div>
  </div>
  <hr class="my-5">
  {% endif %}

  <ul class="post-list list-unstyled mt-5">
    {% assign postlist = site.posts %}
    {% if page.pagination.enabled %}
      {% assign postlist = paginator.posts %}
    {% endif %}

    {% for post in postlist %}
    <li class="mb-5 pb-4 border-bottom">
      <div class="row align-items-center">
        <div class="{% if post.thumbnail %}col-md-9{% else %}col-12{% endif %}">
          <h3 class="mb-2">
            {% if post.redirect == blank %}
              <a class="post-title font-weight-bold text-decoration-none" href="{{ post.url | relative_url }}">{{ post.title }}</a>
            {% else %}
              <a class="post-title font-weight-bold text-decoration-none" href="{{ post.redirect }}" target="_blank">
                {{ post.title }} <i class="fa-solid fa-arrow-up-right-from-square fa-xs"></i>
              </a>
            {% endif %}
          </h3>
          <p class="text-muted mb-3">{{ post.description }}</p>
          
          <div class="post-meta small text-muted d-flex align-items-center flex-wrap">
            <span class="me-3"><i class="fa-solid fa-calendar-day me-1"></i> {{ post.date | date: '%B %d, %Y' }}</span>
            <span class="me-3"><i class="fa-solid fa-clock me-1"></i> {{ post.content | number_of_words | divided_by: 180 | plus: 1 }} min read</span>
            
            {% if post.tags.size > 0 %}
              <div class="mt-1">
                {% for tag in post.tags %}
                  <span class="badge rounded-pill bg-light text-dark border me-1">#{{ tag }}</span>
                {% endfor %}
              </div>
            {% endif %}
          </div>
        </div>

        {% if post.thumbnail %}
        <div class="col-md-3 mt-3 mt-md-0">
          <img src="{{ post.thumbnail | relative_url }}" class="img-fluid rounded shadow-sm" alt="{{ post.title }}" style="max-height: 150px; width: 100%; object-fit: cover;">
        </div>
        {% endif %}
      </div>
    </li>
    {% endfor %}
  </ul>

  {% if page.pagination.enabled %}
    {% include pagination.liquid %}
  {% endif %}

</div>