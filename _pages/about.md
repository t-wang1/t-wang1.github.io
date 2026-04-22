---
permalink: /
title: ""
excerpt: ""
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

<h2 class="about-me-heading">About Me</h2>

I'm a senior at Stony Brook University studying Computer Science with a concentration in Artificial Intelligence and Data Science. I'm particularly interested in systems as they form the fundamental building blocks of our applications. I love understanding how things work under the hood and the elegant design decisions that go into building fast, reliable systems. 

I have experience building a wide range of projects, from database engines and full-stack web applications to AI-powered microservices. I'm constantly exploring new technologies and drawn to complex challenges that require balancing performance with correctness. 

Aside from coding, you can usually find me hunting for the best lattes in NYC, expanding my fragrance collection, or planning trips to explore new cities! 

<h2 class="projects-heading">Projects</h2>

<div class="project-grid">
  {% for project in site.projects %}
  <div class="project-card">
    <div class="project-card__body">
      <h3 class="project-card__title">{{ project.title }}</h3>
      <p class="project-card__excerpt">{{ project.excerpt }}</p>
      <div class="project-card__tags">
        {% for tag in project.tags %}
        <span class="project-tag">{{ tag }}</span>
        {% endfor %}
      </div>
      <div class="project-card__links">
        {% if project.github %}
        <a href="{{ project.github }}" target="_blank" aria-label="GitHub">
          <i class="fab fa-github"></i>
        </a>
        {% endif %}
      </div>
    </div>
  </div>
  {% endfor %}
</div>

<h2 class="technologies-heading">Technologies</h2>
<table class="tech-table">
  <tr>
    <td class="tech-table__label">Languages</td>
    <td class="tech-table__items">Python <span class="tech-dot">·</span> TypeScript <span class="tech-dot">·</span> JavaScript <span class="tech-dot">·</span> Java <span class="tech-dot">·</span> C <span class="tech-dot">·</span> C++ <span class="tech-dot">·</span> SQL <span class="tech-dot">·</span> Rust</td>
  </tr>
  <tr>
    <td class="tech-table__label">Technologies</td>
    <td class="tech-table__items">React <span class="tech-dot">·</span> Node.js <span class="tech-dot">·</span> Express/Next.js <span class="tech-dot">·</span> FastAPI <span class="tech-dot">·</span> Git <span class="tech-dot">·</span> Postman <span class="tech-dot">·</span> Docker <span class="tech-dot">·</span> CUDA <span class="tech-dot">·</span> MongoDB</td>
  </tr>
  <tr>
    <td class="tech-table__label">Libraries</td>
    <td class="tech-table__items">NumPy <span class="tech-dot">·</span> Pandas <span class="tech-dot">·</span> BeautifulSoup <span class="tech-dot">·</span> FAISS <span class="tech-dot">·</span> OpenCV <span class="tech-dot">·</span> MediaPipe <span class="tech-dot">·</span> Azure OpenAI</td>
  </tr>
</table>