---
layout: post
title: "Escape NodeJS Callback Hell with async.js"
description: ""
category:  
tags: [nodejs, server, async, js, javascript, mongodb, database, query, join, parallel, series, processing]
---
{% include JB/setup %}

For those of you that use have used NodeJS at all in conjunction with a database, in today's case, Mongodb, you know of the exact "callback hell" that Im talking about. For the uninitiated to the world of "NoSQL" and document-based stores such as mongodb, mongodb is a database that stores blobs of JSON (documents) and allows you to query them. If you have ever dealt with relational data and rely on the "join" operation in SQL based databases, you wont find that here. Instead, you as the developer are left to manually join your data. A little more effort to get the result set you want, but from what Ive experienced the overall speed increase generally out-weighs this slight inconvenience. Say for example we have a "User" object in our database. Lets say it looks something like this:

```javascript
{
	user_id: 42,
	username: johnsmith,
	full_name: John Smith,
	email: jsmith@gmail.com
}
```

Now, since we're building a social networking site, our user John here is going to have some friends. John is REALLY popular, so lets say he has 1000 friends. Now in our database, we've decided to create a collection mapping John to his friends based on user_id. Now, this is just one way to do it. Since this is a document store, some people might say that Johns friends should be a property of his user object; possibly an array of user_ids. The one drawback I see with doing that is that in order to remove a friend, I have to fetch John's object then iterate over his list of friends until I find the friend that needs to be removed. Then I have to update the list and update the database. By taking a more relational approach, I let the database engine make the query and since I dont need to update a list, once it finds a match, it simply deletes it making the time complexity much more favorable. So heres what the "Friends" document will look like:

```javascript
{
	user_id,
	friends_user_id
}
```

Our objective here is to 1) get John out of the database 2) get his list of friends 3) finally get the "User" object for every friend on his list. This will result in 1002 queries that WE need to make. Lets write our query. In these examples, I'll be using the [mongodb-wrapper](https://github.com/idottv/node-mongodb-wrapper).

```javascript
// db == the mongodb connection object
// query for John's user based on his user_id
db.collection('users');
db.users.findOne({user_id: 42}, function(err, res){
	if(err === null){
		// query for all his friends
		db.collection('friends');
		db.friends.find({user_id: 42}).toArray(function(friend_err, friend_res){
			if(friend_err === null){
				for(i = 0; i < friend_res.length; i++){
					db.collection('users');
					db.users.findOne({user_id: friend_res[i].user_id}, function(err, res){
						// do something with the results
					});
				}
			}
		});
	}
});
```

This is nice and all, but we have hit one inconvenience. How do we know when all of our database queries have completed? WE'd have to either write some type of queue, or some way to keep track of everything. 

# Taming the callback beast with [async.js](https://github.com/caolan/async)

This is the exact reason async.js was built - to take a bunch of asynchronous calls and make them run in series, parallel, in a waterfall (where each next call depends on the result from the previous, etc. It can do all these things and compiles the results for you and returns in a callback when all functions have completed or if one of them returns an error, it will stop immediately and return the error. Async provides a bunch of other useful functions to manipulate collections as well such as map, reduce, forEach, filter, etc. Heres a quick look at the general syntax for bundling together functions to execute in parallel (example take from the async documentation):

```javascript
async.parallel([
    function(callback){
        setTimeout(function(){
            callback(null, 'one');
        }, 200);
    },
    function(callback){
        setTimeout(function(){
            callback(null, 'two');
        }, 100);
    },
],
// optional callback
function(err, results){
    // the results array will equal ['one','two'] even though
    // the second function had a shorter timeout.
});


// an example using an object instead of an array
async.parallel({
    one: function(callback){
        setTimeout(function(){
            callback(null, 1);
        }, 200);
    },
    two: function(callback){
        setTimeout(function(){
            callback(null, 2);
        }, 100);
    },
},
function(err, results) {
    // results is now equals to: {one: 1, two: 2}
});
```
As you can see, the async.parallel function takes either an array of functions to run, returning the results of each one in an array, or you can give it a key-based object of functions and it will return the results in a dictionary, storing the results in the keys you provided. So how does this help us? What if you need to make your requests in a specific order and each request depends on the request before it? Not to worry! Using the same format as ```async.parallel()``` we can run ```async.waterfall()```. When using waterfall, your functions will take two arguments: the callback function and a variable to hold the result of the previous function. The overall result will be the value passed to the callback in the last function of the waterfall.

So now lets take our original example and use async to make it a bit more manageable.


```javascript
// db == the mongodb connection object
// query for John's user based on his user_id
db.collection('users');
db.users.findOne({user_id: 42}, function(err, res){
	if(err === null){
		// query for all his friends
		db.collection('friends');
		
		async.waterfall([
			function(callback){
				db.friends.find({user_id: 42}).toArray(function(friend_err, friend_res){
					if(friend_err === null){
						callback(null, friend_res);
					} else {
						callback(true, 'Error getting friends');
					}
				});
			}, 
			function(friends, callback){
				var friend_queries = [];
				for(i = 0; i < friend_res.length; i++){
					var func = (function(friend){
						return function(cb){
							db.collection('users');
							db.users.findOne({user_id: friend.user_id}, function(err, res){
								if(err !== true){
									cb(null, res);
								} else {
									cb(true, 'Error getting friend data');
								}
							});
						}
					})(friend_res[i]);

					friend_queries.push(func);

				}

				// Make parallel request for friends
				async.parallel(friend_queries, function(err, res){
					if(err !== true){
						callback(null, res);
					} else {
						callback(err, res);
					}
				
				});
				
			}], 
		function(err, res){
			// result of everything
			if(err !== true){
				// res will contain the list of friends with their User object data
			} else {
				// some error occurred
			}
		})
	}
});
```

In this example, I wanted to give an example of how ```async.waterfall`` and ```async.parallel``` worked. Using the waterfall in this case isn't really necessary at all, but just provides an example. The parallel call on the otherhand is a bit more relevent and allows us to grab all of the friends and their associated User object data, collate the results into an array and make a callback with the results when all queries have been completed. Without ```async.parallel``` we would have to devise a way to determine when all database requests had been made, if any errors ocurred, and handle the results. Fortunately, async can do all of that for us.
