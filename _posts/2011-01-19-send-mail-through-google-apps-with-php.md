---
layout: post
title: "Send mail through Google Apps with PHP"
description: ""
category: 
tags: [PHP,
       email,
       mail,
       SMTP,
       Google,
       google apps,
       gmail,
       PEAR]
---
{% include JB/setup %}

Recently while I was working on a site that Im creating, I needed a way to easily send email out to users. Like a lot of people that have domain names and dont want to run their own mail server, I have decided to let [Google Apps](http://google.com/a) handle all of my email and app related needs. Now way you dont have a mail server and dont want to maintain one. Maybe you dont know how or just dont want to deal with maintaining such a service. Well as it turns out, you can use Google Apps as your mail server. In this example, Im using PHP to send out a message using the SMTP server and an account that Ive setup on my Google Apps domain (one that is not my primary, admin account).

First we have to install some libraries through [PEAR](http://pear.php.net)

<script src="https://gist.github.com/786914.js"> </script>

Now that we have our two dependancies installed, we can write our code to send our mail. Basically what this does, is it connects to Gmail's (or Google Apps) SMTP server and sends a message. This is pretty much the same thing that happens when you send an email when using a desktop client like Thunderbird or Apple Mail. All you do is supply it with Google's SMTP server, port, and your username and password. NOTE: if you are using Google Apps, the username is going to be *your-account-name*@*your-domain.com*.

<script src="https://gist.github.com/786903.js"> </script>

And voila! You can now send mail through Google!
