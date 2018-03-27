---
layout: post
title: Introduction to Observables in Angular
comments: true
author: Rohan Paul
categories: Angular
---
<img src="/images/fulls/Observable-in-Angular.jpeg" class="fit image">


## What Are Observables in Angular?


Observables in short handles asynchronous processing and events. So, in a way, observables = promises + events.

Observables are lazy (i.e. if I don’t **subscribe** nothing is going to happen.), they can be canceled and I can apply some operators in them (like map, ...). This allows me to handle asynchronous things in a very flexible way. Observables open the continuous channel of communication where multiple values are emitted over time, allowing me to determine the pattern of the data. So, observables are producers of multiple values, “pushing” them to subscribers. You can think of an observable as an array whose items arrive asynchronously over time and Observables help you manage those asynchronous data, such as data coming from a backend service. 


Observables, makes JavaScript as a language implement the [observer design pattern](https://en.wikipedia.org/wiki/Observer_pattern):

*"The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods. It is mainly used to implement distributed event handling systems."*

**Lets go through a quick example by creating a basic observable**

Lets say, I am building a messenger / chat app, and for the discussion in this article, I am taking only two relevant components, one is ``MessageService``, which holds the basic methods for adding, deleting or editing individual messages. And this ``MessageService`` component will be injected into a second component named ``MessageInput`` component.

**So, below is the code in MessageService component.**


<p data-height="615" data-theme-id="0" data-slug-hash="XZQEbQ" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="message.service-injecting-observable.ts" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/XZQEbQ/">message.service-injecting-observable.ts</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


**Let’s understand the above code, step-by-step:**

1. First I am importing the ``observable`` from rxjs and include it in the MessageService component.

2. The ``addMessage`` method returns and ``observable``.

3. With the line ``return this.http.post('http://localhost:3000/message', body, {headers: headers})`` I am setting up an observable. However to note very importantly this line doesn't send the request yet. It only sets up an observable object that holds the request, which allows me to later subscribe to any data that this request will give me back. 

	And the reason this line has not sent the request yet to server, is because I have not yet set up any subscription to this request, which I will do in the next step, in the ``MessageInput`` Component which will use this ``addMessage()`` method defined in MessageService component. The actual user of the observable needs to **subscribe**, because without ``subscribe()`` the observable won't be executed at all. 

4. So, thats why I am using **``return``** in the above line, because, I want to return this Observable here and making sure some other code can subscribe to this observable in other places. But I still want to manipulate the data events in the current method, which I am implementing by chaining the **``map()``** method here.

5. The return value of the callback functions that I am passing to ``.map()`` will be treated as the return data that the whole observable returns in this case.

	So here, I am using ``json()`` method, which is a method offered by the ``response`` object (and I am declaring the property ``response`` to be of type ``Response`` ). This ``json()`` method will extract and convert the data that was attached to the response (to the ``http.post`` request) to a nice javascript object with which I can work, throwing away all the unnecessary stuffs like status code and headers.


6. Its a good practice to use **``map()``** to preproccess my data because many subscribers can consume this result. So instead of adding preprocessing to each client (subscriber) I can prepare single output with single data schema for all.


**And the code in MessageInput Component is as below**

<p data-height="497" data-theme-id="0" data-slug-hash="WMWJXN" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="MessageInputComponent-Observable.ts" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/WMWJXN/">MessageInputComponent-Observable.ts</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


**And here's what is happening in the above code in MessageInputComponent**


1. The MessageInputComponent, injects ``MessageService`` and calls the ``addMessage()`` service method.

2. And because the ``addMessage()`` method returns an Observable of response data, the MessageInput component need to subscribe to ``addMessage`` method's return value. So we call .subscribe() on this Observable which allows us to listen in on any data that is coming through.

3. The subscription callback copies the data fields into the component's object, which is data-bound in the component template for display.


**Some other notes on Observables in general**

1. When I subscribe to an observer, each call of ``subscribe()`` will trigger it’s own independent setup for that given observable. Subscribe calls are not shared among multiple subscribers to the same observable.

2. While, executing the code for observables with ``subscribe()`` method - there are three callback functions available to send data to the subscribers of the observable:

	**``next``** : sends any value such as Numbers, Arrays or objects to it’s subscribers. Observable calls this method whenever the Observable emits an item. This method takes as a parameter the item emitted by the Observable.

	**``error``** : sends a Javascript error or exception. An Observable calls this method to indicate that it has failed to generate the expected data or has encountered some other error. This stops the Observable and it will not make further calls to onNext or onCompleted. The onError method takes as its parameter an indication of what caused the error (sometimes an object like an Exception or Throwable, other times a simple string, depending on the implementation).

	**``complete``** : does not send any value. An Observable calls this method after it has called onNext for the final time, if it has not encountered any errors. 

	Calls of the **``next``** are the most common as they actually deliver the data to it’s subscribers. During observable execution there can be an infinite calls to the **``observer.next()``**, however when ``observer.error()`` or **``observer.complete()``** is called, the execution stops and no more data will be delivered to the subscribers.

3. Since each observable execution is run for every subscriber it’s important to not keep subscriptions open for subscribers that don’t need data anymore, as that would be a memory leak. So, When I subscribe to an observable, I get back a subscription, which represents the ongoing execution. To stop, just call ``unsubscribe()`` to cancel the execution. 
Angular comes with ``Unsubscribe()`` which will unhook the members listening to the Observable stream. Also, there is a way to provide the custom call back method, ``OnUnsubscribe()``, which will be called to manually clean up the things after the subscription has ended. 
We dont need to call ``unsubscribe`` every time we call observables, as observables have the built-in ability to dispose of all their resources whenever the Complete or error method gets called in the application.



