---
title: "Davide Modesto"
layout: default
---

<div style="display: flex; justify-content: space-between; align-items: start; margin-bottom: 3em;">
  <div style="padding-right: 2em; flex: 1;">
    <p style="margin-bottom: 1.5em;">Master's student in Mathematics.</p>
    <p style="line-height: 1.8;">
      Email: modesto.davide[at]gmail.com<br>
      GitHub: <a href="https://github.com/ElModdy">ElModdy</a><br>
      You can find my <a href="/assets/my_cv.pdf">Curriculum Vitae</a> here.
    </p>
  </div>
  <img src="/assets/picture.png" alt="Davide" style="width: 160px; height: auto; border: 1px solid #ddd; padding: 5px; border-radius: 2px;">
</div>

# Posts

<ul>
  {% for post in site.posts %}
    <li>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
