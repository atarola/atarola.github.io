---
layout: default
title: atarola.dev - Home
---

## Hello

Learning new tools and technologies is awesome, and so is getting stuff done for users.

{% for post in site.posts limit:5 %}
<article>
    <div class="title">
        <h2>{{ post.title }}</h2>
        <span class="subtitle">
            {{ post.date | date_to_long_string }} -
            tags: [{{ post.tags | join: ", "  }}]
        </span>
    </div>

    {{ post.excerpt }}

    <p>
        <a href="{{ post.url }}">[Continue Reading]</a>
    </p>
</article>
{% endfor %}
