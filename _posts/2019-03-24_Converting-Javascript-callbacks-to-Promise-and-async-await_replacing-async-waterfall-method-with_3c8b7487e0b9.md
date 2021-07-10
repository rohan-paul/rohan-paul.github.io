# Converting Javascript callbacks to Promise and async-await & replacing async-waterfall method with…

On regular day of a developer, we have to convert quite a bit of legacy code that uses traditional callbacks to Promises and async-await…

On regular day of a developer, we have to convert quite a bit of legacy code that uses traditional callbacks to Promises and async-await…

![](https://cdn-images-1.medium.com/max/600/1*dF7X5im-3qVRpAcaU_eYCg.jpeg)
undefined

On regular day of a developer, we have to convert quite a bit of legacy code that uses traditional callbacks to Promises and async-await, both for taking advantage of the latest features of ES6 and also to avoid callback-hell and making the code more beautiful and compact and human readable.

So, here I shall go over some of those basic converstions and then finally replacing completely the [**async**](https://www.npmjs.com/package/async) package’s [**async.waterfall**](https://caolan.github.io/async/docs.html#waterfall) method to Promises and async-await.

#### First understanding some of the basic flow of a Promise

A promise is an object that wraps an asynchronous operation and notifies when it’s done. The important differences with a regular callbacks are in the usage of Promises. **Instead of providing a callback, a promise has its own methods (.then)** which you call to tell the promise what will happen when it is successful or when it fails. The methods a promise provides are **“then(…)”** for when a successful result is available and **“catch(…)”** for when something went wrong.

More simply — The Promise constructor function takes in a single argument, a (callback) function called **executor argument**. This function in turn is passed two arguments, **resolve()** and **reject()**.

**resolve** — a function that allows you to change the status of the promise to fulfilled

**reject** — a function that allows you to change the status of the promise to rejected.

Beautifully explained in [**_ECMAScript 2019 Language Specification Draft ECMA-262 / June 27, 2018,_**](https://tc39.github.io/ecma262/#sec-promise-constructor) **_“The executor argument_** _must be a_ [_function object_](https://tc39.github.io/ecma262/#function-object)_. It is called for initiating and reporting completion of the possibly deferred action represented by this Promise object. The executor is called with two arguments: resolve and reject. These are functions that may be used by the executor function to report eventual completion or failure of the deferred computation. Returning from the executor function does not mean that the deferred action has been completed but only that the request to eventually perform the deferred action has been accepted.”_

#### How do I know or listen for and access when the status of a promise changes?

This is the most important question to know how to do anything after the status changes.

What a promise actually is. When you create a new Promise, you’re really just creating a plain old JavaScript object. **This object can invoke two methods, then, and catch. Here’s the key. When the status of the promise changes to fulfilled, the function that was passed to .then() will get invoked. When the status of a promise changes to rejected, the function that was passed to .catch()will be invoked.**

And both the `[Promise.prototype.then()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)` and `[Promise.prototype.catch()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)` methods return promises, so they can be chained

**What this means is that once you create a promise, you’ll pass the function you want to run if the async request is successful to .then(). And you’ll pass the function you want to run if the async request fails to .catch().**

In other words, **When we call a Promise function, the result from the successful path will show up in the .then(), while the error scenario will show up in the .catch()**

![](https://cdn-images-1.medium.com/max/800/1*mbpNNeUYiQjU_N5nR8nORw.png)
undefined

Running the code above will print “After success moving to .then” in the console. Whats happening is, first, when I created the promise, I invoked `resolve` after ~2000 milliseconds - this changed the status of the promise to `fulfilled`. Second, I passed the `onSuccess` function to the promises’ `.then` method. By doing that we told the promise to invoke `onSuccess` when the status of the promise changed to `fulfilled` which it did after ~2000 milliseconds.

Now check the reject method

![](https://cdn-images-1.medium.com/max/800/1*jWN6F5K8mqkMhjRtRdE5Uw.png)
undefined

This time instead of the `onSuccess` function being invoked, the `onError` function will be invoked since we called `reject`.

Promises will handle any exceptions (both explicit and implicit) in asynchronous code blocks that use `then()`. Just add `.catch()` to the end of promise chains. **Because of how the chaining of promises and errors work, just one catch at the end of the chain is guaranteed to catch any errors thrown along the way. This makes error handling really straight forward.**

Because \`.then()\` always returns a new promise, it’s possible to chain promises with precise control over how and where errors are handled. Promises allow you to mimic normal synchronous code’s try/catch behavior.

Like synchronous code, chaining will result in a sequence that runs in serial. In other words, you can do:

![](https://cdn-images-1.medium.com/max/800/1*WEHZem3M3apFX9Q2OM60EA.png)
undefined

Assuming each of the functions, fetch(), process(), and save() return promises, process() will wait for fetch() to complete before starting, and save() will wait for process() to complete before starting. handleErrors() will only run if any of the previous promises reject.

#### Another example to fetch URL results with Promise.

Here, I will write a function in that I can pass a URL to a promise of the results. Here what I am trying to do is to chain a bunch of URL’s by taking the result of an API, one URL call that then contains the URL of the NextPage of results.

I shall be using the package ‘request’, and unlike the other popular network fetching packages like **axios** or the native **Fetch API**, ‘request’ does not return a Promise natively, and thats why I am using it here, so I can actually show how to return a Promise after a call with request. Per [request’s documentation](https://www.npmjs.com/package/request#promises--asyncawait) — _request supports both streaming and callback interfaces natively. If you’d like request to return a Promise instead, you can use an alternative interface wrapper for request.”_

So here’s how, I can make the plain **request package** to return a Promise.

![](https://cdn-images-1.medium.com/max/800/1*yTZBMt09Cd3TmTjWwANemQ.png)
undefined

#### Now, coming to coverstion from callbacks to Promise, lets convert the simplest of Callbacks to Promise. The good-old setTimeout()

Take this simple function that uses a callback to print the full name.

![](https://cdn-images-1.medium.com/max/800/1*V658OZeQadb4SF-Z6Nt88g.png)
undefined

And now converting it to a Promise, it will be like below. And note, the console.log() is a function in JavaScript, which is what I have used above as a callback.

![](https://cdn-images-1.medium.com/max/800/1*6STX-1UbAF3RvtOkxWrF3w.png)
undefined

So, here’s what I did for the conversion, First, we **remove the callback argument**. Then we **add the code to return a new Promise** from our Promise-based function. **The error callback becomes a reject, while the “success path” callback becomes a resolve.** The `**executor**`function (the **_setTimeout()_** here) is executed immediately by the Promise implementation, passing `resolve` and `reject` functions (the executor is called before the `Promise` constructor even returns the created object).

Notice that the parameters of **printFullNameFn** (the one that uses a callback instead of a Promes)have changed. Instead of receiving firstName and callback, it just receives `firstName`. There’s no more need for that callback function because now instead, we use the Promise’s `resolve` and `reject` functions. `resolve` will be invoked if the execution was successful, `reject` will be invoked if there was an error.

Note that, Promise returning functions should never throw, they should return rejections instead. Throwing from a promise returning function will force you to use both a } catch { and a .catch. And people using promisified APIs do not expect promises to throw.

Remember, Promise expects a single function as an argument. That function takes 2 arguments (both are callbacks), a resolve function and a reject function.

**When we call the printFullNameFnPromise(), the result from the success path will show up in the .then(), while the error scenario will show up in the .catch()**

The great advantage about having our original function in a Promise form is that we don’t actually need to “make it an async/await version” if we don’t want to. When we call/execute the function, we can simply use the async/await keyword as below

![](https://cdn-images-1.medium.com/max/800/1*wFzYuxxE65Hvw_UBTiQQyg.png)
undefined

This is because, remember one of the key requirement of using async-await is — **Await works only with Promises, it does not work with callbacks. So to implement async-await to a function/code, I first have to convert the original function from a callback form to a Promise form.**

#### Replacing completely the [**async**](https://www.npmjs.com/package/async) package’s [**async.waterfall**](https://caolan.github.io/async/docs.html#waterfall) method to Promises and async-await.

In this node, react, mongo project mine ([**check the full source code in github**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/tree/sending-otp-with-mailgun)**, branch is sending-otp-with-mailgun**), I am sending an OTP to the user before he can view a document. I am using mailgun-js for generating a template email and sending the email to the user. I also posted a blog on the steps of building this project ([**check it here**](https://medium.com/@paulrohan/sending-verification-otp-to-user-email-with-mailgun-in-a-react-node-and-mongo-app-56ba7e4ac29) ).

Here’s my [**legacy code using async’s async.waterfall**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/routes/mailSender--with-async-waterfall-method.js#L17) method.

![](https://cdn-images-1.medium.com/max/800/1*EUQLaxDeZpG7zEV71JFhOQ.png)
undefined

In the above code, the first function **generateTemplate()** will return the ‘mail’ variable. And that will be passed to the second function **sendMail(mail, callback)** which will further work with that.

The [**async**](https://www.npmjs.com/package/async) package’s [**async.waterfall**](https://caolan.github.io/async/docs.html#waterfall) method used above, will _will take both anonymous function or named function in its chain of function execution — In above, sendMail() is my named function that I am declaring within the async_

_According to the repo’s documentation, this is what the waterfall does —_ **_“Runs an array of functions in series, each passing their results to the next in the array. However, if any of the functions pass an error to the callback, the next function is not executed and the main callback is immediately called with the error.”_**

_And about the callback passed to ‘generateTemplate()’ function — The documentataion says — This is an optional callback to run once all the functions have completed. This will be passed the results of the last task’s callback. Invoked with (err, \[results\])._

**To understand the async.waterfall better lets also understand how its different from** [**async.series**](https://caolan.github.io/async/docs.html#series) — The documentation of async.series says, _“Run the functions in the_ `_tasks_` _collection in series, each one running once the previous function has completed. If any functions in the series pass an error to its callback, no more functions are run, and_ `_callback_` _is immediately called with the value of the error. Otherwise,_ `_callback_` _receives an array of results when_ `_tasks_` _have completed.”_

async.waterfall allows each function to pass its results on to the next function, while async.series passes all results to the final callback. At a higher level, async.waterfall would be for a data pipeline (“given 2, multiply it by 3, add 2, and divide by 17”), while async.series would be for discrete tasks that must be performed in order, but are otherwise separate.

Both functions pass the return value, of every function to the next, then when done will call the main callback, passing its error, if an error happens.

**The difference is that in async.series(), once the series have finished, will pass all the results to the main callback. async.waterfall() will pass to the main callback ONLY the result of the LAST function called**.

And after defining the [**sendOTPToVisitor()**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/routes/fileUploadRoutes-with-async-waterfall.js#L191) **function in the above file,** I was [calling/invoking that **sendOTPToVisitor()** in the relevant route file](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/routes/fileUploadRoutes-with-async-waterfall.js#L191) like below

![](https://cdn-images-1.medium.com/max/800/1*hr4cDN3O1ax2nWfGGF4h-Q.png)
undefined

And now, I am replacing async.waterfall by converting the callback based usage to one where I am returning a Promise as below. So the new [**sendOTPToVisitor()** function definition](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/routes/mailSender.js#L17) will like below.

![](https://cdn-images-1.medium.com/max/800/1*xIHxNykFhTaGjkX9T1LsTA.png)
undefined

And the [**new route file where I am calling that** **sendOTPToVisitor()** function](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/routes/fileUploadRoutes.js#L214) will like below. Here, I am using async-await to get the resolved result from a Promise.

![](https://cdn-images-1.medium.com/max/800/1*CBLJygbbck5QpEdrV1UiaQ.png)
undefined

By async-await’s standard working mechanism, when the word **_‘await’_** is placed in front of a Promise call, **_await_** forces the rest of the code to wait until that Promise finishes and returns a result. This will pause the async function and wait for the Promise to resolve prior to moving on. **The effect, the code _pauses_ _execution_ on those lines until the Promises resolve! Asynchronous programming becomes _synchronous._**

In the above implementaion of the sendOTPToVisitor() function, I could as well have used .then syntax. Basically, By wrapping the logic inside an `async` _function_, we can replace the `then`_callbacks_ with `**await**` **_statements_**.

**Async/Await** enables us to write asynchronous code in a synchronous fashion, which produces cleaner and easier-to-understand logic. Under the hood, it’s just syntactic sugar [**using g_enerators_ and _yield statements_ to “pause” execution**](https://tc39.github.io/ecmascript-asyncawait/#async-function-definitions). In other words, **async functions** can “pull out” the _value_ of a _Promise_ even though it’s nested inside a _callback function,_ giving us the ability to assign it to a _variable._

Now why would we need the _async package anymore given that we have ES6-Promise — The ans is async.js is a vast suite of tools (with 22mn+ npm package download per week) and provides a great many useful methods (especially for handling concurrency) than the builtin promise methods. But sure, you won’t need anything like waterfall or series when working with promises_

By [Rohan Paul](https://medium.com/@paulrohan) on [March 24, 2019](https://medium.com/p/3c8b7487e0b9).

[Canonical link](https://medium.com/@paulrohan/converting-javascript-callbacks-to-promise-and-async-await-replacing-async-waterfall-method-with-3c8b7487e0b9)

Exported from [Medium](https://medium.com) on December 12, 2020.