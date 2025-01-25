---
layout: home
author_profile: true
---

Here you can find my thoughts, experiences, and insights on [your topics of interest].

## Recent Posts
{% for post in site.posts limit:5 %}
  <article>
    <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
    <time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date_to_string }}</time>
    {{ post.excerpt }}
  </article>
{% endfor %}
