---
title: Build a website in 15 easy steps
author: Shyam Valsan
date: '2017-11-04'
slug: build-website-15-easy-steps-hugo-netlify-blogdown-R
categories:
  - R
  - blog
tags:
  - Technology
  - Website
  - Static
  - Hugo
  - Netlify
  - blogdown
  - Rstudio
  - Github
---

This is the story of how I built this website. I've used Blogger and Wordpress to create websites in the past and was frustrated at how restricted and cookie-cutter it all felt. So, when I first thought of creating this website I was mulling a straight up website building service like SquareSpace which all the podcasts I listen to keep asking me to use. But the cost of the whole endeavour put me off of square space for good - after I dug around a bit more I found the likes of Ghost and Jekyll, which were very interesting, the concept of fast loading static sites was appealing and the option to host for free on Github pages was a plus. 

I had almost decided to go build a Jekyll site when I stumbled upon the blogdown R package and this excellent book on how to build websites with it -> https://bookdown.org/yihui/blogdown/

So without further ado, here are the step by step instructions I followed to create this website. 

1. **Buy a custom domain.**  
I bought this domain (shyamvalsan.com) from GoDaddy for just 99 Rs for the first year. And this is the **only** money I've spent in the entire process of creating this website.  

2. **Download and install R studio**  
I didn't really need to do this, as I had these things installed already. If you're using Ubuntu, here's a script which will do this step for you.  

3. **Install blogdown**  
```
install.packages("blogdown")
```
3. **Install hugo**  
```
blogdown::install_hugo()
```
4. **Pick a theme**  
Browse the hugo themes and pick one you like. https://themes.gohugo.io
5. **Create a new github project**  
Sign up for github first if you don't have an account.
6. **Create a new project in R studio**  
Make sure it links to the github repo you created in the previous step.
7. **Build the site locally**  
```
blogdown::new_site(theme = '<github username of theme author>/<github project name of theme>')
```
Wait a few moments for the files to download and everything to get setup.
8. **Update config.toml**
Based on how you want to customize your website.
9. **Use the "New Post" addin to create new blog posts.**  
10. **Modify the website**  
Remove or update any other content that you wish to.
11. **Use the "Serve Site" addin to preview the website locally.**  
12. **Sign up for Netlify**  
To deploy the site to the Internet we'll use a service called Netlify. Go to the Netlify website and sign up. 
13. **Configure Netlify**  
Follow the easy to understand instructions to link your Netlify account to your git repo and also link it to the custom domain you purchased in step 1.
14. **Commit your changes and git push it to remote master**  
Netlify can be configured to automatically trigger a build (using the "hugo" command) and deploy it each time a git push happens.
  
**Your website is now live!**  
I reccomend using the optional SSL certificate from Let's Encrypt that Netlify provides for free.

## Code
https://github.com/shyamvalsan/HugoBlog

## Resources
* https://bookdown.org/yihui/blogdown/
* https://gohugo.io/hosting-and-deployment/hosting-on-netlify/