---
layout: post
title: Intro to Hadoop Workshop
author: Zac
keywords: hadoop, hortonworks, workshop, rutgers, usacs, zac, blanco, hive, pig, virtualbox, sandbox, big data, analytics
permalink: /hadoop-workshop/
date: 2015-10-28 
---

## Outline

The following is an outline of events for this workshop

1. Introduction
2. Install virtualbox + download sandbox
3. Intro to Hadoop Presentation
4. Workshop



## Downloading + Installing VirtualBox

Virtualbox is free virtualization software developed by Oracle. You will need to download and install the software from the website here:

> [Virtualbox Download](https://www.virtualbox.org/wiki/Downloads)

##### IMPORTANT

If you're running on an intel processor, please make sure you've enabled **Intel's Vt-x** virtualization technology in your **computer's BIOS**.

If you don't know how to enable this, please search on google or ask someone else nearby.

## Downloading The Sandbox

You must download the [Sandbox here](http://s3.amazonaws.com/hortonassets/2.3.2/HDP_2.3.2_virtualbox.ova) if you haven't already.

**NOTE** you will need a _MINIMUM_ of 8GB of RAM to run this. 16GB is recommended. I will instruct you on which settings to change in Virtualbox once you 


### Importing the VM into VirtualBox

Open virtualbox, then go to **File > Import Appliance**. From here select the `.ova` file that you just downloaded.

It should import within a minute or two. Sit tight now and we'll get the sandbox up and running after the presentation


## Presentation

The presentation will consist of:

- Information detailing the history of the Apache Hadoop project
- A general overview of what Hadoop really does
- Why we should care about hadoop
- Learn about the architecture of a cluster
- Learn about the architecture of Hadoop services


## Workshop


First off, you'll need the [dataset here](/assets/hadoop-workshop/geolocation.zip) 

You can find the instructions for the whole workshop [here](https://www.outlearn.com/learn/hortonworks/hortonworks-tutorial/1), but we're going to work through the entire tutorial together. However you may work ahead if you like.

If you computer has only 8GB of RAM, please lower the RAM for the Sandbox VM:

**Make sure your Virtual Machine is OFF before doing this**

- Open virtualbox
- Right click on the sandbox
- Open **Settings**
- Navigate to **System > Motherboard**
- Use the slider and set the RAM to about 6000 MB

# Please ask questions if you need help or are confused


