---
layout: post
title: Configuring A Custom Domain with GitHub Pages
keywords: namecheap, hosting, github pages, github, pages, cloudflare, cdn, DNS, domains, records, ANAME, CNAME, A record
permalink: /blog/tips/custom-domains-github-pages/
date: 2015-10-29
author: Zac Blanco
---


This article will tell you how to set up your domain registrar's DNS records so that you can forward your GitHub Pages site to your newly purchased domain.

It's actually quite easy to do. The hard part is actually figuring out where to start and which information to put in the records.

## Steps

1. Purchase domain
2. Commit a CNAME File to your GitHub Pages Directory
3. Add A Records for GitHub
4. Add CNAME record for your GitHub Pages domain.


### Step 1

So if you haven't got your own domain yet, you should head over to places like [Google Domains](http://domains.google.com), [Cloudflare](http://cloudflare.com), or [Namecheap](http://namecheap.com).


You can purchase your own domain for as little as $8 per _year_.

### Step 2

Next you'll need to commit a new file to your github pages named `CNAME`. Note that on this file there is no extension. The entire file name is simply `CNAME`.

This file should be placed at the root of your site directory. That is at the very base of your repository (see image below)

![Github repo](/assets/images/github-pages-dns/github-1.png)


### Step 3

We're going to now go into your domain registrars DNS settings and add two `A records` for your domain.

I'm using namecheap, so I'm going to head over to the dashboard and manage my domain `blanco.io`.

![Namecheap domain](/assets/images/github-pages-dns/namecheap-1.png)

Then we're going to go to **Advanced DNS** then click **Add Records**

![Namecheap DNS](/assets/images/github-pages-dns/namecheap-2.png)


Once you've clicked add records, select the record type  **A Record**.

The **A Record** points to the IP Address for Github. We're going to need to add two of these. One for each IP address

Make sure you fill in the values exactly as shown below, except you will create a record for each of these IP addresses

- `192.30.252.153`
- `192.30.252.154`

![Namecheap A Records](/assets/images/github-pages-dns/namecheap-3.png)

### Step 4

Once you've done that you'll finally need to create a CNAME record that looks exactly like the one below. Except you will just need to replace `username` with the your github username that is the location of your current github pages site.

![Namecheap CNAME Records](/assets/images/github-pages-dns/namecheap-4.png)


Lastly, make sure you have disabled any other redirects or records than may have been active before adding these records as well.

After that you should be done! If you have set the `TTL` (Time to Live) to Automatic, then your page should update almost instantly!

Enjoy your new custom domain :)







