---
layout: post
title: Hosting your Jekyll site with GitHub Pages
permalink: "blog/jekyll/hosting-with-github-pages"
tags: [jekyll, site, poole, web, hosting]
---

As a continuation to the series of the last few posts I will be explaining how to host your jekyll site for **free** using [GitHub](https://github.com). GitHub provides completely free website hosting using your own GitHub account. For more information I suggest reading up on [GitHub Pages](https://pages.github.com/).

First off, you're going to need to [create an account on GitHub](https://github.com/join) if you haven't already. GitHub is a fantastic site that can host private and public git repositories for your personal projects.

Here's a list of a few advantages to using GitHub Pages:

- Completely Free
- A site for every project
- Use your own domain name
- Completely integrated with Git

And of course some disadvantages:

- No server side scripting
- No control over how the site is served
- No secure connections with `https`


Now this really isn't a complete list, but these are some of the things I thought would stand out to most people.

Now for the fun part! Let's get started.

### Create a Repository for Your Site

First make sure that you're logged in to GitHub. Then, head over to your GitHub profile located at `https://github.com/username`. 

At the top of the screen click the **`+`** button at the top of the page and create a new repository. You **MUST** name the repository in the following way: `username.github.io`. Your site will not be created if it is not named correctly.

Now that we've got your site's repository set up we can finally add your Jekyll files to that repository.

Now let's head back to the desktop and open a new terminal window. Using the terminal navigate to the desired folder where you would like your project to be contained.

Then, assuming you've got git installed we're going to use the following command:

        ~ $ git clone https://github.com/username/username.github.io
        
Now check using `ls` or by opening up a new finder window, navigate to the same folder you just had by using the terminal and you should find a new folder by the name of `username.github.io`. Open up this folder. This is the place where we will copy your Jekyll files to be pushed to GitHub's servers.

Now assuming you've already built a Jekyll site, copy the files from your site into this folder. Go back to your terminal window and `cd` into `username.github.io` folder. Then type the following commands

        ~ $ git add --all
        ~ $ git commit -m "Init Commit"
        ~ $ git push -u origin master
        
Once you've done that, congratulations! You've made your first GitHub pages site! Open up a web browser and head over to `http://username.github.io`.

The site should be live within a few minutes. New changes after this should be live within seconds of pushing to the server.

#### An Alternative Method

If you do not have git installed on your computer, or you are unfamiliar with the command line, *and* you're running Microsoft Windows or Mac OS X, there's another method you can use to get your site running. 

Download [GitHub for Windows](https://windows.github.com/) if you're running Windows, or [GitHub for Mac](https://mac.github.com/) if you're running OS X. Once you install GitHub's software, login to the git client. You should see a list of your repositories within the software. Clone the repository that we just created on GitHub. There should be a menu to do this. 

If you open a file browser, look for a folder in the location `~/username/documents/GitHub/username.github.io`. This is the location where your repository is located. You can copy your files from the Jekyll site into that folder. Once you've done that, head over to the GitHub software and name your commit, add a message if you'd like. Then `commit` and `sync`. 

Once you've synced your repository, you can open a web browser and navigate to `http://username.github.io` where your site should be live in a matter of seconds.

If your site is not appearing, then you may have named your repository wrong, or you might have to wait a few minutes for the servers to create your site. Wait at least 10 minutes to see if your site is up.

If you're having trouble refer to the github pages documentation

Another great thing about GitHub pages is that on top of having a site for yourself, you are allowed a site for each project you have located at `http://username.github.io/project-name`. This is a great way to host many pages and to allow people to access and learn more about your open source project.

Hopefully this brief guide was enough for you to get your site set up on GitHub Pages. Good luck!

