---
layout: post
title: "Usability of photo sharing sites"
description: ""
category: 
tags: [
	photography,
	sharing,
	social,
	network,
	flickr,
	smugmug,
	interface,
	design,
	experience,
	UX,
	UI,
	communities]
---
{% include JB/setup %}

Recently Ive noticed that photo sharing sites (eg. flickr, Smugmug, etc) have rather poor user interfaces and user experience. UIs seem to have become overly complex, pushing rudimentary features out of the way to places that are not immediately accessible or take a bit of work to find. When Im using a photo sharing site, there are a couple of really basic features that I believe should be very prominent upon logging in.

## The "upload" button should be VERY easy to find.

When I first log into flickr, it is not immediately obvious how I can upload photos or videos. 

![Flickr](/assets/flickr_account_page.png)

When I first see this page, I immediately look at the top row where the various navigation items and menus are. Theres nothing at the root level that links to an upload page. However, under the "You" dropdown is an upload link. For a site that relies on users uploading photos, Id think that they would make it a single click away and very obvious. Instead, they nest one of the most import actions in a dropdown menu. As you make it down the page, you'll realize that there IS an upload link on the main page, however its the same style as their section headers and doesn't immediately stand out. This really should be styled more like a call to action so that it stands out better. Smugmug, in comparison, has an upload link on their top row of navigation, but they require you to first create a gallery if you dont have one, or pick a gallery to upload to if you have already created one. This is definitely a step in the right direction. I'll get to my issues with their gallery structure in a little bit.

![Smugmug](/assets/smugmug_account_page.png)

## Galleries, sets, and categories oh my!

For those of you that are old enough, think back ten years or so. Chances are your mother, grandmother, or some other family member has a closet full of photo albums of photos from your childhood, or even their own childhood. Photo albums are the most basic and rudimentary method for organizing photos. Have a bunch of photos that happend all at once? Maybe a vacation, birthday party, or other special event? Put them all in one place, like a photo album, so that you can find them later! Even Facebook has this down. You can upload photos and then organize them into albums. Its simple and easy. Flickr and Smugmug on the other hand, make it a bit more difficult. Flickr has the notion of "sets". Sets are essentially the same idea as an album; you name the set and select photos to put in the set. Beyond that its really easy to get lost. Organizing photos into sets is relatively simple; select "your sets" from the "organize" dropdown. Though, upon doing this you are brought to an entirely different user interface than you were just on. The entire root site navigation has disappeared and you are shown a pseudo full-screen page. Adding photos is pretty easy - just drag and drop from your "photostream" on the bottom. The flow of this organize process could be better handled as it seems like they are trying to cram too many features into one page and have thus made it a bit complex to navigate. As a note, my mother who is not the greatest with computers, has never been able to figure out how to use this particular interface on flickr.

Well how about smugmug; How does it stack up against flickr? The first thing that drives me nuts is that you HAVE to create a gallery (equivelant of an album) in order to upload any photos at all. Flickr allows its users to simply upload photos then organize later. Smugmug also doesnt allow you to include one photo in multiple galleries. There is absolutely no way to organize your photos a la flickr. Everything MUST go into a gallery, and one gallery only, and only into that gallery by uploading something to it. Smugmug also has categories. An extra level of hierarchy and organization seems like a good idea, but their flow is very limiting, much like their flow for adding photos to albums. 


## Help help, my photos are being help hostage!

One large point of contention on the internet right now is over reclaiming your own data from a website that you are using. Google+ does a great job with addressing this by allowing users to "liberate" their data by download a zip archive of it. Recently, my flickr pro subscription ran out. Currently I have a few hundred photos hosted through them. However, when your pro subscription runs out, flickr essentially holds your photos hostage allowing you to only have access to the 200 most recent photos. What if I didnt have a backup of my photos? (Yes, stupid I know). The only way to get them back would be to pay $25 to upgrade just to download them all. Flickr also doesnt provide a batch download feature to reclaim all of your uploaded photos. Smugmug is a little bit different. They dont follow a freemium model like flickr. They are purely a subscription service that you pay yearly. So once your subscription runs out, you have to renew it to get back to your photos. Then again, Smugmug targets professional photographers that are using them as a showcase of their work as well as for white label printing. Smugmug, as far as I can tell, also doesnt have a way to batch download photos that you have uploaded.

## Bringing something new to the table

Both flickr and Smugmug have features that are good and features that are not so good. And if they were combined and implemented a little bit differently than you would have one hell of a site. So I am going to attempt to do just that; take features and ideas from both and improve upon them. The internet is a much different place now than it was when flickr and smugmug first launched in the early 2000's. There is now a larger focus on building social communities and applications that are incredibly easy to use. Here is how I would do it:


### Freemium Model

This app is going to be a "pay for what you use" type of deal. There will be a pricing model with a free tier where the amount you pay is based on the amount of space that you are consuming. The free tier will offer a certain amount of space rather than a limit on the number of photos. If you want to compress your photos down to a few kilobytes and upload a few hunderd, go for it! However, if you want to upload photos at their full resolution that are a few megabytes a piece, then you may want to look into one of the paid tiers. This way, people that are into "casual" photography have a place to upload and share photos for free or cheap, and professionals have an affordable way to host their photos as well. Pricing will be focused around space consumed, and not necessarily features available to you. There might be a point where some features appear that are more geared toward professionals and might be offered to paying users only, but for the most part everyone will be on an equal playing field as far as features are concerned


### Focus on the basics

As I explained above, doing the simple things uploading and organizing photos and albums has gotten rather difficult. In this application, viewing photos and albums, uploading photos, and organizing your photos will be a very primary focus. The interface is designed in a way that even my own mother will be able to use it without having to call me up to walk her through the process. I figure if she can do it without help, then most everyone else should be able to as well.


### Building communities

Whats use would a hosting site be if you couldnt interact with people and talk about your love for photography? Users will have the option of enabling commenting on albums and photos. Flickr has some very large communities because of their commenting system, but there is also a high level of spam comments as well. Users will be able to monitor and moderate comment threads on their own photos and albums to hopefully prevent spam from coming up. Users will also be able to favorite photos and albums as well as follow other users. If two users follow each other, they will be classified as friends. On your dashboard, you will see activity on the things you have followed; when a new photo is added to an album, when comments are made, when a user creates a new album or uploads some photos. User activity and engagement will play a key role in this new application.


### Liberate your data

Users will be able to download a zip file containing all of the photos that they have uploaded. 'nuff said.

