---
layout: post
title: "Scaling Web Apps With Messaging Queues"
description: ""
category: 
tags: [web application,
        message queue
        JMS,
        java,
        PHP,
        C,
        C++,
        ActiveMQ,
        Stomp,
        message broker,
        facebook,
        twitter,
		scaling]
---
{% include JB/setup %}

When it comes to scaling a web application, one of the easiest ways to boost performance is with an asynchronous queue. Since web apps have started to become as complex as native desktop applications, users are expecting them to perform like such. This is where using asynchronous queues comes in to play. Typically with high traffic sites like Facebook, digg, twitter, et al, not everything needs to happen instantaneously, it just needs to look like it. For example, when you choose to send a message to someone on facebook, chances when you click that "send message" button, facebook takes your message and shoves it in a queue of other messages to be processed. To you the user, it looks like its already been sent, but in reality there might be a little bit of a delay. This makes it so that facebook doesnt have to handle sending thousands of messages per minute when you immediately click that button. Granted this is just a hypothetical example, I honestly have no idea how they actually handle such requests, but it makes for a good working example so that you guys can have an idea of what's going on.

So thats the general gist of an asynchronous queue, now lets dive in a little deeper. When looking for an asynchronous queue, or messaging system or messaging broker, as some are called, there are a variety of options to consider. Today we're going to be looking at [Apache Active MQ](http://activemq.apache.org) because it takes advantage of the Java Messaging Service (JMS) and also integrates with a wide variety of languages including Java, PHP, C++, C, C#/.NET, and a wide variety of others. The example Im going to show you will be using PHP through the Stomp protocol. First off, lets install ActiveMQ.

First off, we need a server environment. Currently Im using Ubuntu Server 9.04. ActiveMQ is a Java application, so we need to install Java.

```
sudo aptitude install openjdk-6-jre
```

We're also going to need Maven to pull in any necessary Java dependancies. 

```
sudo aptitude install maven2
```

Now somewhere (Id recommend your home directory) download the ActiveMQ source and unpack it:

```
wget http://mirrors.ecvps.com/apache/activemq/apache-activemq/5.3.2/apache-activemq-5.3.2-bin.tar.gz
```

```
tar xvf apache-activemq-5.3.2-bin.tar.gz
```

Now, cd into that directory and run the following:

```
chmod 755 bin/activemq
```

Now we're ready to build ActiveMQ using Maven

```
mvn clean install
```

And thats it! Simply run ```./bin/activemq``` and ActiveMQ will start right up.

So now that we have a message broker set up, we need a way to start sending it messages. To do this, we are going to use the Stomp protocol with PHP. Im going to show you a simple example that opens a Stomp connection, sends a message to the queue, and then retrieves the sent message and displays it on the screen. Typically you would separate this file into a producer (a script that enqueues messages) and then a consumer (usually a daemonized background process) to retrieve the message and decide what to do with it. The nice thing about ActiveMQ is that your producers can be of the same language, or different languages all together depending on your processing needs.

To start, go and [grab the Stomp PHP library](http://stomp.fusesource.org/release/php/1.0/stomp-php-1.0.0.tar.gz) and unpack it. Since we will be including Stomp.php in our script, you are probably going to want to add the Stomp library to your php.ini include_path.

Now with that all installed, we are going to create our script.

<script src="https://gist.github.com/399611.js"> </script>

The code in this example pretty much explains itself. Since this is a simple example, it sends a message, then reads it back. Typically your consumer would be running in a loop, checking the queue for new messages and then processing the messages when it receives them.

That is pretty much it. I wanted this to be just a short introduction, so hopefully in a week or so, I'll come back and give a more involved tutorial that will show how you can separate that script into a consumer and producer to really get work done.
