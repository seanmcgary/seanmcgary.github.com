---
layout: post
title: "Improving Database Performance with Memcached"
description: ""
category: 
tags: [ database,
        performance,
        memcache,
        memcached,
        PHP,
        MySQL,
        Codeigniter,
        web applications,]
---
{% include JB/setup %}

When it comes to web application performance, often times your database will be the largest bottleneck and can really slow you down. So how can you speed up performance when you have a site or application that is constantly hitting your database to either write new data or to fetch stored data? One of the easiest ways is to cache the data that is accessed the most. Today, I cam going to show you a brief example of how to do this with [Memecached](http://memcached.org/) using PHP and the [Codeigniter framework](http://codeigniter.com). First off, what exactly _is_ Memcached? "Memcached is an in-memory key-value store for small chunks of arbitrary data (strings, objects) from results of database calls, API calls, or page rendering." Basically Memcached is a distributed system that dumps key value pairs to RAM for super fast access. If you need to scale, all you do is add more RAM or more nodes with more RAM. Lets get started.

First, we need to install memcached, so I will show you how to do so with a Debian based system (Debian/Ubuntu). First off, run the following from your commandline.

```
sudo aptitude install memcached libmemcached2 libmemcached-dev
```

This installs the base Memcached server along with the development libraries. Now we need a way for PHP to interact with Memcached, so we're going to go and grab the Memcache PHP extension from Pecl. If you haven't installed anything via Pecl before, its basically a PHP extension repository and manager and works similar to aptitude, but specifically for PHP. To use Pecl, you need the PHP5 dev package as well as Pear.

```
sudo aptitude install php5-dev php-pear
```

Once you have those installed, you can install the PHP Memcache extension:

```
sudo pecl install memcache
```

Depending on which operating system you're using, you may need additional packages or libraries. For me on Ubuntu 9.10, I needed to install make and g++ in order to build the extension. So just take a look at the end of the output when you install the extension, and it will tell you what is missing.

Now we're ready to start some coding. For this tutorial Im going to be using Codeigniter since I really like its object oriented structure and its nice database class. I will be using the new (though not yet official) version 2.0. You can find it [over on BitBuckt](http://bitbucket.org/ellislab/codeigniter/) and either clone the repository (note, you'll need Mercurial to do this) or just download the source in a compressed format. Now we're going to need a database to grab data out of. You can call it whatever you want. The only thing that you need is a table called "users". Since I did this with 1000ish "users", you can make the table yourself, or you can grab a dump of the table [here](http://seanmcgary.com/uploads/users.sql).

Now that we have everything set up, we can start some coding. Connecting to Memcache is really simple and is done in two lines:

<script src="http://gist.github.com/408031.js"> </script>

nSo now, what we are going to do is create a controller and a model. In this case, the controller is just there to call our model and display the returned data. The model will be doing all actions associated with accessing the database and accessing Memcache. So we are going to create two files: a controller called main.php and a model called user_model.php:

<script src="http://gist.github.com/408032.js"> </script>

<script src="http://gist.github.com/408034.js"> </script>

In our controller we load the model up in the constructor since we will be using it with every function. The function "cache_users()" is used to call the model to tell it to take all the users in the database and put them into the cache. We'll get to the specifics of that in just a minute. The function get_user tells the model to go into the cache and find a user based on their unique user_id.

The model is a bit more involved process. First off in the constructor, we connect to Memcached and assign it to a class level variable so that all of our other methods can access it. The first function, "get_users" is an all-in-one example. Before I explain the function, lets first figure out how to access Memcached. Memcached stores things as a key-value pair in memory. Keys need to be a unique value and can be as large as 250 bytes. The value can be anything - string, array, object, pretty much anything in PHP that can be serialized can be stored as a value. This however does not include database connections or file handles. For our purposes, we are going to be storing each row of the user table in Memcached. So a way to do that in a unique way would be to store a hashed version of the SQL query that we would use if we were accessing the user from the database. So for example, if our user had the user_id of 43, we would hash the query used to access him from the database:

```
"SELECT * FROM users WHERE user_id=43"
```

The get_users function is going to store ALL users in a single index in Memcached, so the first line we come to is the query to access all the users from the database. The we perform an MD5 hash on it and assign that to a variable. Now we check and see if that key is in Memcached by performing ```$this->memcache->get($key)```. If that key does not exist, it will return NULL. So we check to see if it's null. If it is, we know that we have to hit the database to grab the data. So we do that and while we're at it, we put the resulting data into Memcache so that when we need to get it again, it's now there. And of course if the key does exist, we don't even touch the database. It's all a pretty simple and straight forward process.

Lets take a look at ```cache_users()```. Here what we are doing is grabbing all the users from the database and looping over all of them. The idea behind this is that we want each user to be in the cache individually versus all together like in the previous example. So while we are looping over the returned users, we prepare a SQL statement for them as if we were going to get them from the database, and then we store the user in its own row in Memcache. Now to store something in Memcache, we call ```$this->memcache->set($key, $value, $compression, $time)```. $key and $value are pretty self explanatory. $compression is a boolean value (0 or 1) that specifies if you want your data compressed or not. $time is the amount of time that you want the data to stay in the cache (set in seconds). Once that time has expired, the row is flushed from the cache. Now that we have all of our users in the cache, we can call fetch_user_from_cache and you will get your user!

Hopefully this shouldve given you a decent overview and an idea of how caching works so that you can apply it to your own applications. If you have any troubles or questions, leave a comment and I'll help you out!
