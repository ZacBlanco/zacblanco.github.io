---
layout: page
title: Open-Source Projects
permalink: /projects/
---

{% for project in site.data.projects %}
* [  {{  project.name  }}  ](  {{  project.url  }}  ) --- {{  project.description  }}
        
{% endfor %}
