# YMC Analytics API 

![Mou icon](http://developer.ymcgames.com/images/ymc-logo.png)

## Overview

**YMC Analytics** can receive **Events** from your application, and Events are represented in your requests by Base64-encoded JSON objects, provided to the API as a data query parameter to an endpoint URL.

## Events
Events describe things that happen in your game, usually as the result of user interaction; for example, when a player conquered a level, or purchased some equipment, you can send an event to record the incident.

### Common Attributes
All Events are of standard JSON objects and should have the following attributes at least (thus a "minimal" Event):
	
	{
    "event" : <Event Name>,
    "properties" : {
        "distinct_id" : <UUID>,
        "YA0ver" : <SDK Version>,
        "YA0token" : <YMC ID>,
        "time" : <Timestamp>,
    }
	}

### First Launching
An Event of type "YA0birth" should be posted to YMCA server for tracking first launching of the app. a full example is as following:

	{
	"event": "YA0birth",
	"properties": {
    "YA0ver": "2.0.2",
    "YA0token": "8416e32af87f11e284c212313b0ace15",
    "distinct_id": "942ccee43679a35a4d9ed1a96118641e",
    "time": "2014-02-11T10:14:29.000Z"
    }
	}
	
### App goes to the forground
An Event of type "YA0start" should be posted to YMCA server whenever the app appears (including recover from the background).
On top of the minimal attributes, a YA0birth event should have the **Launch Count** property recording the times of the app going to the foreground. a full example is as following:

	{
	 "event": "YA0start",
	 "properties": {
    "YA0ver": "2.0.2",
    "count": "1",
    "YA0token": "8416e32af87f11e284c212313b0ace15",
    "distinct_id": "942ccee43679a35a4d9ed1a96118641e",
    "time": "2014-02-11T10:14:35.000Z"
    }
	}
	
### App disappears
An Event of type "YA0session" should be posted to YMCA server whenever the app disappears (including pause to the background or shutting down).

On top of the minimal attributes, a YA0session event should have the **start** and **end** properties indicating the time interval of the app's appearance. a full example is as following:

	{
	"event": "YA0session",
	"properties": {
    "YA0ver": "2.0.2",
    "start": "63527710703",
    "end": "63527710704",
    "length": "1",
    "YA0token": "8416e32af87f11e284c212313b0ace15",
    "distinct_id": "942ccee43679a35a4d9ed1a96118641e",
    "time": "2014-02-11T10:14:36.000Z"}
    }
	
### Purchasement Event
An Event of type "YA0charge" should be posted to YMCA server for tracking any purchasement made by the game player.
On top of the minimal attributes, a purchasement event should have the **currency type** and **amount** properties. a full example is as following:

	{
    "event" : "YA0charge",
    "properties" : { 
        "distinct_id" : "449071b3-9535-4989-ad08-261c94e332fa",
        "amount" : 0.01,
        "currency" : "USD",
        "YA0ver" : "0.2.1",
        "YA0token" : "cb830e90be8d11e2bf88080027dde1ce",
        "time" : ISODate("2013-07-08T19:13:05Z"),
     }
	}
	
### Custom Event
Sometimes you might want to track other specific things happened in your game, such as when the player passed one level, and YMCA allows developers adding extra properties to the "minimal" Event and logging it eventually. Here comes an example of "Level-up" Event:

	{
    "event" : "Level-Up",
    "properties" : { 
        "distinct_id" : "449071b3-9535-4989-ad08-261c94e332fa",
        "level" : 10,
        "killed" : 20,
        "YA0ver" : "0.2.1",
        "YA0token" : "cb830e90be8d11e2bf88080027dde1ce",
        "time" : ISODate("2013-07-08T19:13:05Z"),
     }
	}
	
## Track events
Events are tracked at endpoint 
	
	http://yax.ymcnetwork.com/track
	
This URL tracks an event with YMC Analytics:

	http://api.mixpanel.com/track/?data=[BASE64 encoded JSON data]
	
The request will return an HTTP response with body "YAP T1" if the track call is successful.

### Example of tracking an IAP:
1. Construct the JSON object:

		{ "event": "YA0charge",
		  "properties": {
    	  "YA0USD": 0.99,
    	  "YA0token": "8416e32af87f11e284c212313b0ace15",
    	  "YA0ver": "2.0.0",
    	  "amount": "0.99",
    	  "currency": "CAD",
    	  "distinct_id": "2b20a64859506aedc03a16c193f28640",
    	  "time": "2014-01-24T19:39:07.000Z",
    	}
		}
		
2. Encode the above JSON string in BASE64  		
3. POST the encoded data:

	`http://api.mixpanel.com/track/?data=[BASE64 encoded JSON data]`

