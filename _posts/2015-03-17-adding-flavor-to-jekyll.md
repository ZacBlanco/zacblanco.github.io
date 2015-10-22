---
layout: post
title: Adding Flavor to Jekyll
permalink: /blog/jekyll/adding-flavor-to-jekyll/
keywords: jekyll, poole, site, css, web
description: Making Jekyll cleaner by using the Poole theme on your Jekyll GitHub Pages site.
---

So you've got your Jekyll site running, you can write some posts, but it still doesn't feel like your own. 

This is where Jekyll can be awesome, but also slightly confusing.

At the core, Jekyll takes a markdown file and uses it's templating system called **liquid** to help insert html and structure pages, and all of this is then converted into static html files that can be served on a web server.

For the rest of this post I'm going to assume that you at least know how to build your site with Jekyll and serve it locally to your web browser.

## Liquid 

I'm going to breeze over liquid right now, but I suggest that you take a look at [the liquid templating features in Jekyll](http://jekyllrb.com/docs/templates/).

To be brief, **liquid** allows us to pull sitewide variables and html code and place them right into our pages with ease.

{% raw %}
In your posts and any pages on  your Jekyll site by calling sitewide variables to insert values into your pages. In one of your posts, try adding the following.

    ##### [Homepage]({{ site.url  }})

{% endraw %}

The previous code should produce a link that directs you to your blog's homepage. See below for it in action! 

> ##### [Homepage]({{ site.url }})

## Changing the Style of Your Site

Jekyll has a whole host of themes that can change the look of your site. This includes downloading and installing prebuilt themes, or diving straight in to the `sass`, `css`, and `_layouts` foldera of the site to change how it looks.



### sass and CSS

No, this sass is not the same thing that you gave to your mother when you were a teenager. Sass is a version of css that can be run through a compiler  to compile css. If you'd like to learn more about sass, [click here](http://sass-lang.com/) 

Take a look inside the `css` or `_sass` folder within the directory of your jekyll site. Although these files are not `.css` files, you can still write normal css, and if you know sass, you can write sass too. The files that you modify will change the corresponding sitewide visuals based on those files.

### _Layouts

The `_layouts` folder is where you can write html templates for certain types of pages on your site. There should currently be at least one layout with the name `default`, `page`, and `post`. These names are the same names you will specify in the `yaml` at the beginning of a post, or page that you write. An example of the `yaml` header for this page is:

        ---
        layout: post
        title: Adding Flavor to Jekyll
        tags: [jekyll, poole, site, css, web]
        ---
        
We can see that the layout type for this post is `post` which tells Jekyll to use the `post.html` layout to format and style this page. You can also edit these files to change the look of your site.

### Poole

Personally on this site I use a theme called [Poole](http://getpoole.com/). If you would like to use this theme for your own site you can download and replace all of the `layout` and `css` files with the ones from Poole. It's a very simple and easy to use theme. Follow the documentation for the installation procedure.


### Wrapping Up

Hopefully from here you can better understand how Jekyll generates the site pages on your site. A bit of research and some work can make your site unique! You can also add new features and pages too by using the layouts and css files too. 

I hope now that you have enough knowledge about the structure of a Jekyll site to be able to customize it to your liking.



