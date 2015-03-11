---
layout: post
title: Building a Site with Jekyll
permalink: none
tags: [jekyll, hyde, site, blog, markdown]
categories: [jekyll, site, markdown]
Author: Zac
---

So I when I first started trying to make a blog I didn't realize how difficult it would actually be to find something that worked. No other free platform contained the features that I wanted. And as a college student we should know that, free = good.

[Jekyll](http://jekyllrb.com/) is no perfect solution, but it does provide a host of features and is much more open than any other option I could find, which is why I opted to switch.

For a quick intro, Jekyll straight out of the box really isn't anything special. Basically it takes some text that you write up for your posts and converts them into some static html files that can be served to a user.

Posts are written in [Markdown](http://en.wikipedia.org/wiki/Markdown) which is a really handy markup language. Great for writing posts, but it does have a bit of a learning curve and takes some time to get used to.

Another great thing about using Jekyll is that there's quite a bit of resources, and its open source! It's very flexible, you can use plugins, create new themes, and add commenting features. This list just goes on!

Creating a site from scratch with Jekyll is definitely not for the faint-hearted. If you're willing to put the time and effort in you can come out with a blog customized to your very liking including only the features you would like. It's a very hands-on get-dirty-with-the-code project, but it's a lot of fun and you might learn a few things along the way.

The rest of this post I'll be showing you how exactly to install and run Jekyll to *quickly* create your own static website.

# Prerequisites

##### **Please download ruby first, then RubyGems**

You're going to need **ruby** installed on your machine. If you dont have ruby installed [you can get it here](https://www.ruby-lang.org/en/downloads/)

You're also going to have to install [RubyGems](https://rubygems.org/pages/download). It's package manager for ruby. 

Now I'm going to assume you know how to install these or already have them on your system. Follow the documentation provided online or do a quick google search if you're having any trouble.

Once you've got them both installed, open up a command terminal and try this command

    ~ user$ ruby --version
    ~ user$ gem --version

If everything works correctly you should get some type of outputs similar to this: 

    ruby 2.0.0p481 (2014-05-08 revision 45883) [universal.x86_64-darwin14]
    2.0.14

Great! now we have the two essentials installed. 

# Getting Jekyll


With a valid internet connection and a command terminal, use the following commands to install Jekyll on your machine.

    ~ user$ sudo gem install jekyll
    
Check now that you have the latest version of Jekyll installed by using 

    ~ user$ jekyll --version
    
# Creating your first site

Here's where things start getting fun.

Using your terminal, navigate to the chosen directory on your filesystem where you would like to keep your code, site, and any other files *(Preferably a location where you might keep some repositories)*

Assuming you've already navigated there perform the following

    ~/location/ user$ jekyll new <site-name>
    
Or if you'd like to have jekyll generate the files in the folder you're currently located in

    ~/site-name/ user$ jekyll new .
    
Now if you use ls command or view that directory you just created inside a file manager you should see something similar to the following

    _config.yml	_layouts    _sass	css		index.html
    _includes	_posts		about.md	feed.xml


##### _config.yml

This file is where we store some sitewide variables, like author name, baseurl, twitter and GitHub handles, etc. Edit the variables in here as you like.

##### _posts

This is where you're going to write posts in the Markdown syntax. I suggest an editor than can support previews of Markdown. Personally I use [Adobe Brackets](http://brackets.io) with the Markdown plugin to preview. More on all of this later

##### about.md 

This is a markdown file you can edit to change the "About" page on your site.

##### _includes, _layouts, _sass, css

These are all other places where you can change configurations and views of your site. The layout and css can all be found under these places. We're not really going to edit those right now.


# Making your first post

Lets navigate to our folder and create a new post. Create a new file and name is something following the scheme: `YYYY-MM-DD-your-title.md`. Open this new file in a text editor.

Inside we're going to put:

    ---
    layout: post
    title: Hello World!
    ---
    
    # Howdy there partner
    Congratulations! You've made it to[site](site.url)
    
    We're ust getting started here.
    
Now save your document and return to the terminal that has navigated inside of your site's directory.

# Building the Site

Run the following command

    ~/location/ user$ jekyll build
    ~/location/ user$ jekyll serve
    
Leave the terminal open. Open up a new tab in your browser of choice and navigate to `http://localhost:4000`

You should see a home page with a link to Jekyll's default page for new sites, and your own post we just wrote!

Click on the post to make sure it works there. If it does, congratulations! you've made your first blog post using Jekyll

There's tons more you can do with themes, pages, and other sweet customizations for your shiny new blog, but I'll go into those at a later date.

For now, play around with Jekyll and get a feel for the software. It's something interesting that I've never encountered before and is definitely worth a try.

Happy blogging!
