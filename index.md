---
layout: default
title: Home
---

Hi, my name is Zac (if you didn't figure that out already). I studied Electrical and Computer Engineering at Rutgers University through the spring of 2018. I am currently a software engineer at [Alluxio](https://alluxio.com).

I like writing [code](https://github.com/zacblanco) and doing research. I also [play guitar and sing](https://soundcloud.com/zac-blanco) from time to time.

You can find notes for some of the classes that I took [here](education/)

--- 


#### Recent Activities

{% for act in site.data.activities %}

* **{{  act.date }}** - {{ act.desc }}

    {% if forloop.index0 >= 4 %}
        {% break %}
    {% endif %}

{% endfor %}