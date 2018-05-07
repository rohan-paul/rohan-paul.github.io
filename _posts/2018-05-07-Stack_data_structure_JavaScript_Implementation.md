---
layout: post
title: Stack Data Structure JavaScript Implementation-Hacking School-4th-Week
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/stack-data-structure.jpg" class="fit image">

One of the most commonly used data structures in web development are stacks. Couple of enlightening examples: the 'undo' operation of a text editor uses a stack to organize data. The browser keeps a stack of the web pages that I have visited in that window. When I press the Back button, the browser peeks element from the top of the stack (list of webpages) and takes me back to the previous page.

The stack follows a **last-in, first-out (LIFO)** data structure and because of which any element that is not currently at the top of the stack cannot be accessed directly (like we do in an array with index no position). To get to an element at the bottom of the stack, I have to dispose of all the elements above it first.


JavaScript already supports a stack out of the box using an array. We can easily push items onto the stack and pop them off. Keep in mind that as you pop values off the stack they'll be in reverse order that you put them on.

Here's a simple class implementation of the Stack class.

