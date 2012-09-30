---
layout: post
title: "Drag and Drop File Uploads with Javascript"
description: ""
category: 
tags: [drag
	   drop
	   javascript
	   file
	   upload
	   FileList
	   HTML5
	   jQuery]
---
{% include JB/setup %}

The other day I was working on building a file upload interface in Javascript where a user could drag and drop files to upload to a server. I already knew that this was possible using the [drag and drop api](https://developer.mozilla.org/en-US/docs/DragDrop/Drag_and_Drop). Users could drag files from their desktop or other folder to a defined dropzone on the page and it would pull a [FileList](https://developer.mozilla.org/en-US/docs/DOM/FileList) from the drop event. I use Google Chrome as my default browser, so heres what I started with:

```javascript
$(document).ready(function(){
    dz = $('#dropzone');

    dz.on('drop', function(evt){
        evt.preventDefault();
        evt.stopPropagation();
        console.log(evt.originalEvent.dataTransfer.files);     
    });
});
```

First thing to keep in mind is that by default, browsers will try and open a file when you drag it into the window. To prevent that, we need to predent the default action as well as prevent the event from propagating up the DOM tree. After that, we are able to access the FileList. Then I fired up Firefox just to check to make sure it worked across different browsers, knowing sometimes that just because it works in Chrome doesn't mean it will work in other browsers. Upon trying it in Firefox, it loaded up the file in the browser. Turns out, non-Chrome browsers require a bit of an extra step; you need to listen for the 'dragover' event and prevent that from propagating and taking effect. Here's the revised code:

```
$(document).ready(function(){
    dz = $('#dropzone');
    
    dz.on('dragover', function(evt){
        evt.preventDefault();
        evt.stopPropagation();           
    });
    
    dz.on('drop', function(evt){
        evt.preventDefault();
        evt.stopPropagation();
        console.log(evt.originalEvent.dataTransfer.files);     
    });        
});
```

Now our drop event will work in Chrome, Firefox, and Safari. I havent had a chance yet to try it in Internet Explorer, but according to [caniuse.com](http://caniuse.com/#feat=dragndrop) it looks like IE10 with Windows 8 will support drag and drop events. For those curious, here's a [jsfiddle](http://jsfiddle.net/seanmcgary/XWHYK/1/) of the above example.
