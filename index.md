---
layout: default
---

## 欢迎！

> 我的博客首页，慢慢维护起来。记录一些开发规范、技术笔记和踩坑记录。

## 全部文章

{% if site.posts.size == 0 %}
还没有文章，敬请期待。
{% else %}
<ul>
  {% for post in site.posts %}
  <li>
    <small>{{ post.date | date: "%Y-%m-%d" }}</small> ·
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </li>
  {% endfor %}
</ul>
{% endif %}

## 分类

<ul>
  {% assign cats = site.categories | sort %}
  {% for cat in cats %}
  <li>{{ cat[0] }}（{{ cat[1] | size }}）</li>
  {% endfor %}
</ul>
