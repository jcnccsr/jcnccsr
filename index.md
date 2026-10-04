---
layout: welcome
cover: true
title: Welcome
description: >
  A personal dumping site for notes and thoughts about the questions of the meaning of life, the universe, and everything.
---

  I'm learning how computers work, one small piece at a time. This site is where I keep track of what I discover along the way.
{:.lead}

## Where should I start ?

If you just found this place, have a look through the [Notes][notes]{:.heading.flip-title}. They're arranged (roughly) in the order I encountered things, so you can wander through them from the beginning or you can just jump straight into something you find interesting (if there's any).


## Currently exploring

- C
- Git and GitHub
- Operating systems
- How computers actually work
- Pointers, memory, and data structures
- Whatever interesting rabbit hole comes next

<div class="recent-posts">
  {% for post in site.posts limit:3 %}
    <article>
      <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
      <p class="faded">{{ post.date | date: "%B %d, %Y" }}</p>
      <p>{{ post.description }}</p>
    </article>
  {% endfor %}
</div>


## Elsewhere

I'm learning my way toward software development, and this site is where I keep track of the journey.

I'm always happy to talk about programming, computers, automation, farming, or whatever interesting problem happens to come along.

If you'd like to know more about me, have a look at the [about page]({{ '/about/' | relative_url }}) or [resume]({{ '/components/resume/' | relative_url }}). You can also find me on [LinkedIn](https://www.linkedin.com/in/jcnccsr).

<-->

[notes]:  {{ '/notes/' | relative_url }}