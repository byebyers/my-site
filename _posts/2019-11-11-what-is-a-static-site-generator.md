---
layout: post
categories: Jekyll
title: What is a Static Site Generator
sub_heading: Understanding the basics of these tools.
date: 2019-11-11 17:00:00 -0700

---
With the site builders in the world, you may wonder if there are other ways to streamline development? Many agencies rely on tools like Wordpress to create websites but are there other ways? With the same ease of use to generate content?

I am not here to rag on Dynamic site builders for I know there will be great ones to come to make the web even better. Anyone who does not want to waste time coding will happily pay for a solution so they can focus on design. But when it comes to creating sites fast you may have heard of Static Site Generators.

What are they? Simply put, they are local servers that take templates and content to create HTML. Whenever the code or content changes, a new site is spun up for hosting.

![](/uploads/build2.gif)

This is a hybrid approach to web development. You get all the benefits of hosting a web server at home and the perks you get with static sites. You never have to worry about a server going down online.

With that, you open yourself to a huge network of tools. Repository sites, hosting, headless CMS (will talk about those soon), and other great features. A lot of these tools are not offered inside a site builder's walled garden. And these features will be a better bang for your buck in the long run. For example, my brewery client's last hosting bill was between $0.50 and $0.60 cents! Nuts!

Plus, since your site is static, that means your content files do not need to be served from a server. Which makes your website less of a resource hog. Not to mention the fact you can keep these content files on Github or Gitlab. Giving you a free rollback system that goes beyond a week, month, or a year depending on what you pay for. I especially like that these generators have ways to migrate from one system to the other. So I am never tied down. Just a free spirit living my coding life on the web ;)

What other benefits does this leave for you as the developer or creator? For me it was better oversight on the content creation process. You get a local server, you can store code in a repository, and can use Headless CMS's commit changes. This creates a clean track record via git, so you don't have to worry about an online server. As a creator, you get all of the same bells and whistles you get with a CMS like Wordpress or Squarespace. Pretty cool!

Which brings me to the Headless CMS. They are a separate operation acting like a regular CMS on the surface. The difference is that they commit changes to your repository instead of rendering the content on a server. Usually, you operate with an online CMS but they connect to your templates and serve as your backend. They give all the nice shiny things creators want when blogging or running online shops. But hey didn't you say this was done as a local environment? Yes! But in 2019 we have these amazing tools called build environments. So those local commands are done via a virtual machine on the cloud. You give the could-based terminal instructions and it scans your repository for changes. With the new site sent to your host.

So if you want to give one of these a go I recommend trying Jekyll as a first. It will help with the basics like infrastructure and configuration. Plus, there is a huge network of tutorials out there. Otherwise happy coding!