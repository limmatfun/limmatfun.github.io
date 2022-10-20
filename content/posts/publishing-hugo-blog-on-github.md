---
title: "How this blog is published"
date: 2022-09-19T12:44:28+02:00
draft: true
limmat_temperature: 19.8
tags: ["Tech"]
---

Once I decided I wanted to write a blog, I started looking at what options were out there. I already knew I was not interested in anything heavy like Wordpress. I started reading what people recommended on [Hacker News](https://news.ycombinator.com/) and came across [Bear](https://bearblog.dev/) and [Mataroa](https://mataroa.blog/). Instead I wanted to try out one of the static content generators, which I think are a perfect fit for a blog anyways.

first checked what blogging platforms there are. ended up with mataora (instead of the one it's based on). but didn't like I can't change the design of the blog.

so started checking largest two, jekyll vs hugo. ended up with hugo because it didn't require ruby, was just a single binary so it's convenient to run it from anywhere.

first followed https://gohugo.io/hosting-and-deployment/hosting-on-github/, didn't work
github actually has pre-generated github action for hugo, that worked
but blog page was missing (was marked as draft; shows on local server, which is slightly confusing)
used custom domain, followed github instructions for apex domain and `www` subdomain.
then https was broken. was just a matter of time: https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https#troubleshooting-certificate-provisioning-certificate-not-yet-created-error

Along the way added little details like limmat temperature