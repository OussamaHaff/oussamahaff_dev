+++ title = "How to use this Blog effectively"  
date = "2020-12-26"  
author = "Oussama Hafferssas"  
cover ="/img/00_how_to_uses_website/cover_site_readme.svg"  
description = "If it's the first time you visit my blog, you may find it a little different from other websites, therefore I'll try to make sure in this dedicated article that everything is clear and transparent for you."  
+++


>*Blog post edit history can be found in the website's Github repository* [*here*](https://github.com/hfrsoussama/oussamahaff_dev/commits/master/content/posts/01_groovy_to_kotlin_dsl_2018.md).

[TOC levels=1-3]: #

# Table of Contents
- [What can I find in this Blog ?](#what-can-i-find-in-this-blog-)
- [Does this website respect my privacy ?](#does-this-website-respect-my-privacy-)
- [There is no way to comment on an article, what's the alternative ?](#there-is-no-way-to-comment-on-an-article-whats-the-alternative-)
- [What is the tech stack used to build this blog ?](#what-is-the-tech-stack-used-to-build-this-blog-)



# What can I find in this Blog ?
I'll be sharing my knowledge about
- Mobile Apps engineering especially **Android**
- Software craftsmanship
- My own view on tech in general

# Does this website respect my privacy ?
This blog
- has `no ads`
- has `no trackers`
- has `no SEO scripts`
- does `not collect` / does `not store` / does `not share` any user data

It's a static site, generated from markdown files found in the master
branch of this
[Github repository](https://github.com/hfrsoussama/oussamahaff_dev)
using a static site generator

# There is no way to comment on an article, what's the alternative ?
In every article you will find a link to **create an issue** on the blog
Github repository. Templates for different types of comments and
feedbacks are **already filled** with basic information.

Using this
[link](https://github.com/hfrsoussama/oussamahaff_dev/issues/new/choose)
(found in every article) you can :
- ask a question
- suggest an improvement for an article or for the whole website
- alert the author about a deprecated content
- give a general feedback

{{< figure
src="/img/00_how_to_uses_website/github_issues_for_comments.webp"
position="center" style="border-radius: 8px;"  
caption="To avoid comments plugin that track you, I've made Github templates ready for you :)"  
captionPosition="center" >}}

# What is the tech stack used to build this blog ?
For optimal speed and security, the blog is built what's called the
[*JAM Stack*](https://jamstack.org)
- Pages and articles are just markdown files
- The actual website html pages are generated using
  [*Hugo*](https://gohugo.io), a static site generator
- An open source
  [theme](https://github.com/panr/hugo-theme-hello-friend/) is used by
  Hugo for styling the generated pages

The blog is published through a continuous delivery system using
[*Vercel*](https://vercel.com/) that builds automatically the website from
github and publish it on CDN.

If you want to try the website locally, you need to :
- install [Hugo](https://gohugo.io/getting-started/quick-start/)
- clone the website [repo](https://github.com/hfrsoussama/oussamahaff_dev/)
- launch the command `hugo server` from the root folder of the clone repo
- in most cases, hugo will indicate that the site is available locally
  on your machine at `localhost:1313`

