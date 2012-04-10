---
layout: post
title: "Threaded TCP Server in Python"
description: ""
category: 
tags: [Tutorial,
        example,
        connections,
        sockets,
        threads,
        python,
        Server,
        TCP]
---
{% include JB/setup %}

Recently I finally decided to take some time to learn Python. So I figured the best way to learn something new is to dive right in and write an application. This application happened to be a new server for Computer Science House's networking vending machine(s) 'Drink'. The general idea behind Drink is that it's a 'communal refrigerator' for CSH that the members 'donate' money to in order to stock it with delicious drinks (such as Coke products since RIT is exclusively a Pepsi campus). Being the geeks we are, these vending machines that we have on our floor _must_ be accessible via the internet in some way, thats where the server comes in. The server needs to facilitate connections to each machine as well as accept incoming connections from clients that want to drop drinks and then shuffle those requests off the to tini-boards that control the physical machines. So immediately I was thrown into learning threading and sockets in Python.

Well thats cool and all, but youre probably asking why youre here. I mean, the title does hint that you'll be learning something. This is true, we're getting there so just sit tight for another minute. So one of the issues that needed to be overcome while writing this server was having an instance of a server that can server multiple clients at the same time and not have one client blocking the socket connection. Python, being the flexible language it is, offers you multiple ways to handle sockets, threads, and the combination of the two. In this process what we want to happen is have a server bound to a specific address and port, but once the connection is accepted, we want the server to scoop that connection to a semi-random port in its own thread so that we dont block other clients from connecting. First lets take a look at the server implementation:

<script src="http://gist.github.com/627757.js"> </script>

First we start off by importing the socket and thread modules. Now, if we wanted to make this a threaded class, we could 'from threading import Thread' so that our server could inherit from Thread (this is an example of one of those many modules for threading I mentioned). Now if we look at our main "method" we define our host, port, and buffer size and we create a tuple called 'addr' to hold the host and port. Now in this implementation, we have created a SOCK_STREAM socket which is the same as a TCP socket. When making this a server, you have to remember to bind the socket to the addr tuple (this is not the case with the client we will implement). And finally we tell the socket to listen for connections. The 2 we pass to the listen function call tells the server that it can queue up 2 connections before it starts to refuse them.

Now for the magic/voodoo awesomeness of the server. In the while loop you'll notice that when we call ```serversocket.accept()``` we get two variables back: a clientsocket and the clientaddr. Now, we take that socket connection and we hand it off to a thread by calling ```thread.start_new_thread```. In this call, we pass it 'handler', which is the function you see defined at the top with the clientsocket and clientaddr as parameters. This function then runs, receiving and sending data with the client. Because we spawned a new thread to handle this connection, the server is free to keep accepting connections from other clients.

Now lets take a look at a simple client to interact with our server.

<script src="http://gist.github.com/627768.js"> </script>

99% of this should look the same as the server we just created. The only difference here is that we arent creating new threads and we're not binding our socket to our address. Instead we are just connecting to the server and then looping while we send/receive data that we receive from standard in.
Hopefully this has been helpful. There are a lot of Python resources out there, but it took me a while to find an implementation that worked in my situation. So hopefully this is simple enough for anyone to modify to fit their needs.
