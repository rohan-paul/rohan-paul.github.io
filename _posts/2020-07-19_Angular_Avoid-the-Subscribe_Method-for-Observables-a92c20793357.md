# Angular: Avoid the Subscribe() Method for Observables

Use AsyncPipe instead

undefined

One of the best-practice principles of Angular is to always use `AsyncPipe` when possible and only use `.subscribe()` when the side effect is an absolute necessity and cannot be avoided.

### **What Is an Observable?**

An `Observable` is an abstraction of an asynchronous stream of data. For example, when we look at an`Observable`, it represents a stream of strings that will be delivered one by one over time. TK ([https://medium.com/isop-nepal/subscribe-vs-async-pipe-in-angular-21bb38f3ee49](https://medium.com/isop-nepal/subscribe-vs-async-pipe-in-angular-21bb38f3ee49))

But when the `Observable` gets modified, how do we display data? This is where we use `.subscribe()`.

### **Subscribe Function**

We pass the Observable around, combining it and saving it to different variables with different combinations of operators, but at the end, an `Observable<T>` is useless on its own. We need a way to “terminate” the `Observable` and extract the type `T` out of it. That is what `.subscribe` is used for: to subscribe to the resulting stream and terminate the observable. TK ([https://kimsereyblog.blogspot.com/2018/05/async-pipe-versus-subscribe-in-angular.html](https://kimsereyblog.blogspot.com/2018/05/async-pipe-versus-subscribe-in-angular.html))

We all recognize this pattern where I dispose of a subscription with the `unsubscribe()` method:

undefined
undefined

With this pattern, I can be sure that my subscription will always be ended and I am safe from a memory leak! Note that we absolutely have to create the subscription during the initialization (`ngOnInit`) of a component, and later when it’s being destroyed, we need to check if the subscription is still live, cancel it, and clear all references to it. TK ([https://medium.com/angular-in-depth/why-you-have-to-unsubscribe-from-observable-92502d5639d0](https://medium.com/angular-in-depth/why-you-have-to-unsubscribe-from-observable-92502d5639d0))

But for most regular situations where I am just doing a simple subscription to get the value of a variable and passing it to the template, this pattern becomes tedious very fast with an increasing number of subscriptions.

For example, see the code below:

undefined
undefined

So here is how I can avoid the subscription completely.

But first, let’s look at the code where it’s a simple consumption of an `Observable` returned by a service function and how to handle this with the `.subscribe()` method

Let’s assume I have a `serviceFunctionReturningObservable()` function that returns me an observable of the following signature. And in the parent `component.ts` file, I want to subscribe to that `Observable` and pass that subscribed data down to `Children` to be consumed.

```
serviceFunctionReturningObservable( flag: Flag ):Observable<boolean> {}
```

#### The suboptimal version of my code (without using an async pipe) with regular .subscribe()

undefined
undefined

And then in the corresponding .html template file, I access the data as follows:

```
[isSomeBooleanVarToPassDownToChildComp]="isSomeBooleanVarToPassDownToChildComp"
```

Now to understand what’s happening above, we need to note that normally to render the result of a `Promise` or an `Observable`, we have to:

1.  Wait for a callback.
2.  Store the result of the callback in a variable.
3.  Bind to that variable in the template. TK ([https://codecraft.tv/courses/angular/pipes/async-pipe/](https://codecraft.tv/courses/angular/pipes/async-pipe/))

So going by that flow, what we are doing above is:

*   Creating a boolean `Observable` that publishes out a boolean value within the `ngOnInit()`.
*   Rendering the value of `isSomeBooleanVarToPassDownToChildComp` in our template.
*   Subscribing to the output of this `Observable` chain and storing the boolean on the property `isSomeBooleanVarToPassDownToChildComp`.

### AsyncPipe

But with `AsyncPipe`, we can use `Promises` and `Observables` directly in our template without having to store the result on an intermediate property or variable. TK ([https://codecraft.tv/courses/angular/pipes/async-pipe/](https://codecraft.tv/courses/angular/pipes/async-pipe/))

`AsyncPipe` accepts as argument an Observable or a Promise, calls `subscribe` or attaches a then-handler, then waits for the asynchronous result before passing it through to the caller. TK ([https://codecraft.tv/courses/angular/pipes/async-pipe/](https://codecraft.tv/courses/angular/pipes/async-pipe/))

So for this case, we can do even better and never actually use `subscribe` by using `AsyncPipe`.

_“Unwraps a value from an asynchronous primitive._

_The async pipe subscribes to an Observable or Promise and returns the latest value it has emitted. When a new value is emitted, the async pipe marks the component to be checked for changes. When the component gets destroyed, the async pipe unsubscribes automatically to avoid potential memory leaks.” —_ [Angular’s documentation](https://angular.io/api/common/AsyncPipe#description)

### **Why Use AsyncPipe?**

*   Because it automatically subscribes and unsubscribes from `Observables` as the component gets instantiated or destroyed, which is a great feature.
*   This is especially important in the case of long-lived `Observables` (e.g. certain `Observables` returned by the router or by `AngularFire`).
*   Also, because it makes our programs easier to read and more declarative with fewer state variables in our component classes.

By using `AsyncPipe`, we don’t need to perform the `subscribe` and store any intermediate data on our component like so:

undefined
undefined

In case I had to pipe something with the initial subscribed data, I would have to do this:

undefined
undefined

We pipe our `Observable` directly to the `AsyncPipe`, it performs a subscription for us, and then it returns whatever gets passed to it. TK ([https://codecraft.tv/courses/angular/pipes/async-pipe/](https://codecraft.tv/courses/angular/pipes/async-pipe/))

By using AsyncPipe, we:

*   Don’t need to call `subscribe` on our `Observable` and store the intermediate data on our component.
*   Don’t need to remember to unsubscribe from the `Observable` when the component is destroyed.
*   Have fewer state variables. TK ([https://codecraft.tv/courses/angular/pipes/async-pipe/](https://codecraft.tv/courses/angular/pipes/async-pipe/))

### Another Example of the Principle

In the case below, `arrayListFromSelector$` is coming from a selector (using the `reselect` package and consuming a `redux-reducer` state).

Initially, I was subscribing to that selector coming from `reselect/redux` with `.subscribe()` as follows.

#### Initial working code with .subscribe()

undefined
undefined

And then passing that subscribed data `arrayListFromSelector` down to the `Child` component where I will, in turn, just pass that data, as it is ultimately to be consumed by an `ng-select` to be fed to a dropdown list.

However, your `Child` component does not need to know anything about the `Observable`.

#### Final refactored code without using .subscribe()

undefined
undefined

And now `arrayListFromSelector` is available in the `Child`, just as a local state variable.

Generally, the pattern of “subscribe and, in the subscription function, copy data into the state of the component” is not a healthy one. It gets you unnecessarily involved in change detection.

### Exceptions

But there are some cases where you should explicitly subscribe to `Observables` in components and directives.

For example, when the `Observable` makes a change to the outside world. Most commonly, this is done by issuing a `POST` (or `DELETE` or `PUT`) through HTTP to a back-end API.

When working with `HttpClient`, we might face a situation where we just can’t use `AsyncPipe` for the `Observable`. To dive deeper, let’s look at some examples:

*   Re-sending requests when parameters are being changed.
*   Receiving data in parallel from several streams.
*   Sending individual requests.
*   Creating, deleting, and updating data.

Also, when the component itself — and not Angular via a template — is consuming the data. It frequently happens when a component opens a modal dialog or sends a message to the user, like a pop-up or a “snackbar.”

In all those cases, the component is the entity that “wants” and “consumes” the `Observable` to actually execute, so it also _should_ be the one to subscribe.

But otherwise, whenever possible:

_“We should always use_ `_AsyncPipe_` _when possible and only use_ `_.subscribe_` _when the side effect is an absolute necessity, as we are safe as long as we stay in the_ `_Observable_`_. The code terminating the_ `_Observable_` _should be the framework (Angular) and the last piece (the UI).”_ TK ([https://kimsereyblog.blogspot.com/2018/05/async-pipe-versus-subscribe-in-angular.html](https://kimsereyblog.blogspot.com/2018/05/async-pipe-versus-subscribe-in-angular.html))

By [Rohan Paul](https://medium.com/@paulrohan) on [July 19, 2020](https://medium.com/p/a92c20793357).

[Canonical link](https://medium.com/@paulrohan/angular-avoiding-subscribe-method-by-replacing-it-with-an-asynpipe-when-possible-a92c20793357)

Exported from [Medium](https://medium.com) on December 12, 2020.