---
layout: post
title: "Getting started with native NodeJS modules"
description: ""
category: 
tags: [
	NodeJS,
	C,
	C++,
	javascript,
	modules,
	native]
---
{% include JB/setup %}


NodeJS has quickly gained in popularity since inception in 2009 due to its wide adoption in the web app community, a lot due to the fact that if you already know javascript, very little has to be learned to begin developing with it. As evident by its [modules page on github](https://github.com/joyent/node/wiki/modules), you can pretty much find the library you are looking for (it's sort of starting to remind me of PHP where if you could think of something, chances are there is a module for it). 

One of the things that I think people forget is the fact that you can develop NodeJS modules not only in javascript, but in native C/C++. For those of you that forgot, NodeJS is possible because it uses Googles V8 javascript engine. With V8 you can build extensions with C/C++ that will be exposed to javascript. Recently, I decided to dive into the world of native modules because I needed a way to use the imagemagick image manipulation library directly from javascript. All of the libraries currently listed on the NodeJS modules page take a rather round-about approach by forking a new process and calling out to the commandline binaries provided by the imagemagick library. This is VERY VERY VERY SLOW and since image manipulation can be very intense, being able to use the library dirctly will make things MUCH faster.

## Part one: babby's first native module

This is the first part in what will hopefully be a multipart tutorial as I write a native module for imagemagick. Today, we will take a look at making the most basic of of native modules and how to use it with Node.

To start off, lets create a file called ```testModule.cpp```. This is where everything (for now) will happen. Heres what we need to start:


<script src="https://gist.github.com/3971190.js"> </script>


Note, this is assuming you have NodeJS installed already and in your path (if you dont, go do that!). We need to import both the Node header and the V8 header.

To build our module, we will be using the ```node-waf``` tool that comes bundled with NodeJS. In the same directory as ```testModule.cpp``` create a file called ```wscript``` and put the following stuff in it:


<script src="https://gist.github.com/3971198.js"> </script>


The wscript file sets up needed environment variables and libraries that need to be linked at compile time. Think of it as some kind of makefile. The ```t.target``` property needs to match up to the name of the export property in your module (I'll point this out when we get there).

Now, to build your module simply run the following:


<script src="https://gist.github.com/3971199.js"> </script>


Alright, now that we have those basics out of the way, lets make a module that when called, returns the string "Hello World". 


<script src="https://gist.github.com/3971202.js"> </script>


So to quote the V8 handbook:

> A __handle__ is a pointer to an object. All V8 objects are accessed using handles, they are necessary because of the way the V8 garbage collector works.

> A __scope__ can be thought of as a container for any number of handles. When you've finished with your handles, instead of deleting each one individually you can simply delete their scope.

So if we were to think of this in javascript, we'd basically have something like:


<script src="https://gist.github.com/3971204.js"> </script>


So now that we have a function that can do some kind of work, how do we expose it to Node? Lets take a look:


<script src="https://gist.github.com/3971206.js"> </script>


The function ```TestModule``` takes an object handle and basically shoves our function in it. This is how exports work in C++. In javascript we'd have:


<script src="https://gist.github.com/3971207.js"> </script>


Now, a note on the ```NODE_MODULE(…)``` line. Before when I said ```t.target``` needed to match, this is where it needs to. The first argument of ```NODE_MODULE``` needs to be the same as your target value.

Once you have all of that, build your new node module. To try it out, run node and import your module.


<script src="https://gist.github.com/3971211.js"> </script>


That's it! You now have your first native module. In my next post, we'll dig deeper into building something a bit more substantial. One of the main draws of NodeJS is its asynchronous nature, so next time we'll take a look at how to go about building a module with asynchronous function using libuv that its at the very core of NodeJS.



