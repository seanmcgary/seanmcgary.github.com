---
layout: post
title: "Codeigniter Google Calendar Library"
description: ""
category: 
tags: [Google Calendar,
        Codeigniter,
        Library,
        Codeigniter Library,
        GData,
        API,
        Tutorial,
        Calendar]
---
{% include JB/setup %}

So I was playing around with the Google Calendar portion of the Gdata API the other day and did some searching and found that there wasnt a Codeigniter library for it, probably because it seems that Google has teamed up with the guys that are working on the Zend Framework to bring Gdata to the PHP world. So I took the ZendGdata API for Google Calendar and implemented it in Codeigniter so that you just need to make a few simple function calls to gain authorization to a calendar, add events, query events, etc. This is my first attempt at writing a \"library\" for something, so hopefully it turned out well. If you find any errors in it or have any suggestions for improvement, [just let me know!](mailto:sean@seanmcgary.com)

Files you'll need: 

- [Codeigniter-Gcal](http://www.seanmcgary.com/uploads/Codeigniter-Gcal.zip) (Link fixed)
- [ZendGdata]("http://framework.zend.com/download/gdata)

I also have it managed through a Git repository on on [Github](http://www.github.com/seanmcgary/Codeigniter-Gcal/tree/master). Feel free to clone it or pull from it.

##Setup:

First off we need to edit your config file. Open up your application/config/config.php file. Scroll down to the uri_protocol option and change it to PATH_INFO. Then go to the uri_allowed_chars setting and place a question mark after 'a-z'. This allows you to put question marks in the URL. That?s it!

* Place the Gcal.php file in your Codeigniter syste/application/libraries direcetory
* Install the ZendGdata library with one of the two options:
	* Make a directory anywhere on your machine/server
	* Place the ZendGdata directory in it
* Open you php.ini file and find the include_path line.
	* Add to the include_path the directory that you placed the ZendGdata directory in. ```/your/directory/ZendGdata/library```
	* Save and restart Apache
* Place the ZendGdata directory where you want.
* In the Gcal.php library file, modify the require_once so that it points at the directory where ZendGdata is located. IE: ```require_once(\"/your/directory/ZendGdata/Library/Zend/Loader.php\");```
* Place the calendar.php file in your controllers directory

The calendar.php file is a sample controller. In it you'll find an example of a call to each function in the library. Also with each one, I go through and show how to manipulate the data returned since it can get a little confusing at times (the return values are often many-dimensional arrays that are a bit difficult to interpret just by looking at them).

## AuthSub

AuthSub is basically Googles version of OAuth. Like OAuth, AuthSub sends the user to log into their Google account and then returns them to a specified URL with an access token as a GET variable.

<script src="http://gist.github.com/131008.js"> </script>

## ClientLogin

ClientLogin is your basic, straight forward authentication with a users Google account username and password. It's usually used for installed applications, but I included it anyway if you want to use it.

<script src="http://gist.github.com/131009.js"> </script>

And finally here is a list of the functions included in the library:

<script src="http://gist.github.com/131012.js"> </script>
