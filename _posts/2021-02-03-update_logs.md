---
title: 博客更新日志
date: 2021-02-03 00:00:00
updated: 2021-02-04 00:00:00
categories:
- site/logs
comments: false
---

记录博客的搭建、更新过程，以便学习和后期维护。

{% raw %}

### 2021-02-03

#### 搭建成功

使用[jekyll theme next](https://github.com/Simpleyyt/jekyll-theme-next.git)搭建完成

[配置参考](http://theme-next.simpleyyt.com/getting-started.html)

### 2021-02-04

#### 把标签加到post meta里，从而可以在首页显示

1. 修改了\_config.yml文件，在post\_meta里加上

    ```yaml
    post_meta:
      ...
      tags: true
    ```

2. 在\_data\languages\zh-Hans.yml的post里加上

    ```yaml
    post:
      ...
      tag: 标签
      ...
    ```

3. 修改\_includes\\\_macro\post.html文件，下面那一段删掉

    ```html
    {% if post.tags and post.tags.size != 0 and is_index == nil or is_index == false%}
              <div class="post-tags">
                {% for tag in post.tags %}
                  {% assign tag_url_encode = tag | url_encode | replace: '+', '%20' %}
    	          <a href="{{ '/tag/#/' | relative_url | append: tag_url_encode }}" rel="tag"><i class="fa fa-tag"></i> {{ tag }}</a>
                {% endfor %}
            </div>
            {% endif %}
    ```

    然后在<header class="post-header"\>里，找到块`{% if site.post_wordcount.wordcount or site.post_wordcount.min2read %}`，然后在其`{% endif %}`后面加上

    ```html
    {% if post.tags and post.tags.size !=0 and site.post_meta.tags %}
      <span class="post-category" >
        <br>
        <span class="post-meta-item-icon">
          <i class="fa fa-tag"></i>
        </span>
        {% if site.post_meta.item_text %}
          <span class="post-meta-item-text">{{ __.post.tag }}</span>
        {% endif %}
        {% for tag in post.tags %}
          {% assign tag_url_encode = tag | url_encode | replace: '+', '%20' %}
          <span itemprop="about" itemscope itemtype="http://schema.org/Thing">
            <a href="{{ '/tag/#/' | relative_url | append: tag_url_encode }}" itemprop="url" rel="tag">
              <span itemprop="name">{{ tag }}</span>
            </a>
          </span>
    
          {% assign tag_length = post.tags.size %}
          {% if tag_length > 1 and forloop.index != tag_length %}
            {{ __.symbol.comma }}
          {% endif %}
        {% endfor %}
      </span>
    {% endif %}
    ```

4. 删掉\_sass\\\_schemes\Mist\\\_posts-expanded.scss文件里的

    ```scss
    .post-tags {
        text-align: left;
        a {
          padding: 1px 5px;
          background: $whitesmoke;
          border-bottom: none;
        }
        a:hover { background: $grey-light; }
      }
    ```

    {% endraw %}