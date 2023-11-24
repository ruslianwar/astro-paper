---
author: Rusli Anwar
pubDatetime: 2023-11-23T13:22:00Z
title: How I set up my blog using a Astro static site generator
postSlug: how-i-set-up-my-blog-using-a-astro-static-site-generator
featured: true
draft: false
tags:
  - docs
description:
  Some rules & recommendations for set up blog using astro.
---

Personal blogs are a great place to share your work and knowledge, and they have become a big part of the data analytics and visualization community. After trying several website builders such as Wix, Wordpress, and Squarespace, I decided to set up my blog using a static site generator instead.

Recently, several people asked me about my blog’s setup, and what tools I’m using to run it. Instead of answering these questions individually, I decided that it was time to summarize this information in one post. Here I’m going to answer some of the most frequent questions about setting up a static website for your blog based on my experience.

![Static Site generator Flow](@assets/images/ssgflow.png)

## Table of contents

## Site Generator (SSG)

As you may know, in a traditional web application an end user (also called ‘client’) requests a webpage through a browser such as Chrome or Firefox, and a web server, upon receiving the request, assembles the data and resources that comprise the page (database calls, static assets such as images, etc.), and responds with an HTML webpage. This makes sense for websites that require dynamic content, for example, user-generated comments in a forum, but it’s overkill for a blog or portfolio page that doesn’t require interactivity or frequent updates.

In contrast, a SSG builds all pages of a website in advance on your computer before they are uploaded to the server, so all pages are ready to be served as static files ahead of any actual request. This means that to serve your content there is no need for a more complicated server setup involving databases, caches, or reverse proxies. Once you have all pages of your website generated as static HTML and JavaScript files, you then upload those files to your server and make them available to everyone as your website.

Today there are over 400 different SSGs to choose from, with some of the most popular being Jekyll, Hugo, Gatsby, and Pelican. For my blog I chose Jekyll which uses the Ruby programming language under the hood. Jekyll is one of the most mature SSGs with an active community of contributors that have developed a rich ecosystem of plugins, themes, and other enhancements.

I would recommend reading this overview by Snipcart or this intro to SSGs by Netlify to understand how to choose the best SSG for your project. As the majority of SSGs are open-source, you can learn more by reading detailed documentation and help pages in their respective GitHub repositories.

### Why should I choose a static site generator for my website?

Here are some benefits of using SSGs that were important to me when I was choosing between a website builder and a SSG:

* **Access to my content at all times in a convenient format**. Many website builders offer an easy drag and drop interface, but it might be difficult to transfer the content of your website from one platform to another and preserve the original formatting including images, text formatting, links, etc. It was also not clear what happens to my content if a website builder ceases to exist at some point. With an SSG all my images and posts (in human-readable Markdown format) are saved locally on my computer, and I can always move them to another platform, if needed.
* **Fast load times**. Every time you request a page of a traditionally built website, it is built and loaded after the request is submitted. That might result in longer loading times. As I mentioned earlier, SSGs serve pre-built pages that significantly reduces the load time, even for content-heavy websites.
* **Security**. As the infrastructure for serving a static website is simplified compared to a traditionally built website, there are fewer parts involved in building and serving it. Therefore, there are fewer ways for malicious attacks to be executed.

> Of course, it is important to remember that since the stages of generating the website’s pages and publishing them are separate when using a SSG, managing a static website requires more technical knowledge compared to a drag-and-drop website builder. If you are not afraid of learning the basics of HTML, CSS, and JavaScript, and working with the command line, I’d encourage you to give SSGs a try.

## Where can I host my blog?

Whether you are going to use your custom domain or not, there are numerous platforms that you can choose from to host your static website. This is a good overview of different options from Geekflare. When selecting which platform to go with, you’d need to consider not only the technical knowledge you need to have to manage it, but also the cost of the solution you will choose.

In my case, I’m using a private repository on GitHub to host the content of my blog, and Netlify to build my website and publish it on the web. Both platforms offer free tiers for individuals.

### What tools do I need to run a static blog?

Here is the list of software and services I’m using to run my website:

* **Jekyll** as my SSG platform. To design your blog, you can go for one of the free themes or buy one from a marketplace such as Envato Market or JekyllThemes.io.
* **VS Code** as my text editor. I’ve also recently started using Notion to draft my posts as it allows me to collect my ideas and add changes using multiple devices (laptop or phone). I can also export posts from Notion in Markdown format. However, Notion doesn’t support all elements of Markdown, so you might need to fix the formatting in a text editor afterwards.
* **GitHub** to store my content. If you are new to GitHub, I would recommend going through their learning guides first. If using the command line to interact with GitHub sounds intimidating, I’d suggest using GitHub’s Desktop client which makes adding and updating content in your repository much easier.

* **VS Code** also integrates with GitHub natively, so you can connect and add content to your repository directly from the text editor.

Netlify to build and publish my blog on the web.

## Bonus

### Image compression

When you put images in the blog post (especially for images under `public` directory), it is recommended that the image is compressed. This will affect the overall performance of the website.

My recommendation for image compression sites.

- [TinyPng](https://tinypng.com/)
- [TinyJPG](https://tinyjpg.com/)

### OG Image

The default OG image will be placed if a post does not specify the OG image. Though not required, OG image related to the post should be specify in the frontmatter. The recommended size for OG image is **_1200 X 640_** px.

> Since AstroPaper v1.4.0, OG images will be generated automatically if not specified. Check out [the announcement](https://astro-paper.pages.dev/posts/dynamic-og-image-generation-in-astropaper-blog-posts/).

## Conclusion

I hope this gives you a good overview of why and how to use static site generators to set up your website. I’ll keep updating this post as and when I receive new questions in the future.
