---
layout: default
---

# Posts


<ul>
  {% for post in site.posts %}
    <li>
      {{ post.date | date: "%-d %B %Y" }} <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>


