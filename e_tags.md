---
layout: page
title: Теги
icon: fa fa-tags
permalink: /tags/
---
<!--
<ul class="tags-box">
{% if site.posts != empty %}
{% for tag in site.tags %}
<a href="#{{ tag[0] }}" title="{{ tag[0] }}" rel="{{ tag[1].size }}">{{ tag[0] }}<span class="size"> {{ tag[1].size }}</span></a>
{% endfor %}
</ul>

<ul class="tags-box">
{% for tag in site.tags %}
    <li style="font-size: {{ tag | last | size | times: 100 | divided_by: site.categories.size | plus: 70 }}%">
        <a href="/{{ tag | first | slugize }}/">
            {{ tag | first }}<span class="size"> {{ tag[1].size }}</span>
        </a>
    </li>
{% endfor %}
</ul>
-->

<ul class="tags-box">
{% for tag in site.tags %}
    <a  class="tag"
            style="font-size: {{ tag | last | size | times: 100 | divided_by: site.categories.size | plus: 50 }}%"
            href="#{{ tag[0] }}" title="{{ tag[0] }}" rel="{{ tag[1].size }}"
        >{{ tag | first }} <span class="badge">{{ tag | last | size }}</span><span class="size"> {{ tag[1].size }}</span></a>
{% endfor %}
</ul>

<ul class="tags-box">
{% for tag in site.tags %}
<li  id="{{ tag[0] }}">{{ tag[0] }}</li>
{% for post in tag[1] %}
<time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time> &raquo;
<a href="{{ site.baseurl }}{{ post.url }}" title="{{ post.title }}"><i class="{{ post.icon }}"> </i> {{ post.title }}</a><br />
{% endfor %}
{% endfor %}
{% else %}
<span>No posts</span>
{% endif %}
</ul>
