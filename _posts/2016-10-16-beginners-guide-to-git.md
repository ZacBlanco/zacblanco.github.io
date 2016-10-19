---
layout: post
title: A Beginner's Guide to Git and Version Control
author: Zac Blanco
keywords: git, version, control, cvs, svn, git, software, engineering, commit, learn, guide
permalink: /blog/tips/intro-to-git/
date: 2016-10-16
---

![Git Logo](/assets/images/version-control/Git-logo.svg.png)

## Audience

This guide hopefully will be useful to students and others who have never used or heard of version control and are interested in using it or incorporating it into their own projects. By the end of this guide readers should have a general idea of how to get started using version control systems and begin to effectively use hosted Git services like GitHub.

## Topics

- [What is Version Control?](#what-is-vc)
- [Why Version Control?](#why-vc)
- [Popular Version Control Programs](#popular-vc-programs)
- [A Short History of Git](#a-short-history-of-git)
- [Introduction to Git](#intro-to-git)
  - [Installation](#installation)
  - [Repositories](#repositories)
  - [Commits](#commits)
  - [Branches](#branches)
  - [Checkout](#checkout)
  - [Merging](#merging)
  - [Cloning/Pulling](#clone-and-pull)
  - [Pushing](#push)
- [Typical Workflow](#typical-workflow)

## <a id="what-is-vc"></a> What is Version Control? 

From the Google dictionary<sup>[[1]](#1)</sup>:

> **ver·sion con·trol**, _noun_ - computing <br> the task of keeping a software system consisting of many versions and configurations well organized.

Basically by version control is a way that we can keep track of the changes that occur to a software project. Generally it is used for software and related tech, but you could use version control for anything such as writing a paper, an article, or even for a school project. Basically it just gives users a way to make sure that they don't lose work on an important project. It also allows them preserve the history of anything that they've worked on.

##  <a id="why-vc"></a> Why Use Version Control?

There are various reasons that version control is useful. But one of the main, almost obvious, reasons is to be able to keep track of the changes that occur to a project in order to identify rogue behavior and prevent a loss of work.

There are several other reasons listed here on this [Stack Overflow Article](http://stackoverflow.com/questions/1408450/why-should-i-use-version-control)<sup>[[2]](#2)</sup>

## <a id="popular-vc-programs"></a> Popular Version Control Programs 

Below is a list of popular version control systems.

**Disclaimer**: This is not a full list of all version control systems. There many others but I have chosen to highlight some of the more popular ones.

1. [Git](https://git-scm.org)
2. [Apache Subversion](https://subversion.apache.org)
3. [Bazaar](http://bazaar.canonical.com/en/)
4. [Mercurial](https://www.mercurial-scm.org/)

**Git** - An open source version control system originally designed and developed by the famous Linux Kernel developer, Linus Torvalds.

**Apache Subversion** - A version control system originally developed by [CollabNet](http://www.collab.net/), but donated to the Apache Software Foundation where it has been top level project since 2010<sup>[[3]](#3)</sup>

**Bazaar** - A version control system which is a part of the GNU project and sponsored by Canonical, the company behind Ubuntu.

**Mercurial** - A free open source versioning system with a GNU GPLv2 License. It is implemented with Python and has been since around 2005.


## <a id="a-short-history-of-git"></a> A Short History of Git 

Git is one of the most widely used, if not _the_ most widely used version control system in existence today. Git is a project which was started by Linus Torvalds, the man behind the Linux Kernel. He originally created the project so he could manage contributions to his wildly popular open source operating system kernel.<sup>[[4]](#4)</sup>

The purpose of git was to be better than the current VCS solutions available at the time. The Linux community had learned its lessons from using other systems while going through development. Torvalds and the community wanted something more reliable and robust than what they had used before. So they based this project "git" around a few main principles<sup>[[4]](#4)</sup>: 

- Speed
- Simplicity (Compared to other systems)
- Support for non-linear and branching development.
- Completely Distributed
- Ability to scale with large projects.

With these principles in mind, git was born into existence. Since then it has become a fierce force in the market for version control systems. Companies have been built around it, services have been created, and hundreds of thousands of projects are now hosted online thanks to Git and companies like GitHub.

## <a id="intro-to-git"></a> Intro To Git 

In this section we're going to take a deep dive into git and its features. We'll do a quick installation guide and then dive into the basic gits concepts.

From now on, assume that anything like the below block is meant to be run in the command line, such as the bash shell.

```bash
echo "I'm a bash command"
```

### <a id="installation"></a> Git Installation 

Most unix systems will already come with Git installed. However, if not you can run the following commands:

For Ubuntu/Debian based systems:  
```bash
sudo apt-get install git
```

CentOS:
```bash
sudo yum install git
```

Mac OS X/MacOS:

First you'll need to [install homebrew](http://brew.sh)
```bash
brew install git
```

Windows:

If you're on a windows based system you can install git via the [git website](https://git-scm.org).

[Download Location](https://git-scm.com/download/win)

After installing git on windows you should have access to a program called "Git Bash" which will give access to a git command line.

Git itself is a program on the command line, but it has what we'll call **subcommands**. Each subcommand servers its own purpose. Below is a list of the subcommands we'll be covering and using.

- `git init`
- `git status`
- `git add`
- `git commit`
- `git clone`
- `git pull`
- `git push`
- `git diff`
- `git merge`
- `git checkout`
- `git branch`

Don't worry too much about what each one means. We'll go into detail below.

### <a id="repositories"></a> Repositories 

So now that we've installed git you should keep your command line open. We'll work off the command line. I'm going to assume you have basic skills in navigating the Unix command line and in creating directories with commands like `mkdir`, `cd`, `touch`, `echo`, etc..

Now first let's go over some of the basic concepts behind git. 

- Every 'project' in git is typically called a **repository**
- A **repository** is basically a project, or set of code, files, etc. that we want to keep track of and version.
- Everything that resides within a repository will be tracked by git.

So basically think of a repository as a kind of "special" directory within your file system. Everything within the directory is what will be versioned.

In order to create a repository we need to run the following command:  
```bash
git init <REPOSITORY_NAME_HERE>
```

Alternatively if you want to initialize a repository in the current directory you can simply run  
```bash
git init 
```

What this does is create a special directory within your repository directory called `.git`. This directory keeps track of your repository objects and a bunch of other metadata that is useful for kep track of files.

### <a id="commits"></a> Git Commit 

**Commits** define the basic unit of change in a project.

Every commit defines a set of changes to the repository. You can think of it as a kinda of "_delta_". It basically tells us what changes should be played on top of a repository to go from one commit to the next.

So if you haven't already made a repository, create one by running the following commands:

```bash
git init hello-git
cd hello-git
```

So now that we've created that we can run this command called `git status`. It will tell us which files might have been changed, or if any new files were added to the repository.

```bash
git status
```

You will probably get an output like the following.

```
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

So the command tells us exactly what's going on in our repository and what changes may have been made, Note the last instruction "..use "git add" to track"

So let's create a new file and add it to our repository.

```bash
echo "Hello, World!" > hello.txt
git status
```
Your output should be something like

    On branch master

    Initial commit

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            hello.txt

    nothing added to commit but untracked files present (use "git add" to track)

Let's add our file so that it is **tracked**. Basically a file being **tracked** means it's a file within the repository that should have any changes to it tracked by version control.

But why wouldn't you want to track every file? There's a couple of reasons why you might not want to.

- Tracking uneccesary file can make your repository larger than it needs
  - This can lead to longer cloning times and lots of useless information
  - Typically you only want to track the files that are 100% necessary to run/compile your code
- Sometimes you want to temporarily use data files in the repository which aren't necessarily part of the code base.
- Compiled code should not be tracked.

So now let's add hello.txt to our tracked files and check the status.

```bash
git add hello.txt
git status
```

We should then have something like

    On branch master

    Initial commit

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)

            new file:   hello.txt


Great! So now if we make a commit this file will be tracked.

Let's make our first commit. Remember that a **commit** defines a basic unit of change in a repository. This means that this commit will define the unit of change going from an empty repository to one with a text file named `hello.txt` 

One important note about commits is adding a useful commit message. Code isn't always readable. And trying to make sense of code after commits can be difficult. Comments in code help, but typically they're a little too specific.

A commit message should be short, maybe 30-50 characters which describe the changes that you're making to a repository. A commit title should be relatively short, but the body of a commit message can be much longer. For now on the command line we're only going to deal with short commit titles. But one of the most important things about git and tracking changes is having *short*, **descriptive** titles for every commit. Ideally these titles should be completely human readable and easily understandable.

The most well-engineered pieces of software will typically have great commit messages. I suggest looking at commit messages from commits on the Linux Kernel or from git development itself

- [Sample Linux Kernel Commit](https://github.com/torvalds/linux/commit/f1a9622037cd370460fd06bb7e28d0f01ceb8ef1)
- [Sample Git Commit](https://github.com/git/git/commit/9ed0d8d6e6de7737fe9a658446318b86e57c6fad)


```
git commit -m 'My first git commit: Add hello.txt'
```

Output should be similar to

    [master (root-commit) f21a1ab] My first git commit: Add hello.txt
    warning: LF will be replaced by CRLF in hello.txt.
    The file will have its original line endings in your working directory.
    1 file changed, 1 insertion(+)
    create mode 100644 hello.txt

Awesome! We've now created our repository, added a file, and now we've even started tracking changes. From now on, any lines added to the file will result in changes when running `git status`.

Try to add some more files on your own or maybe some code too. Don't forget to write good commit messages! We'll be moving on to more advanced topics soon. Do your best to play around and use what you've learned so far to get comfortable with git and the command line.
 
### <a id="branches"> </a> Git Branches

So aside from basic units of change, like **commits**, one of the other fundamental ideas behind version control (for git at least) is the ability to create what we call **branches** in a repository.

_What is a branch?_

One of the easiest ways to think of this is that every project has a main code-line, or master **branch** (sometimes called the **trunk**). That is the place where every feature gets added. Then using that main code line, you have the ability to create these branches which stem completely from the main code line. It's like they're their own mini-repository. They have all of the code and all of the history from the main code line, but the changes end up being detached from the master branch.

So then why do we have branches?

Basically branches are a way for individuals to break up pieces of work on a project in a way that they don't affect the main branch, but can still are able to track the work done on a particular set of files or features without affecting the main code line until the work on the branch is completed. Then finally you want the code from the branch to be merged back into the master branch. The reasoning behind this is that you don't want broken code or bad features to be merged into the main code line for a project. But you still want to use all of the features of the version control as you work on a specific feature for the project. This way when that feature is completed, it can be tested for errors to make sure that the code is stable before being merged to master.

So let's try to create a branch.

Go into that repository we just made named `hello-git` and run

```
git branch my-special-branch
```
If you run git status you should notice a line

      on branch master

Now run

    git branch -v

You should now see the new branch `my-special-branch`. Next, we'll show you how to do what is called a **checkout** of a branch

### <a id="checkout"></a> Git Checkout

So at this point we've made a new repository named "hello-git" and we've also made a new branch alongside master called "my-special-branch". The issue now is that we made the branch, but we actually can't make changes on the branch until we do what is called a "checkout" which makes our repository switch over to the copy of the code which the branch uses.

So let's run

```bash
git checkout my-special-branch
echo "Look a special change!!" >> hello.txt
git status
```

After running those three commands your git status should be something like

    On branch my-special-branch
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

            modified:   hello.txt

    no changes added to commit (use "git add" and/or "git commit -a")

Notice that to commit any changes we still need to add the file to the staging area. But notice that after the checkout we have the line `On branch my-special-branch` meaning our changes are being made to the "my-special-branch".

So let's add the file and make a commit

```bash
git add -A
git commit -m "Adding new special line to hello.txt file" # Make sure you have a really clear commit message!
```

Great! We have our changes committed to the second branch.

Try running

```bash
git checkout master
git status
cat hello.txt
```

If you run these commands you should notice that your changes from the my-special-branch didn't actually carry over to the master branch yet. That's due to the fact that the code in a separate branch from master is kept separate until you explicity tell git that you want the changes merged into the other branch. That's what we'll be going over next in git merge.

This should at least give you the basics on how to create a branch in git, how to checkout and then make changes, without affecting the rest of the repository. Remember most good project want to keep the commit history on the main code line somewhat clean, but still use the powerful features of version control to save work and keep intermediate versions. That's what branching is good for. We'll go over typical git workflows once we learn about merging

### <a id="merging"> </a> Git merge

So at this point our repository has two separate branches, a **master** and **my-special-branch**. My special branch has a new change that we want on the master branch. How do we do that? We do it with `git merge` Git merge does exactly what it sounds like. It merges two branches. But to clarify, `git merge` is more like a one-way merge than a two-way merge.

So what git **doesn't** do:

- Take two different branches (one of which was branched from an earlier point in time), merge them into a single one and automatically discard the old ones and just leave the new one.

What git merge actually does:

- Looks at the specified branch, and the current branch. It will look at the history of the branch you're currently on and find how many commits ago the branch was made.
- Then it will "replay" the commits (all of the changes) from the other branch onto your current branch and create a new commit on the current branch with a message (if specified).

So in short, it kind of "adds" the history of one branch onto another, thus merging the changes into the new branch with a commit.

Let's try it now:

```
git checkout master
git merge my-special-branch
```

_Voila!_ We've now officially merged the branch into master.

You can see the changes by getting the content of the file from `cat`

```
cat hello.txt
```

### <a id="clone-and-pull"></a> Cloning and Pulling 

So one of the ideas around git is that it is _designed_ to be decentralized. A decentralized system prevents bottlenecks or downtime when cloning code. Not only that but it also means you can grab a copy from the code over the network from anyone's repository that is available. 

There is no "master" that is _required_ to always have the latest version of code, and that's one of the beauties of git. Any copy of the repository which doesn't exist on the local machine is called a **remote**. Remotes can be added or removed to a repository and they allow you to push and pull bits of code too and from your repository to other repositories based on the remote configuration. 

So lets jump in to actually grabbing code from another repository:

The `clone` subcommand will attempt to access a hosted repository based on a specified URL. You can use HTTP, HTTPS, or Git protocols.

Let's try clone a repository from GitHub.

```bash
git clone https://github.com/git/hello-world.git
```

Another option:

```bash
git clone git://github.com/git/hello-world.git
```

Notice the difference between the protocols `https://` and `git://`

Note however according to the [git docs](https://git-scm.com/book/ch4-1.html) that the `git://` protocol is not typically only used for cloning or pulling. It also does not provide any form of authentication.

Either way, you should get something like this:

    Cloning into 'hello-world'...
    remote: Counting objects: 158, done.
    remote: Total 158 (delta 0), reused 0 (delta 0), pack-reused 158
    Receiving objects: 100% (158/158), 17.78 KiB | 0 bytes/s, done.
    Resolving deltas: 100% (47/47), done.
    Checking connectivity... done.

If you do `ls` you should now see a new directory named `hello-world`. This is the directory of the freshly cloned repository. Typically a clone is only performed once. It is for the first time you need to get a repository onto your computer.

When you clone the repository you automatically get this thing called a **remote** added to the git metadata. The **remote** is the _origin_ location that the repository came from. This will default to  whatever url was used to clone. You can add more remotes by using the command `git remote add`.  Use `git remote -h` to see all of the options. `git remote -v` will show all of the push and fetch URLs for each remote.

One thing that similar to cloning is called **pulling** using the command `git pull`.

Pull is _kind of_ similar to clone, except for a pull, you need to already be within a repository.

If you run `git pull -h` you'll get a help text for the commands.

The two most common pulls you'll use are probably

- `git pull [remote_name/branchname]`
- `git pull`

These two commands basically look for any new commits or updates that were on the remote repository, and attempt to replay them on top of your own repository in order to reflect any changes that have been made to the code, making a commit in the process. It's similar to git merge, except for remote repositories.

It's also common to use the  `--rebase` option which runs `git rebase` instead of `git merge`. I suggest looking up the [documentation for rebase](https://git-scm.com/docs/git-rebase).  

### <a id="push"></a> Git push

When working with repositories that may not be located on your machine locally, you'll need a way to be able to get those remote repositories to reflect the changes you've made to your local computer. You're able to do that with `git push`

`git push` will basically take the current branch you're working on and push it to your default (or specified remote) and it's the basic way to make sure any of your changes are replayed into your remote repository to reflect the work that you've done. 

It's relatively simple. Typically you'll use it if you have remote repositories where you have push access (privileges to change code). Once you get started cloning your own repositories from GitHub you'll see exactly how to  

## <a id="typical-workflow"></a> Typical Git Workflow

Below is a basic workflow that you might find:

```
git clone https://myremote/myrepository.git
cd myrepository
git checkout -b feature-123 # creates a new branch
# edit/compile/test
git commit -a -s
# edit/compile/test
git commit -a -s
# You're finally happy with your changes, merge into new branch
git checkout master
# Make sure we have any new changes from master 
git pull --rebase
git merge feature-123 -m "[Issue-123] - Add special feature"
# Reflect the update in the remote.
git push 
```
And that's it! It's not too hard to start using. The real challenge is using a tool like git _well_. Good use of version control should make rollbacks and upgrades easy as well as differentiating the the software at various points in time. Bad version control could be just as bad and more time consuming as no version control at all! Make sure to do some more reading to brush up on the advanced features and some of the options that you can add to commands which can make git even more powerful

I **highly highly** recommend [reading the git documentation](https://git-scm.com/docs/) to get a better idea of what some of these commands do. This guide is intended for serious beginners who want to "just get started" without learning much of the technical details. But alas,  there is a high chance you'll need to know more in the future. The documentation on git is very thorough and I suggest reading through it to gain a better understanding of the functions of git.  


That's all folks!

#### References

<a id="1" href="https://www.google.com/search?q=define+version+control">[1]:</a> Google Definition of Version Control<br>
<a id="2" href="http://stackoverflow.com/questions/1408450/why-should-i-use-version-control">[2]:</a> Reasons to Use Version Control from StackOverflow<br>
<a id="3" href="https://en.wikipedia.org/wiki/Apache_Subversion#History">[3]:</a> History of Apache Subversion from Wikipedia<br>
<a id="4" href="https://git-scm.com/book/en/v2/Getting-Started-A-Short-History-of-Git">[4]:</a> History of Git<br>


