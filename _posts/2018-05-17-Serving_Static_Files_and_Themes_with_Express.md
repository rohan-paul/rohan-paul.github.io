---
layout: post
title: ExpressJS - Serving static files and html themes from local machines @ The Hacking School-6th-Week
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/express-serving-static-files.jpg" class="fit image">

6th week moving at the super-fast pace at **[The Hacking School](http://thehackingschool.com/)** and today had an awesome session with our lead instructor [Prashant](https://github.com/prashanthteja) implementing the technique to server static files and directories from Express server.

<img src="/images/fulls/serving-files-express/4.jpg" class="fit image">

<img src="/images/fulls/serving-files-express/5.jpg" class="fit image">


## Showing list of Files and Dirctories in the local machine

Here's my code using the npm module ``serve-index``

<script src="https://gist.github.com/rohan-paul/fdea2e0117a99e39c9898840104c3bf4.js"></script>

In the above, I am using the npm packages [serverIndex](https://github.com/expressjs/serve-index) which returns a middlware that serves an index of the directory in the given path. In the code above, I am completely configuring the server with serverIndex's methods without using Express. And the path is based off the ``req.url`` value, so a ``req.url`` of ``/some/dir`` with a path of ``public`` will look at 'public/some/dir'.

However, if I am using **Express**, I can set the URL "base" with ``app.use``. Below is the implementation using **Express**


<script src="https://gist.github.com/rohan-paul/3cc0204a4f64fc79b6d19365caa9b196.js"></script>


## Serveing a static html theme from local machine

Here I am going to serve a static theme that I downloaded from [https://templated.co/hielo](https://templated.co/hielo) so as soon as I start my express sever the entry root will launch the site. 

Here's the code 

<script src="https://gist.github.com/rohan-paul/9b2d504d2dedcc590c46e4dbbc1dc5b3.js"></script>

And thats it, as soon as I fire up my server, a full blown beautiful website is up and running in my Express Server.

The entry point to my app is server.js and after I run ``node server.js`` the express server will start and in my code the ``app.use`` middleware will chain itself with another middleware ``express.static`` to sever the files in the directory ``public/templated-hielo``

Express middlewares processes the incoming requests before handling them down to the routes. Each middleware layer is essentially adding a function that specifically handles something to your flow through the middleware. 


I am using node's ``path`` module to join all given path segments together using the platform specific separator as a delimiter, then normalizes the resulting path. Normalizing in that sense that, Windows does paths with backslashes where Unix does paths with forward slashes. ``path.join()`` will normalize these to use the correct slash. ``path.join`` will also take care of unneccessary delimiters, that may occur if the given paths come from unknown sources (eg. user input, 3rd party APIs etc.). So ``path.join('a/','b')``  ``path.join('a/','/b')``, ``path.join('a','b')`` and ``path.join('a','/b')`` will all give ``a/b``.

And here's the full source code in my [github](https://github.com/rohan-paul/Hacking-School-Full-Stack-Bootcamp-Projects/tree/master/nodejs-snippets-parctice/server-static-theme-with-express)

<img src="/images/fulls/serving-files-express/1_lzn.jpg" class="fit image">

<img src="/images/fulls/serving-files-express/2_lzn.jpg" class="fit image">

<img src="/images/fulls/serving-files-express/3_lzn.jpg" class="fit image">