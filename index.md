---
title: Список блогов
---

## [Введение в Scala]({% post_url scala/intro/2015-06-10-setup %})

{% for post in site.posts reversed %}
  * [{{post.title}}]({{post.url}})
{% endfor %}
