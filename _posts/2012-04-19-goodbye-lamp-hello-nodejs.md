---
layout: post
title: "Goodbye LAMP, Hello NodeJS"
description: ""
category: 
tags: [nodejs,
		php,
		LAMP, 
		mongodb, 
		markdownwiki, 
		server, 
		webapp web, 
		applications]
---
{% include JB/setup %}

When I first started developing web applications about five years ago, PHP was my language of choice because it was super easy to learn and a LAMP stack was trivial to setup on my local machine to get started. At the dawn Web2.0, PHP was booming as a web language. People began developing and rapidly prototyping applications with PHP and MySQL. You knew C or C++? PHP was even easier for you to dive into and get started with. When I started, I jumped on the Codeigniter bandwagon as it made development quick and provided the minimal amount of structure needed via the MVC design pattern so that you didnt just end up with tons of files of spaghetti code that wasnt structured at all. After a while with Codeigiter, I realized it was _too_ simple. At the time, it lacked an autoloader and didnt play well with libraries that didnt comply to its limited interface provided to work with libraries. So I decided to [build my own framework](https://github.com/seanmcgary/foundation-php). Foundation-PHP (as I decided to call it) provided a similar structure to Codeigniter, but also allowed me to change how classes were loaded and how objects were created, including the features that Codeignter lacked. At this point, I also had been introduced to MongoDB, so I decided to make that the default database handler instead of MySQL or Postgres. My framework wasn't quite on par in terms of features compared to Codeignter, but it included all the features that _I_ needed to build applications quickly. 

## Javascript comes to the server

During this time that I was hacking out applications built on PHP, other frameworks such as Ruby on Rails, or Django, or Pylons were starting to mature and developers were starting to move away from the battle tested LAMP stack. People started throwing around buzzwords like "high availability", "big data", "NoSQL". PHP started its decent from a popular web language whith people starting to move to languages like Ruby and Python. Things like HTML5 and CSS3 were skyrocketing in popularity and usage. Browsers, with tons of help from Google Chrome and its rapid release cycle, started to incorporate these features that were't even finalized by the W3C into their new versions so that people could start using these new exciting features. Javascript in particular, started to become very popular in due to developers using it to create applications with an almost desktop experience. 

Then in 2009 NodeJS, created by Ryan Dahl, appeared. NodeJS consists of Google's V8 Javascript engine along with a set of core libraries providing APIs to expose common lower level features such as file I/O, sockets, IPC mechanisms, etc. Because NodeJS is built with the help of Javascript, it is event driven providing purely asynchronous I/O thus minimizing overhead compared to traditional blocking I/O patterns. 

Due to the asynchronous nature of NodeJS, it was able to perform very well when writing servers. A community quickly formed around NodeJS and started to contribute to its feature set. Developers started writing libraries and modules to work with databases, the underlying operating system, websockets, and graphics just name a few. Soon, developers jumped at the fact that NodeJS makes a fantastic web application server. Hell, an HTTP server library is built into Node's core.

## The great migration

Not to long ago, I decided to say farewell to PHP and being moving over to NodeJS. The first thing I did? (Build a web framework)[https://github.com/seanmcgary/NodeWebMVC]. Node is still very young and does not (yet) have all the frameworks and tools that more mature languages such as PHP, Ruby, and Python have. To me, having a small framework to get up and running is a huge advantage t orapid protyping and development of ideas.

## Jumping in the express lane

By default, NodeJS comes with the necessary tools to create an, albeit simple, webserver. In less than 20 lines of code, you can have a "functioning" webserver that will send data to a browser upon connection. That however doesn't do us much good when trying to create an application. Fortunately, the guys over at Visionmedia decided to build a little library called [expressjs](https://github.com/visionmedia/express) to make developing HTTP servers a bit easier. Express is built on a number of [connect](https://github.com/senchalabs/connect) libraries giving you features like HTTP routing, sessions, and parsing POST, PUT, GET, and DELETE requests. Having these features puts express on par in terms of features and functionality with libraries such as Sinatra. You can quickly build a server that has routing and session handling, great for developing API servers, but it still lacks a little bit more structure needed for rich web applications.

## Adding some structure

For my framework, I took express as a base and started to build around it. I very much liked the MVC pattern of Codeigniter, so I decided to sprinkle some MVC on top of express. Doing this allows the developer to clearly seperate code into controllers, models and views instead of just setting functions for specific HTTP requests. This also allows the developer to write code that is a bit more modular, taking advantage of inheritence for controllers and models. The one thing that I am loving about Javascript that is making development much easier is using Mustache (in this case Handlebars.js, a fork or mustache) for templating and view parsing. Now instead of needing to write code in view files, they are done in HTML with Mustache placeholders and then parsed serverside before being sent to the user. This makes everything much cleaner and less intrusive. This also makes the flow of data from datastore/database to view much simpler. View content can be fetched from the database and with very minimal modifications, sent directly to the view to be parsed. This is a HUUUUGE advantage. 

## Lets build some apps!

Since building this small framework to provide a bit of structure, Ive been able to start rapidly prototyping applications. Right now, the app I am writing, (Markdownwiki.com)[http://blog.markdownwiki.com], is a living test of my framework and allows me to constatly add features into the framework that I feel might be commonly needed by developers. 

Now that my migration to NodeJS from PHP is nearly complete, I will be continually building out my NodeWebMVC framework for people to use. So if you're looking for a framework to get started with building an app, I encourage you to check it out. I would love to get some feedback on it. As I develop it, I'll tag stable points along the way so that you wont be confused if it for some reason it doesnt work. If you run into bugs, [file them here on github](https://github.com/seanmcgary/NodeWebMVC/issues) and I'll get to fixing them .Alternatively, feel free to fork it and fix them yourself and submit a pull request with your fix. 
