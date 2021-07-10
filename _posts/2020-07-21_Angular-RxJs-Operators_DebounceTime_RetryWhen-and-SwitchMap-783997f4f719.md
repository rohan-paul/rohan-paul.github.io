# Angular RxJs Operators: DebounceTime, RetryWhen and SwitchMap

Exploring three of the most awesome RxJS operators to add beauty and efficiency to your code

![](https://cdn-images-1.medium.com/max/1200/1*jhrN0nMsgpjdv6OFPrxRcA.jpeg)

Before jumping into rx-js operators we have to understand two of the most fundamental concepts which are **observable and pipe in rx-js**

Now what is an Observable

Usually, an observable is a collection of values that may be updated over time. This means that this collection can be observed by an observer — a collection of callbacks — which will be invoked whenever the data is changed.

To simplify this idea, you can think of it as a very powerful “Event Bus.” You can register to events, deregister from events, and trigger events, quite the same as you would on a regular Event Bus. However, at any step in these operations, you can define an operator function in order to combine, filter, and create new events at any time. This is actually the operation of looking at a stream of data (or better: observing) and performing actions to alter that stream to the application’s needs.

Now lets see some super basic codes around Observable to get its basic nature

_An observable is Lazy, Observables will only execute upon subscribe. And if you don’t subscribe it will not start. As long as the observer didn’t subscribe it, it doesn’t emit any value. So I used the subscribe() function to subscribe to this Observable_

![](https://cdn-images-1.medium.com/max/800/1*I-2sEVzKH8WSxMbmlHiVUA.png)
undefined

Observables will only execute upon subscribe, and will re-execute every time they are subscribed.

![](https://cdn-images-1.medium.com/max/800/1*dto3P280l792kNSp1sAsaA.png)
undefined

Subjects are observables, that are also observers. They will send updates to all subscriptions, and allow updating from external sources.

![](https://cdn-images-1.medium.com/max/800/1*P-pUMYZBoZwtruARP_W2kA.png)
undefined

The pipe method.

According to original Documentation: The pipe operator is that function take observables as an input and it returns another observable .previous observable stays unmodified.

`pipe(...fns: UnaryFunction<any, any>[]): UnaryFunction<any, any>`

Pipes let you combine multiple functions into a single function. The pipe() function takes as its arguments the functions you want to combine, and returns a new function that, when executed, runs the composed functions in sequence.

The purpose of the pipe() function is to lump together all the functions that take, and return observable. It takes an observable initially, then that observable is used throughout the pipe() function by each function used inside of it.

First function takes the observable, processes it, modify its value, and passes to the next function, then next function takes the output observable of the first function, processes it, and passes to the next function, then it goes on until all the functions inside of pipe() function use that observable, finally you have the processed observable. At the end you can execute the observable with subscribe() function to extract the value out of it. Remember, the values in the original observable are not changed.

```
Observable.pipe(function1(), function2(), function3(), function4())
```

And remember You need to call subscribe on the observable to execute the request.

#### `Most simply pipe()` Returns an Observable

To demonstrate, the code below shows that `pipe` returns its own observable:

const observable = fromEvent()

console.log(observable === observable.pipe()) _//true_

#### So What’s an `Operator`?

An `operator` is a function you pass into a `pipe`. And `pipe` returns its own observable. So let’s think about what that means:

1.  An `operator` has the original observable as the first argument
2.  An `operator` returns an observable

So, Operators are simply methods that you can use on Observables (and Subjects) that allow you to change the original observable in some manner and return a new observable. These operators do not change the existing Observable; they simply modify it and return a new one. Operators are known as pure functions, which are functions that do not modify the variables outside of its scope.

#### Now a little deeper dive into pipe()

First note the difference between concepts pipe in the context of Angular and RxJS

We have pipes concept in Angular and pipe() function in RxJS.

1.  Pipes in Angular: A pipe takes in data as input and transforms it to the desired output [https://angular.io/guide/pipes](https://angular.io/guide/pipes)
2.  pipe() function in RxJS: You can use pipes to link operators together. Pipes let you combine multiple functions into a single function.

The pipe() function takes as its arguments the functions you want to combine, and returns a new function that, when executed, runs the composed functions in sequence. [https://angular.io/guide/rx-library](https://angular.io/guide/rx-library) (search for pipes in this URL, you can find the same)

pipe() is a function/method that is used to chain multiple RxJS operators while map() and filter() are operators that operate and transform the values of an Observable (sequence of values). They are similar to the map() and filter() methods of JavaScript arrays.

What does this pipe() function exactly mean in this case?

```
return (  this.http.get <  Hero >  url.pipe(    tap((_) => this.log(`fetched hero id=${id}`)),    catchError(this.handleError < Hero > `getHero id=${id}`),  ))
```

The **pipe()** in above example is the pipe() method of RxJS 5.5 (RxJS is the default for all Angular apps).

The pipe() function takes as its arguments the functions you want to combine, and returns a new function that, when executed, runs the composed functions in sequence.

**tap()** — RxJS tap operator will look at the Observable value and do something with that value. In other words, after a successful API request, the tap() operator will do any function you want it to perform with the response. In the example, it will just log that string.

**catchError()** — catchError does exactly the same thing but with error response. If you want to throw an error or want to call some function if you get an error, you can do it here. In the example, it will call handleError() and inside that, it will just log that string.

_RxJS Operators are functions that build on the observables foundation to enable sophisticated manipulation of collections._

You can use pipes to link operators together. Pipes let you combine multiple functions into a single function.

It decouples the streaming operations (map, filter, reduce…) from the core functionality(subscribing, piping). By piping operations instead of chaining, it doesn’t pollute the prototype of Observable making it easier to do tree shaking.

### debounceTime operator

> Emits a value from the source Observable only after a particular time span has passed without another source emission.

Its function signature is

```
debounceTime<T>(dueTime: number, scheduler: SchedulerLike = async): MonoTypeOperatorFunction<T>
```

#### Parameters

dueTime — The timeout duration in milliseconds (or the time unit determined internally by the optional `scheduler`) for the window of time required to wait for emission silence before emitting the most recent source value.

scheduler — Optional. Default is `async`.

The `[SchedulerLike](https://rxjs-dev.firebaseapp.com/api/index/interface/SchedulerLike)` to use for managing the timers that handle the timeout for each value.

`debounceTime` emits a value from the source Observable only after a particular time span has passed without another source emission.

_It’s like_ [_delay_](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-delay)_, but passes only the most recent value from each burst of emissions._

`debounceTime` delays values emitted by the source Observable but drops previous pending delayed emissions if a new value arrives from the source Observable.

This operator does keeps track of the most recent value from the source Observable, and emits that only when \`**dueTime**\` enough time has passed without any other value appearing on the source Observable.

If a new value appears before \`**dueTime**\` silence occurs, the previous value will be dropped and will not be emitted on the output Observable.

#### Now the classic use case of debounceTime() — Let’s say we wanted to debounce search queries and dismiss consecutive duplicates

This is a reusable reusableSearch that can be reused in multiple components which have a search box. A benefit of having a reusableSearch is that we can change search behaviour in a single place.

Here’s the full implementation

![](https://cdn-images-1.medium.com/max/800/1*GUcdHkcrgG2eCqjPAZtJtQ.png)
undefined

#### Now lets see another example of a very special implementation of debounceTime

I have a web worker that crunches data when a message is received from the main thread. I’ve created a hot observable of those messages (using fromEvent). While the worker is crunching numbers, several messages will have come in telling the worker to re-crunch, I wanted to disregard all but the latest of those.

I have a web worker that crunches data when a message is received from the main thread. I’ve created a hot observable of those messages (using fromEvent). While the worker is crunching numbers, several messages will have come in telling the worker to re-crunch, I wanted to disregard all but the latest of those.

Here is the simple Solution

```
messages$.pipe(debounceTime(0))
```

**The above approach presumes though that these messages come synchronously**.

### retryWhen() operator

Before going deep into this operator, first note that one of the key difference between Javascript Promise and Observable is that Promises are not retry-able but Observables are indeed. And one of the way we implement this retry-ability is with this retryWhen() rx-js operator.

Common use case of retryWhen() operator includes: retrying after a certain period of time or retrying maximum n-times with time intervals.

`retryWhen` gets into play _only_ when the source observable produces an error. It then prevents error from propagating further and resubscribes to the source. If the source produces a non-error result (whether on first subscription or on a retry), `retryWhen` is passed over and is not involved.

`retryWhen(fn)` maintains an inner subscription for the observable resulted from calling the provided function `fn`. The `retryWhen`'s source will be **re-subscribed** only when the inner observable **emits a value**.

I want to retry an api call 10 times (waiting one second since it fails until next execution) and if this 10 times it fails, then I will execute a function, this is my approach:

![](https://cdn-images-1.medium.com/max/800/1*H-5xW70x2I9r_0t5TfNoIQ.png)
undefined

A few words on `errors` argument passed to **retryWhen()** function. It’s an observable. It is not the source observable. It is created specifically for `retryWhen`. It has one use and one use only: whenever subscription (or re-subscription) to the source observable results in an error, `errors` fires a `next`. We are given `errors` and are free to use it in order to react in some way to each failed subscription attempt to the source observable.

And also few words on the function inside **retryWhen()**. Remember, this function we are currently inside of, it must return an observable, using `attempts` as input. This resulting observable is only built once. `retryWhen` then subscribes to that resulting observable and: retries subscribing to source observable whenever resulting observable fires `next`; calls `complete` or `error` on source observable whenever resulting observable fires those corresponding events.

**Another very slightly different use-case of retryWhen** — while making sure that the last error is the one that gets thrown. The answer is a bit less clean but no less powerful, just use one of the flattening map operators (concatMap, mergeMap, switchMap) to check which index you are at.

![](https://cdn-images-1.medium.com/max/800/1*1Xr1VHeP7BndP7iDXbdUkw.png)
undefined

### switchMap() operators

`switchMap()` does exactly what it says, it switches from the primary observable to a secondary (or 'inner') observable.

[switchMap](https://www.learnrxjs.io/operators/transformation/switchmap.html) is often recommended with the subscription because of its advantages compared to other flattening operators. **It will switch to a new observable and cancel the previous observable**.

`switchMap` is usually used when you have some async operation that is triggered by some prepended "event/stream". The difference to e.g. `flatMap` or `concatMap` is, that as soon as the next trigger emits, the current async operation is canceled and re-triggered.

One of the most common use case of switchMap() is to pull a url parma (the employee ID in the below code example ), and retrieve the employee to display.

![](https://cdn-images-1.medium.com/max/800/1*veDHZd8ql9U9NX9DrncINw.png)
undefined

So in above, as the route-params change, my **service.getEmployee()** is automatically called again with the changed params and the previous call is canceled so you won’t receive outdated data.

So in above, if I subscribe to the paramMap and I start spamming changes to the route parameters, switchMap will cancel any pending requests and pick up the new request. If you make an HTTP request within the subscription then use switchMap to cancel any unnecessary pending request.

#### But now lets see a more interesting and more directly use-experience enhancing effect of using switchMap()

Here we will explore the very common case when a user types some text in an input box and based on that text an api call is being executed to the backend to fetch the relevant data. And we certainly want to make this as smooth and fast experience for the user as possible.

First note that, the benefits of using a switchMap() operator will becomes, clearer when we execute a time consuming function. Lets define one,

![](https://cdn-images-1.medium.com/max/800/1*0JhWDoqYmtP2sUo5qDtLMA.png)
undefined

In this code, we have a function that takes 2 seconds to execute, and let’s see the effect if we keep using the flatMap() operator in executing this job:

![](https://cdn-images-1.medium.com/max/800/1*4ATPXwtKLN33PyaktcApEw.png)
undefined

With above code, every time user hits a key, it generates an event. However, we have a .filter() operator in place that ensures an event is only generated when at least four keys are entered,

```
filter((key) => elem.value.length > 3)
```

Now, if a user enters keys in an input control, they expect a request to be made when they are done typing. A user defines being done as entering a few characters and also that they should be able to move characters if they were mistyped. So assume the following input sequences — that almost all of us do regularly:

```
// enters abcdeabcde// removes 'e'
```

At this point, they have entered characters and then reasonably quickly, edited their answer. The user expects to receive an answer based on `abcd` Using the `flatMap()` operator here, however, means the user will get two answers back because, in reality, they typed `abcde` and `abcd`.

Imagine we get a results list based on these two inputs — it would most likely be two lists that looked somewhat different.

First the full response based on `abcde`

And then the full response based on `abcd`

Our code and app most likely would be able to handle the situation described by re-rendering the results list as soon as a new response arrives. There are two problems with this though:

firstly, we do an unnecessary network request for abcde, and secondly, if the backend is fast enough in responding, we will see a flickering in the UI as the result list is rendered once and then, shortly after, is rendered again, based on the second response. This is not good, and we want to have a situation where the first request will be abandoned if we keep on typing. This is where the switchMap() operator comes in. It does exactly that.

Let’s therefore alter the preceding code to the following:

![](https://cdn-images-1.medium.com/max/800/1*dCoiTaMYtW47llc1Qa4QHQ.png)
undefined

In this code, we simply switched our `flatMap()` to a `switchMap()`. When we now execute the code in the exact same way, that is, the user firstly typing `abcde` and shortly altering that to `abcd`, the end result is:

the full response based on `abcd`

Because with `switchMap` we get one request only. The reason for this is that the previous event is aborted when a new event happens — `switchMap()` is doing its magic. This is especially helpful for search-queries that might take longer then 200–300ms

To compare switchMap with its other rx-js counterparts.

*   **flatMap/mergeMap** — creates an Observable immediately for any source item, all previous Observables are kept alive
*   **concatMap** — waits for the previous Observable to complete before creating the next one
*   **switchMap** — for any source item, completes the previous Observable and immediately creates the next one
*   **exhaustMap** — source items are ignored while the previous Observable is not completed

By [Rohan Paul](https://medium.com/@paulrohan) on [July 21, 2020](https://medium.com/p/783997f4f719).

[Canonical link](https://medium.com/@paulrohan/angular-rxjs-operators-part-1-debouncetime-retrywhen-and-switchmap-783997f4f719)

Exported from [Medium](https://medium.com) on December 12, 2020.