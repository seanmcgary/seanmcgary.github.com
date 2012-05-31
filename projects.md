---
layout: page
title: "Projects"
description: ""
---
{% include JB/setup %}

One thing that I absolutely love is learning. Learning new programming languages, new technologies, new skills. One specific area that I am passionate about is the web. I love what the web has to offer and I love learning about the new and exciting things that are quickly becoming available. As such, I love to do projects that expose me to these new technologies. It also helps me keep my knowledge up to date and my skills sharp. So here is what I have been working on recently and a little bit about each.

ALL of my projects can be found [here on github](https://github.com/seanmcgary). I also tend to blog about things I find interesting from time to time, so be sure to check the blog archive link at the top.
<hr>

# MarkdownWiki

- [Blog](http://blog.markdownwiki.com)

MarkdownWiki is an idea that I started working on back in January of 2012, the idea being that wiki platforms and wiki software has become rather outdated compared to the technologies available in todays web. MarkdownWiki is (the start of) an attempt to solve that problem by providing a cloud based platform for people to create wikis for anything they want and allow people to collaborate publicly (or privately) on their content. Whenever I have to deal with a wiki, its usually either Mediawiki or Confluence and I generally get frustrated because the "classic" wiki syntax is far more complicated than it needs to be and is not intuitive at all for people that edit things infrequently or have never edited anything before. In the majority of these settings, Ive been editing pages in the position of a software engineer, so I figured, why not make something with a syntax that every developer knows: Markdown. Also, lets improve the editing experience and make it easier by providing a syntax highlighted editor so that you can see what you're typing; almost like you're using an IDE or improved text editor. 

This has become a project that I keep returning to because it has many components and has already gotten fairly large. When I started, I was on a NodeJS binge, so Im using a framework that I wrote on top of the ExpressJS web server framework to provide more of an MVC structure to everything. Not sure if that was the absolute best choice out there, but its been working decently so far and has really exposed me to how NodeJS is in a web server/application setting.

# DrinkJS - the CSH Drink Server

- [Github page](https://github.com/ComputerScienceHouse/Drink-JS)

This is definitely one of the more interesting projects that Ive done as its not directly web app related. For those of you that don't know, CSH, or Computer Science House, is a special interest house in the dorms at RIT. In short, CSH is an organization of students that share one broad common interest - computing. They are notorious for staying up late working on servers, hacking out cool hardware, and for the most part, learning everything there is to know. 

The project that got the most publicity (and still does) was Drink - our networked <s>vending machine</s> community refrigerator that allows members to "drop" a drink from any device in the world connected to the internet with the idea being that you can delay the dropping of said drink so that it exits the machine as you walk up to it. Over the years drink has had a few servers to allow it to do all of this, the last two iterations being in ruby and erlang. Since it hadnt been rewritten in about 2-3 years, I decided to give it a go; this time in NodeJS to see if it lived up to all of the asynchronous hype it had been getting. 

So in short, I implemented, and improved upon the "Sunday Protocol" (a protocol developed by CSHer, Joe Sunday) with this new iteration in NodeJS. I also improved upon the existing WebSocket interface by moving it over to the more backwards-compatible friendly Socket.io library so that those people without websocket (eg. some mobile browsers) could still easily drop their favorite beverage while on the go.

# Foundation - an MVC PHP web framework

- [Github page](git@github.com:seanmcgary/foundation-php-core.git)

Foundation is probably the project that has left me with the most knowledge. A year or two ago, I was really into using Codeigniter as it was a great framework that could quickly get you going. The only problem it lacked a few things that I needed and wanted - class autoloading, better handing for external libraries, not forcing you to use MVC at all times, ability to integrate with PHPUnit for unit testing, etc. So I set out to design a framework that was similar to Codeigniter, but more flexible and included these needs. From this I learned a lot about PHP and how to do things like implemeting a routing system, pretty URLs, how to use mod_rewrite effectively in Apache, how to buffer view and template files, parse them together and flush the result to the browser. Building this framework really gave me a pretty intimate knowledge of PHP and programming for the web in general.

