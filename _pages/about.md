---
permalink: /
title: "About Me"
excerpt: "About me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

I'm a senior at Stony Brook University studying Computer Science with a concentration in Artificial Intelligence and Data Science. I'm particularly interested in systems as they form the fundamental building blocks of our applications. I love understanding how things work under the hood and the elegant design decisions that go into building fast, reliable systems. 

I have experience building a wide range of projects, from database engines and full-stack web applications to AI-powered microservices. I'm constantly exploring new technologies and drawn to complex challenges that require balancing performance with correctness. 

Aside from coding, you can usually find me hunting for the best lattes in NYC, expanding my fragrance collection, or planning trips to explore new cities! 

<h2 class = "projects-heading"> Projects </h2>
  <div class = "project-grid"></div>
  {% for project.site._projects % }
    <div class = "project-card"></div> 
  {% endfor % }