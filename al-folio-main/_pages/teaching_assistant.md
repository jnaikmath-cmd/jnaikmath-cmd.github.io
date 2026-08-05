---
layout: page
title: teaching 
permalink: /teaching/
description: Courses assisted during my doctoral studies at IIT Goa.
nav: true
nav_order: 6
---



I currently serve as a Teaching Assistant in the Department of Mathematics at the **Indian Institute of Technology (IIT), Goa**. In this role, I support faculty members by conducting tutorial sessions, assisting with course administration, and helping students master fundamental mathematical concepts.

### Courses Assisted at IIT Goa

* **MA101: Calculus** (2024 – Present)
* **MA203: Numerical Analysis** (2024 – Present)

---

## Academic Responsibilities

My duties as a Teaching Assistant include:
* **Tutorial Sessions**: Leading problem-solving sessions to clarify complex topics in Calculus and Numerical Methods.
* **Student Mentorship**: Providing guidance during office hours to help undergraduate students bridge the gap between theory and application.
* **Grading & Assessment**: Assisting in the evaluation of assignments and examinations to ensure consistent academic standards.

## Pedagogical Interests

I am particularly interested in the integration of computational tools into mathematics education. I enjoy demonstrating how theoretical results in **Numerical Analysis** can be implemented using:
* **Python** and **MATLAB** for algorithmic verification.
* **FEniCS** for solving differential equations numerically.

---

{% if site.teaching_assistant %}
  <div class="teaching">
    {% for item in site.teaching_assistant reversed %}
      {% include resume/teaching.liquid %}
    {% endfor %}
  </div>
{% endif %}