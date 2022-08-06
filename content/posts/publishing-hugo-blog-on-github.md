---
title: "Publishing Hugo Blog on Github"
date: 2022-08-06T12:44:28+02:00
draft: true
---

first checked what blogging platforms there are. ended up with mataora (instead of the one it's based on). but didn't like I can't change the design of the blog.

so started checking largest two, jekyll vs hugo. ended up with hugo because it didn't require ruby, was just a single binary so it's convenient to run it from anywhere.

first followed https://gohugo.io/hosting-and-deployment/hosting-on-github/, didn't work
github actually has pre-generated github action for hugo, that worked
but blog page was missing (was marked as draft; shows on local server, which is slightly confusing)
used custom domain, followed github instructions for apex domain and `www` subdomain.
then https was broken. was just a matter of time: https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https#troubleshooting-certificate-provisioning-certificate-not-yet-created-error