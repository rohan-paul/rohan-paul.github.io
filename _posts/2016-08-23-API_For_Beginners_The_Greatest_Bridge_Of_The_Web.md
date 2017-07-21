---
layout: post
title: API For Beginners - The Greatest Bridge Of The Web
comments: true
author: Rohan Paul
categories: Web development
cover:  "assets/bridge-API.jpg"
---

Like me, ever wondered how the websites we visit "talk" to each other so seamlessly and what is that invisible bridge connecting them so beautifully ?


To understand, we have to delve into the beautiful world of **APIs**, or **Application Programing Interfaces**, that over the past years is making the web so much more free and an absolutely exciting place for developers who are making thousands of amazing apps on top those APIs.


A quick example, when I enter credit card information to make an online purchase, the website sends my credit card information through an API to another application, which confirms that the provided information is correct.


Simply put, APIs let third-party developers utilize the power of web platforms like Facebook, Twitter, Amazon, eBay, GoogleMap and thousands more, to create amazing and powerful apps. By tapping into the Twitter streams, Google Map location data, or YouTube playlists of millions of people, developers are creating incredibly innovative apps. 

An API is (almost always) a set of pre-built methods & functions that are offered by the API providers (usually a company that maintains data or codebase offering specific functionality). In some (or many) cases, these pre-built methods are used to (1) tap into a company's database (Facebook API, Twitter API) or (2) use the company's codebase for some niche functionality (stream.io / jquery API).


The **“Application”** can be many things in the context of **Application Programming Interface**; it can be a single piece of software, a whole web server, the whole app, or just a single component of an app. The API is an architecture that makes it possible for one application to 'consume' capabilities or data from another application. So API is a gateway, defining stable, simplified entry points to another application's logic and data.


API essentially is a documented contract between the developers or consuming application and the API provider's ecosystem of applications, services, website, web pages and content. From the API provider's perspective, think of it as a product, representing something the provider has chosen to share with a particular audience under defined terms and conditions.



***What is REST API***


**REST** is an acronym for Representational State Transfer (The full explanation of REST is beyond of the scope of this article). For now, it is sufficient to state that REST enables developers to access information and resources from another web application using a simple HTTP invocation. A REST API defines a set of functions with which developers can perform requests and receive responses via HTTP protocol such as GET and POST. 


The REST API should specify what it can provide and how to use it, details such as query parameters, response format, request limitations, public use/API keys, method (GET/POST/PUT/DELETE), language support, callback usage, HTTPS support and resource representations should be self-descriptive.


REST API providers offer an API to developers or any API consumers generally, by building a set of dedicated URLs that return pure data responses (in JSON or XML format), instead of the regular visual presentational structure, that I get when surfing a website or URL with a browser.

As an example, imagine that www.ExampleBookSite.com operates a book selling site, on various subjects like Physics, Computer Science etc. Human visitors to the site access books on these different subjects manually, that is, by clicking links.

But www.ExampleBookSite.com also makes avaiable its services to other Web applications by exposing its catalog of books with REST API. So, instead of clicking around, those other Web application obtains information with a simple HTTP invocation. For example, http://www.ExampleBookSite.com/catalog/rest/Physics?format=json returns a list of all Physics books offered by the site in JSON format. And another example, http://www.fishinhole.com/catalog/rest/item?id=343221 returns information about item #343221 in the default format.

And that's what a REST API is for: I can obtain domain-specific data simply by pointing a URL to a specific location.


For a real-life example, Google Maps Geocoding API request takes the following form: (where outputFormat may be either json or xml)

``https://maps.googleapis.com/maps/api/geocode/outputFormat?parameters``


Using the above general form if I want to request to Google Map API the json data for the below Mountain View, California address, put the following URL in any browser..


``https://maps.googleapis.com/maps/api/geocode/json?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA``


And the Google Map API will then return me the below json formatted response for the address


<script src="https://gist.github.com/rohan-paul/ab056e7d7d521b5e90b6747bbfd8bac4.js"></script>



***Twitter API***

Twitter (or Amazon, or eBay for that matter), from a distance seems simple, in reality its a very deep and complex ocean of data stream infrastructure, including the always growing timeline, relationships between users, mentions, notifications, favorites, lists, geolocation, multimedia, places, et al. Meaning plethora of possibilities of building exciting apps based off Twitter's data. And for a third party developer its API is the way to go.


And Twitter has several APIs for various functions – searching, posting, etc. "The [REST APIs](https://dev.twitter.com/rest/public) provide programmatic access to read and write Twitter data. Author a new Tweet, read author profile and follower data, and more. The REST API identifies Twitter applications and users using OAuth; responses are available in JSON".


So, for instance, if I use a 3rd party Twitter app like [Hootsuite](https://hootsuite.com/) to post to Twitter, it’ll use the POST APIs to communicate to Twitter and tell it to post my tweets.


Using Twitter, I can microblog my way to building an entire online network of individuals who share common interests with me. And using the Twitter REST API, I can automate just about everything I can do with Twitter manually. I can programatically access a specific user's timeline, can reply to that user, I can search a user's tweets for information specific to my own interests. I can filter tweets based on certain criteria and display those tweets on my own blog. And that's where automated Twitter accounts or Twitter bots comes into play and by some estimates Twitter has more than 23 million active automated accounts.


Then, unlike Twitter search API, which is part of Twitter’s REST API, [Twitter's Streaming API](https://dev.twitter.com/streaming/overview) gives me tweets in (near) real-time as long as they match my search query (e.g. by keywords, usernames, places, hashtags). Whereas the REST APIs force me to request information, the Streaming APIs feed information to me. It's basically a pipe from Twitter to my application which delivers a continual flow of tweets.



***Instagram API***

[Instagram API](https://www.instagram.com/developer/) is another versatile and powerful platform for developers to build exciting apps on top of it.


Instagram's [User endpoints](https://www.instagram.com/developer/endpoints/users/), for example, represent a variety of REST-based web service URLs for accessing much of Instagram's overall functionality, allowing me to search for users by name, look up basic information about them, and see the media in their newsfeed (people they follow on Instagram) as well as their own media posts and liked media. Some of these features require the user's specific authentication and others can be used by any developer.


A simple use of the API is to post Instagram photos or a photostream, from a specific Instagram account, or from hashtags created by other users on a website. Users can create a gallery of images or have the photostream automatically update as new pictures are added.


Instagram User subscriptions are useful for real time photo update, i.e. if I want to be notified when people who authenticated my app post new media on Instagram. Instagram Realtime API subscription will let me know when there has been an update to my subscribed object. Once I receive a notification, I can make a call to the API.


Most social networks are conducive to live feeding an event. With the Instagram Real-Time API, event organizers can pull photos with event-specific tags and display them onscreen in real time. I can subscribe to events to monitor live activity for users, tags, locations (Instagram's native place IDs) and GPS areas.


While the above are some example of calling APIs from across a network or remote server, developers leverage APIs offered by the local system or device that their application runs on. For example, a smartphone’s current location is discovered by calling the API associated with the phone’s GPS receiver. Similarly all modern desktop web browsers comes with APIs (like Mozilla's [Camera API](https://developer.mozilla.org/en-US/docs/Mozilla/B2G_OS/API/Camera_API) ) to access the host device's storage, audio, camera, microphone and so on.



***Why APIs are such an empowering tool for developers***

By abstracting away the detailed underlying codebase or structures, and only exposing objects and functions the API consuming developer needs, the API reduces greatly the learning curve & code complexity for the consuming developers, tremendously increasing the speed of new app development.


Also, the API endpoints adequately decouple the consuming apps from the API provider's underlying code infrastructure. That means, as long the API endpoints specifications reamins unchanged, the API provider can shift from physical server to virutal server or from PHP stack to JavaScript stack, and the API consuming developer would not need to worry much.


***APIs are liberating the web and acting as an engine of innovation in the new API economy***


The Google Maps API is one single API that spawned a cycle of innovation inspiring thousands of developers build new apps on top of this.


Another fine example, [Twilio](https://www.twilio.com/), the API-as-a-product platform, handles text messaging for companies like Uber, AWS, among many, many others. It connects browsers and backend servers to make requests to the Twilio REST API to dispatch to the person being contacted, delivering a predefined intelligently filtered message, such as the status of an AWS service, from the specified URL.


Its Lookup API retrieves insights into phone number, so users can get information on who is calling them, protect themselves against spam and fraud, or enhance outgoing message efficiency by identifying dormant numbers to reduce undelivered messages.


APIs have been strategic foundation for many global tech powerhouses like Twitter's, Amazon's, Ebay's. The Twitter API, for example, get 10x more traffic than Twitter site.


One of the founding force behind Instagram's fairy-tale success story had been the Facebook API, that brought the huge Facebook platform to Instagram users to broadcast their latest photo-updates to their facebook friends.



Thank you for taking time to read this post and if you enjoyed, follow me on **[Twitter](https://twitter.com/paulr_rohan)** for my future updates and posts.
