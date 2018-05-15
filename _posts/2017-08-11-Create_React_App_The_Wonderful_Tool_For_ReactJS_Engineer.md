---
layout: post
title: create-react-app-cli - The Wonderful Power-Tool For The ReactJS Engineer
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/Create_React_App.jpeg" class="fit image">


If you have struggled to set up a React Application from ground zero, you probably know that starting a React project can be quite painful. There's a lot of tools to know and configure before even writing the first line of your actual application code.

And then came [create-react-app cli](https://github.com/facebookincubator/create-react-app) from the great React Engineering team at facebook (currently led by Dan Abramov). So lets try to know this powerful tool a little bit more.

First thing first, install the create-react-app (CRA) npm package, globally in your machine by running the following command:

``npm install -g create-react-app``
 
Now that we have the CLI installed globally, we can create our first React application:

``npx create-react-app hello-world``
 
After **CRA** installs all the packages, we’ll get an output like the following showing a list of commands we can run:

<img src="/images/fulls/create-react-app-hello-world-terminal.png" height="370" width="600">

And now we’ve got a nice and simple React application running at ``http://localhost:3000/``. Open it in the browser.  It now also display the LAN address in addition to the localhost address so that we can quickly access the app from a mobile device on the same network.

The project codes are pretty standard. Take a look at ``./public/index.html file``


<p data-height="411" data-theme-id="0" data-slug-hash="gxRWQv" data-default-tab="html" data-user="rohanpaul" data-embed-version="2" data-pen-title="gxRWQv" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/gxRWQv/">gxRWQv</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


In the above, we have a ``<div>`` called ``'root'`` and as we can see below the ``src/index.js`` will mount the ``App`` component into the ``root`` ``<div>``


<p data-height="265" data-theme-id="0" data-slug-hash="BdZRxx" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="BdZRxx" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/BdZRxx/">BdZRxx</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


[create-react-app cli](https://github.com/facebookincubator/create-react-app) has a quite exhaustive docuementation. The underpinnings of Create React App are housed in the ``react-scripts`` package. This package contains scripts for building, testing and starting the app. The [bin script](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/bin/react-scripts.js) is responsible for calling the respective scripts. When we do an ``npm install`` in the root folder of our application, npm creates a symlink in the ``node_modules/.bin`` folder to the [react-script.js](https://github.com/facebook/create-react-app/blob/next/packages/react-scripts/bin/react-scripts.js) so that it can be directly used from the CLI.

There are four default commands bundled into create-react-app: ``start``, ``build``, ``test``, and ``eject``. Also, notice that create-react-app now uses Yarn by default instead of just standard NPM. Lets understand them a little bit more

**``npm run build`` or ``yarn build``**

 Basically, what it does is, it’s going to take all of the Javascript code that the browser can’t interpret without any help and turn it into a smaller (“minified”) version that the browser can read and understand. It shrinks the file down as much as it possibly can to reduce the download time and builds the app for production by creating a build directory. This directory will contain the bundled .js file and the index.html file. However, I can't just open the index.html in my browser to render the app. Because, it is supposed to be served with a static file server, as most React apps use client-side routing, and we can’t do that with file:// URLs.


**``npm test`` or ``yarn test``**

“Starts the test runner.” create-react-app now ships with Jest as its test runner. 

**``npm run eject``**

Running ``npm run eject`` executes the [eject.js](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/scripts/eject.js) inside the react-scripts package.

The [documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#npm-run-eject) says - “If you aren’t satisfied with the build tool and configuration choices, you can eject at any time. This command will remove the single build dependency from your project. Instead, it will copy all the configuration files and the transitive dependencies (Webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except eject will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.” 

What this does is it pulls our application out of the context of the create-react-app framework and into a standard webpack build. This removes the react-scripts dependency from our project completely, and copies all scripts and config files right into our project as separate dependencies like Babel, Webpack, etc. The package.json of our application is also rewritten since react-scripts no longer exist & hence, to be be able to run, test & build the app, all of those scripts are put in scripts/ so that they can be referenced in the scripts object in package.json


After ejection there will be additional scripts and config directory and ~10 new files with 50–200 lines of code at each file.

**Changing the development server port**

By default the app will open in port 3000. To change it, create a file called .env in the root of your project and specify port number there. Like

```PORT=3005```

And this is the [source code](https://github.com/facebookincubator/create-react-app/blob/10c1f577da211d65bcc278f94198ef75f00f0277/packages/react-scripts/scripts/start.js#L53) if you are interested.


<p data-height="138" data-theme-id="0" data-slug-hash="brRojj" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="brRojj" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/brRojj/">brRojj</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


Let’s have a deep dive inside the source code by looking at the ./node_modules/react-scripts/scripts directory and [The build.js function](https://github.com/facebookincubator/create-react-app/blob/10c1f577da211d65bcc278f94198ef75f00f0277/packages/react-scripts/scripts/build.js#L114)

<p data-height="812" data-theme-id="0" data-slug-hash="mMwpJZ" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="mMwpJZ" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/mMwpJZ/">mMwpJZ</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

[The build.js](https://github.com/facebookincubator/create-react-app/blob/10c1f577da211d65bcc278f94198ef75f00f0277/packages/react-scripts/scripts/build.js) runs the webpack configuration to compile and create a bundle ready for production.


And then [the start.js](https://github.com/facebookincubator/create-react-app/blob/10c1f577da211d65bcc278f94198ef75f00f0277/packages/react-scripts/scripts/start.js) file starts the Webpack dev server.


<p data-height="1345" data-theme-id="0" data-slug-hash="EvXoWR" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="EvXoWR" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/EvXoWR/">EvXoWR</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>