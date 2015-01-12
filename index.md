---
layout: page
title: A few words about PostgreSQL 
tagline: 
---

{% for post in site.posts %}
<article class="home">


  <h1>
     <a href="{{ site.BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
  </h1>
  <p> {{ post.date | date: "%d/%m/%Y" }}</p>
  <div>
    {% if post.fullview %}
    {{ post.content }}
    {% else %}
    {{ post.excerpt }}
    {% endif %}
  </div>

  <a href="{{ site.BASE_PATH }}{{ post.url }}">&rarr; Read more...</a>

</article>


{% endfor %}





