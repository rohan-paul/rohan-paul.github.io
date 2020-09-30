---
title: Understanding Proxy — Metaprogramming in JavaScript
description: >-
  8th week running @ The Hacking School and one of the thing we are learning
  this week is metaprograming, and Proxy is a major concept within…
date: "2018-05-31T10:18:24.831Z"
categories: []
keywords: []
slug: /@paulrohan/understanding-proxy-metaprogramming-in-javascript-b1c727b747f2
---

![](../images/fulls/1__SQm5ZwCZdYRLXwz9em5xLA.jpeg)

8th week running @ [**The Hacking School**](http://thehackingschool.com/) and one of the thing we are learning this week is metaprograming, and **Proxy** is a major concept within it.

The Oxford English Dictionary defines a proxy as **_the authority to represent someone else_.** That is exactly what a Proxy in JavaScript is. The Proxy object is used to define custom behaviour (by intercepting or coming as a Proxy in between) for fundamental operations (e.g. property lookup, assignment, enumeration, function invocation, etc).

In a nutshell, I can use a Proxy to determine behaviour whenever the properties of a target object are accessed. A handler object can be used to configure traps for your Proxy, as we’ll see in a bit.

Users of the Proxy don’t have direct access to the original object, which makes it a good tool for encapsulation, validation, access control, and a whole bunch of other things. Keep on reading to see some interesting examples.

**There are 3 key terms we need to define before we proceed:**

**handler** — the placeholder object which contains the trap(s). This is an object whose properties are functions which define the behavior of the proxy when an operation is performed on it.

**traps **— the method(s) that provide property access. Traps allow you to intercept interactions with targetObject in different ways, as long as those interactions happen through proxy.

This has some similarities to the concept of traps in operating systems. In computing and operating systems, a trap (also known as an exception or a fault), is typically a type of synchronous interrupt caused by an exceptional condition (e.g., breakpoint, division by zero, invalid memory access).

Some examples of traps — accessing a property wtih `get` trap. Setting a value to a property with `set` trap. Deleting a property from an object with `deleteProperty` trap. There are many traps. See all of them [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy). Also in your IDE / Text Editor **right-click on Proxy and select Go To Definition** and it will show all methods for Proxy as below ..

**target **— object which the proxy virtualizes. It can be any sort of object, including a native array, a function or even another proxy, which will be wrapped with Proxy.

Proxies enable us to intercept and customize operations performed on objects.

**Example-1** — Here I am creating a custom behavior for fundamental operations of simple property access.

Say, I have a simple object as below

**const originalTargetObject = { prop1: ‘Awesome1’, prop2: ‘Awesome2’}**

And the default way access the value of prop1 for **‘originalTargetObject**’ is to do as below.

**console.log(originalTargetObject.prop1);**

Here, the console.log() statement performs a get operation on the object ‘originalTargetObject’ to get the value of the property ‘prop1’.

But now we will perform the get operation with Proxy

Few points to note here.

1.  When we asking forproxiedObject.key1 in our example, our handler.get trap will be called.

2\. The **get trap receives three parameters: target, property, receiver**.

**The target** is the original object for which we created the proxy.

**The property** is the name of the property that is being accessed.

**The receiver** is either the proxy or an object that inherits from the proxy.

So, in the above Proxy comes in between as an interceptor to access the key of the object. Without using the Proxy, I would access the key normally by just doing **originalTargetObject.key1 . That is, my program will directly as the Object to give the value of property and the Object obliges by giving the value back.**

But with Proxy the situation is like below

![](../images/fulls/1__Zf__XhxF7tQdDK7__lSdryeA.png)

Now, in the above code, when I want to get a non-existent property from the originalTargetObject (i.e. with **proxiedObject.prop3**) — I will get ‘undefined’ . So lets modify the handler to handle non-existent properties.

**For operations which do not have any traps defined, they are passed to the target normally as if the proxy did not exist at all.**

**As a more general concept “Proxy Pattern” comes to control access to a resource. By doing so it can solve several potential issues:**

1.  prevent incorrect or malicious use of the resource
2.  prevent/control access to a resource that is too expensive to create
3.  Giving warnings or preventing certain operations
4.  Instantly revoking access to sensitive data

In Javascript Patterns book, Stoyan Stefanov describes this pattern as so:

> _“In the proxy pattern, one object acts as an interface to another object. The proxy sits between the client of an object and the object itself and protects the access to that object.”_

This pattern might look like an overhead but it’s useful for peformance purposes. The proxy plays a guardian role for the object (also called “real subject”) and tries to make this object do as little work as possible.

The proxy pattern is useful when the real subject does something expensive. For example in web applications, one of the most expensive operations is network requests, so it makes sense to combine HTTP requests as much as possible by routing them through a Proxy and only then hitting the real subject.

**Another use of Proxy — Creating truly private properties in JavaScript.** Meaning only accessible from within the class as opposite to public (accessible internally or externally).

In JavaScript, typically we use underscores (or other characters) before and/or after a property to signal that it’s for internal use only. But that doesn’t stop someone from peeking at or changing it anyway.

This is the case below, where we have an apiKey we want to be accessible to the methods in the api object, but we really don’t want to be accessible outside of it.

However the below codes will work just so as expected, getting or setting the value.

With ES6 proxies, I can achieve complete privacy in JavaScript, as below.

First, I use a proxy to intercept requests to certain properties and then restrict them or just return undefined / error.

**A note on using Reflect in the above code**

Reflect is a built-in object that provides methods for interceptable JavaScript operations. We are using Reflect because it gives us a programmatic way of manipulating an object. It’s not much different from obj.prob = ‘newly Assigned value’ type of property assignment.

When using Proxy objects to wrap existing objects, we typically intercept an operation, do something, and then to **“do the default stuff”**, which is typically to apply the intercepted operation to the wrapped object. For example, say I want to simply log all property accesses to an object obj:

The `Reflect` and `Proxy` APIs were designed in tandem, such that for each `Proxy` trap, there exists a corresponding method on `Reflect` that "does the default thing". Hence, whenever we find ourselves wanting to "do the default" thing inside a Proxy handler, the correct thing to do is to always call the corresponding method in the `Reflect` object.

Unfortunately, it’s not possible to polyfill or transpile ES6 proxy code using tools such as [Babel](https://babeljs.io/), because proxies are powerful and have no ES5 equivalent.
