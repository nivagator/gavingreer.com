---
title: "Hugo Site Conversion"
date: 2024-10-23T15:16:10-05:00
---
Welcome to the new site. 
<!--more-->

Previously, this site was built on the Jekyll blog platform. I was new to static site generators and Jekyll was where I started. From there I built another site using a single script site generator built by a guy named Roman, [SSG](https://romanzolotarev.com/ssg.html). This was fantastic for extremely simple sites, converting markdown to HTML with a one size fits all template approach. Eventually I wanted more bells and whistles and moved to [Hugo](https://gohugo.io/). Since then I've built several sites using this platform and I've been happy with it. 

The site, however, has gone un-updated for several years. There are a few reasons for that and somewhere on that list is the Ruby/Gem system underneath the Jekyll platform. It was the "plan" to learn a little Ruby as part of the Jekyll experience. I had a friend who worked as a Ruby developer who as since moved away and taken my Ruby ambitions with him. A few years back, I pulled up the site repo to make some edits only to have the `bundle exec` command return errors and wasn't ready to put the time into correcting it. And again, by this time, I had built and was maintaining other sites using Hugo. I knew a conversion was going to happen sooner or later. 

## Static all the sites

I've built sites with Node.js, Wordpress, Jekyll and single page templates, but of the sites I manage, the majority of them are built in Hugo now. The common direction they have migrated is that most of them are now generated static sites of some kind. This makes a lot of sense for the content I manage. It's simpler to manage with a text editor and git than something like PHP and Wordpress and requires less resources on the server side. 

Hugo has been great for getting started quickly with their templates. I don't have a background in Go and that has made some of the scripting frustrating at times. Overall, there's enough user content out there to navigate most of the challenges. To date, there hasn't been anything that I wanted to do to a site that Hugo could not deliver on. 

I'll share some of the lessons learned working with Hugo here going forward. I still have a few sites that need to be converted to Hugo too. More to come.
