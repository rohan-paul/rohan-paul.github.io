---
layout: post
title: Introduction to Observables in Angular
comments: true
author: Rohan Paul
categories: Angular
---
<img src="/images/fulls/Observable-in-Angular.jpeg" class="fit image">


**What Are Observables?**


Observables in short handles asynchronous processing and events. So, in a way, observables = promises + events.

Observables are lazy, they can be canceled and I can apply some operators in them (like map, ...). This allows me to handle asynchronous things in a very flexible way.

Observables open the continuous channel of communication where multiple values are emitted over time. This allows us to determine the pattern of the data.

**Lets go throug a quick example by creating a basic observable**

Here, I am building a messenger / chat app, and for this article, I am taking 2 components, one is MessageService (which holds the basic methods for adding, deleting or editing of individual messages) and this MessageService component will be injeted and into the other is component named MessageInput component.

**So, below is the code in MessageService component.**

<p data-height="615" data-theme-id="0" data-slug-hash="XZQEbQ" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="message.service-injecting-observable.ts" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/XZQEbQ/">message.service-injecting-observable.ts</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

**Let’s understand the above code, step-by-step:**

1. First I am importing the observable from rxjs and include it in the MessageService component.

2. With the line ``return this.http.post('http://localhost:3000/message', body, {headers: headers})`` I am setting up an observable. However to note very importantly this line doesn't send the request yet. It only sets up an observable object that holds the request, which allows me to later subscribe to any data that this request will give me back.

And the reason this line has not sent the request yet to server, is because I have not yet set up any subscription to this request, which I will do in the next step, the MessageInput Component which will use this ``addMessage()`` defined in MessageService component. The actual user of the observable needs to subscribe(), because without subscribe() the observable won't be executed at all. 

3. So, thats why I am using **``return``** in the above line, because, I want to return this Observable here and making sure some other code can subscribe to this observable in other places. But I still want to manipulate the data events in the current method, which I am implementing using the **``map()``** method.

4. Its a good practice to use **``map()``** to preproccess my data because many subscribers can consume this results. So instead of adding preprocessing to each client (subscriber) I can prepare single output with single data schema for all.


**And the code in MessageInput Component is as below**

<p data-height="497" data-theme-id="0" data-slug-hash="WMWJXN" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="MessageInputComponent-Observable.ts" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/WMWJXN/">MessageInputComponent-Observable.ts</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


**And here's what is happening in the above code in MessageInputComponent**


1. The MessageInputComponent, injects MessageService and calls the addMessage() service method.

2. And because the addMessage() method returns an Observable of response data, the MessageInput component subscribes to addMessage method's return value. In other words, we call .subscribe() on this Observable which allows us to listen in on any data that is coming through.

3. The subscription callback copies the data fields into the component's object, which is data-bound in the component template for display.



4. With the subscription, we used three callbacks which already accept the data emitted by the observable. The second callback given is the Error call.
