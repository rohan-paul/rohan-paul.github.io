---
layout: post
title: Understanding JavaScript Call Method
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/call-function.jpg" class="fit image">

One of the features of JavaScript is that functions are first-class objects here. This means that functions can be assigned to a variable and passed around. And if functions are objects and objects can have methods, then functions can have methods. The ``call`` is such a method of functions. Its used to borrow methods from other Objects and set ``this`` value explicitly.

`call` is used when I want to control the scope that will be used in the function called.  Each function call gets its own ``this`` binding, and I might want the ``this`` keyword to be something else than the scope I assigned the function to, in those cases I need to use `call` or `apply` to bring the right scope into the function.

It also allows me to call utility methods outside the scope, but still bring the local scope into the method

An almost equivalent method to `call` is `apply`. So what's the difference? `apply` works in almost exactly the same way as call. The difference is that instead of a series of arguments, apply takes an array of values to use in its invocation. Another thing that ``apply`` can do which ``call`` can’t is executing Variable-Arity Functions or variadic functions. These are functions that accept any number of arguments instead of a fixed number of arguments. The arity of a function specifies the number of arguments the function is supposed to accept. A common example of variable-arity function in JavaScript is Math.max() method.

Lets check out the below example.

<script src="https://gist.github.com/rohan-paul/8c6087c65845b8b3ba25fd5ea8a8e70d.js"></script>

Here, I had this master object `Website` and I want to inherit the function `updatePageView` in another object `profilePage`, although `profilePage` does not have the `updatePageView` function.  So, the solution is  - ``Website.updatePageView.call(profilePage)``
That is, effectively ``call`` allowed me to borrow the function ``updatePageView`` from another object and set the ``this`` value in function invocation. 


To note, the first argument to ``call()`` sets the ``this`` value. In the preceding example, it is set to the `profilePage` object. The other arguments after the first argument are passed as parameters to the ``updatePageView()`` function.

If I wanted to use ``apply`` method instead of ``call`` in the above example, the argument passed need to be a single array of values. So the code will need to modified like below..

``Website.updatePageView.apply(profilePage, [5, 6, 7, 8]);``

The numbers 5, 6, 7, 8 may come from any number of sources like an API or whatever.

Now, a lot of people, including me, have felt that the semantics of ``this`` in function invocations is confusing. So, lets look at a very simple example.

<script src="https://gist.github.com/rohan-paul/cb573154036c1a330f55929389bf8a31.js"></script>

We invoked ``myFunction()`` with ``this`` set to ``"Paul"`` and a single argument ``"world"``. The first parameter given to ``call`` has a special purpose, which is to set ``myFunction()``'s internal ``this`` value. But any subsequent parameters are treated the same as if ``myFunction`` had been invoked normally. 

When a function is invoked ordinarily (i.e. without using ``call``), the ``this`` value is set implicitly.


If I am not in strict mode and the function isn't attached to an object, then it will inherit its this from the global object. If the function is attached to an object, its default ``this`` is the receiver of the method call. 

So, in the preceding code, if I just run ``myFunction("World");``, the output will be ``"[object global] says hello world"``.


It’s important to remember that using ``.call()`` or ``.apply()`` actually invokes my function, so instead of doing this:

``myFunction();`` // invoke myFunction

I wil have to let ``.call()`` handle it and chain the method:

``myFunction.call(scope);`` // invoke myFunction using ``.call()``
       

**Common usecase of ``call`` as a Function-borrowing tool**


The most common use of ``call`` and ``apply`` is that they enable re-use of built-in functions, i.e. invoking functions in a different context and is a great way to reuse existing functionality without having to make one object extend or inherit from another. 

Consider this code below, where I am re-using or borrowing built-in ``Array.prototype.slice()`` one the most often borrowed function.

<script src="https://gist.github.com/rohan-paul/4d7ebc874938659831d1196d312eb46e.js"></script>

In the above, I only wanted to sort the arguments of function ``myFunc()`` from the ``arguments`` object that is a property of all JavaScript functions and is an array-like object. So to do that sorting, the eaziest way would have been with a line of code ``arguments.sort();``. But ``arguments`` is an array like object, not a real array. So using ``arguments.sort()`` would throw ``"TypeError: arguments.sort is not a function"``

Consider another example of borrowing built-in functions with ``call``


``var arrayLikeObj = {0:"Apple", 1:"Uber", 2:"50", length:3};``

``var convertedRealArray = Array.prototype.slice.call(arrayLikeObj, 0);``

``console.log(convertedRealArray);``

Will output => [ 'Apple', 'Uber', '50' ]

      

**In conclusion**, ``call`` and ``apply`` promote code reuse by allowing runtime function invocation in the context of a sepcific instance - regardless of the hierarchical lineage of the function. While inheritance is great, in most situations simply borrowing a method is a more leaner solution and will let me cherry-pick specific functionality without the baggage of a hierarchy.