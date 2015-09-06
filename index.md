---
---

## Введение в Scala

{% for post in site.categories.intro reversed %}
  - [{{post.title}}]({{post.url}})
{% endfor %}

## Practical Scala

{% for post in site.categories.practical reversed %}
  - [{{post.title}}]({{post.url}})
{% endfor %}
