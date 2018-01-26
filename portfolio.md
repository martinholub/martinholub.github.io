---
layout: page
title: projects
permalink: /portfolio/
---
{% assign shuffled_portfolio = site.portfolio | sample: 6 %}
{% for project in shuffled_portfolio %}

{% if project.redirect %}
<div class="project">
    <div class="thumbnail">
        <a href="{{ project.redirect }}" target="_blank">
        {% if project.img %}
        <img class="thumbnail" src="{{ project.img }}"/>
        {% else %}
        <div class="thumbnail blankbox"></div>
        {% endif %}
        <span>
            <h1>{{ project.title }}</h1>
            <br/>
            <p>{{ project.description }}</p>
        </span>
        </a>
    </div>
</div>
{% else %}

<div class="project ">
    <div class="thumbnail">
        <a href="{{ site.baseurl }}{{ project.url }}">
        {% if project.img %}
        <img class="thumbnail" src="{{ project.img }}"/>
        {% else %}
        <div class="thumbnail blankbox"></div>
        {% endif %}
        <span>
            <h1>{{ project.title }}</h1>
            <br/>
            <p>{{ project.description }}</p>
        </span>
        </a>
    </div>
</div>

{% endif %}

{% endfor %}
<br/>
<hr/>
<br/>
<span class="contacticon center">
	<a href="mailto:martin-holub@outlook.com"><i class="fa fa-envelope-square"></i></a>
	<a href="https://twitter.com/holub_martin" target="_blank"><i class="fa fa-twitter-square"></i></a>
	<a href="https://www.linkedin.com/in/holubmartin" target="_blank"><i class="fa fa-linkedin-square"></i></a>
	<a href="https://www.researchgate.net/profile/Martin_Holub2" target="_blank"><i class="ai ai-researchgate-square"></i></a>
	<a href="https://github.com/martinholub" target="_blank"><i class="fa fa-github-square"></i></a>
	<a href="/feed.xml" target="_blank"><i class="fa fa-rss-square"></i></a>
</span>
