---
layout: post
title: Introduction to Observables in Angular
comments: true
author: Rohan Paul
categories: Angular
---
<img src="/images/fulls/Promises-in-JavaScript.jpeg" class="fit image">


## What Are Observables in Angular?


A global constructor function called Promise exposes all the functionality for promises. The resolver serves two purposes: it receives the resolve and reject arguments, which are functions used to update the promise once the outcome is known, and any error thrown from the resolver is implicitly used to reject the promise. All the logic that was originally done in loadImage is now done inside the resolver. The resolve function is called when the image loads and reject is called if the image cannot be loaded.


