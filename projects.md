---
layout: page
title: Projects
permalink: /projects/
---


## Open-Sauce Projects


{% for project in site.data.projects %}
*{{[  project.name  ]}}{{(  project.url  )}} --- {{  project.description  }}
        
{% endfor %}