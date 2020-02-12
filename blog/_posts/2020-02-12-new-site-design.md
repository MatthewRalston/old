---
title: Site Redesign
subtitle: Chris Rhymes - Bulma Clean
image: /assets/img/jekyll-cms.jpg
hero_image: /assets/img/clean-blog-jekyll.png
tags: meta prose
---

## Welcome!

Welcome to the new website, I've recently merged my blog Not Very Humerus with my [static homepage](http://matthewralston.github.io). The point of this blog post is to introduce the reader to my experience theming with Jekyll and specifically the Bulma clean theme.

The best part of re-theming was the essential plug-and-play experience of copying my blog's `_post` directory into the Jekyll theme and watching the site automatically populate with my blog's content. 

The biggest challenge was wrestling with the site's `site.baseurl` and `site.url` in generating working links throughout. I'm not sure if that was a bug that was truly mine, but it seemed like the raw theme didn't quite have the same problems. The problem started when I was changing the `site.url` to my localhost and eliminating `site.baseurl` in anticipation of the deployment to my primary Github static page. 

Aside from fixing some broken links in the blog's pagination, the purpose of exploring the possibilities by resetting `site.baseurl` was to simply get my meme popovers working again! After making the necessary adjustments, all of the memes display correctly on the site.

The short term goal for the website is to simplify the sites maintenance and deployment needs. To this end, I was looking at 2 or 3 themes for the site redesign, with bulma-clean and prologue being the two forerunners. At the time, I liked the idea of the prologue theme's contact form using formspree.io but I couldn't ignore the flexibility in templating offered by bulma-clean. After copying the contact form, I elected to go for the older template instead of the very responsive HTML5 design in prologue. 

In the long term, I'd like to add a few applications and perhaps a more permanent solution to the contact page when I deploy the website to a dedicated server. But until that time, I'm very happy with the appearance and functionality of the site and its sections.
