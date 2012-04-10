---
layout: post
title: "Getting Started with NodeJS and Server Side Javascript"
description: ""
category: 
tags: [ Node,
        NodeJS,
        Google Chrome,
        Google V8,
        server side javascript,
        javascript,
        browser,
        asynchronous,
        threaded,
        Server,
        TCP,
        socket,
        Event driven IO]
---
{% include JB/setup %}

With [NodeJS](http://nodejs.org/) and server-side Javascript becoming a very prominent technology in web applications today, I figured Id give an introduction to NodeJS to get everyone up and running. "But how can Javascript run in a server environment, I thought it required a browser and was only used to make my website interactive". NodeJS allows you to build applications written in Javascript with the help of Google's V8 Javascript rendering engine that is at the heart of the Google Chrome web browser. Coupled with the asynchronous nature of NodeJS, V8 contributes greatly to Node's performance and scalability.

"Okay, thats cool and all, but why should I use it?"

Theres really two reasons - Performance and the fact that Javascript is becoming a pretty universal language. If you've ever dealt with web development of any kind, chances are you've had some experience with Javascript. The performance comes from a combination of Google's V8 engine, the other is the fact that Node is asynchronous and event driven, very much like JS applications that run in your browser. NodeJS is all a single thread. 

"But wait, how does that make it more efficient than multi threaded applications? Wouldnt a single thread be a bottle neck?

If this were a traditional server implementation that would be true, but Node is a bit different. Instead of needing to manually identify events and spawn new threads based on those events, you simply register events that will act as callbacks when fired - very similar, if not exactly the same, as most applications of Javascript that will run in your browser. Since we're not manually creating new threads for each new event, and each event is asynchronous, none of the events will block the NodeJS event loop. This allows NodeJS to execute concurrent connections very efficiently and quickly by receiving an event and then pushing it to the background to run, with the process notifying the server when its done. 

"Thats pretty cool, how do I get started then?"

First off, you're going to need a computer with a non-windows operating system (so anything Unix, Linux, or Mac OS X will work). They're working on a build to work with Windows, but its not quite done yet. Next, grab the source from Github [here](https://github.com/joyent/node). We're going to be building version v0.4.x. NodeJS only has two things needed for building and installing - python 2.4 (or higher) and libssl-dev (if you plan on using SSL encryption). Now to build Node, cd into the directory you checked out and run the following:

<script src="https://gist.github.com/857826.js"> </script>

This will build Node and install it to your path. Now, all you need to do to run Node on the command line is just run: ```$ node <your file name>```. 

Now, lets take a look at a simple NodeJS TCP server implementation:

<script src="https://gist.github.com/857837.js?file=gistfile1.js"> </script>

Now lets take a more detailed look at what this is doing. First, we need to include the "net" library that is built into Node. This is done using the "require" function. Node comes with a [bunch of libraries](http://nodejs.org/docs/v0.4.2/api) that allow you to create sockets, interact with the filesystem, make system calls, and much much more. You can also create your own libraries, both in C++ and plain javascript, to include in your Node applications. Now that we have the "net" library included, we can proceed with creating our server object. The server takes a single variable, we'll call this "socket" because thats essentially what it is. We'll use this to write and read data to/from our connection. Because Node is asynchronous, we need to add some listeners - connect, data, and end. The first two are pretty self explanatory, with the data event firing whenever the client sends data to the server. So, instead of creating a thread loop to block and wait for data from the client, we simply register an event for it that will listen while the rest of the server runs unblocked. When the server receives data, the callback function will be called and the logic contained will be executed. The last line in our file simply tells the server to start listening on a provided port and host. And thats it! It's very simple, you just set some listeners and then just forget about it while it runs.
