---
layout: post
title: Call(), Apply() and Bind() method - The three workhorses of Javascript land
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/Apply-Method.jpg" class="fit image">


Further to my previous [post](https://rohan-paul.github.io/javascript/2016/10/21/Understanding_JavaScript_Call_Method/) on this topic, wanted to revisit this again, given the great usefulness that they have and should be a must for your Javascript repertoire of tools.

To understand them, the first thing we should know is what ``this`` stands for in Javascript function. Basically, ``this`` keyword in Javascript always refers to the 'owner' / 'receiver' of the function that is being executed or called. If no explicit owner / receiver is defined, then the top most owner, the window object, is referenced.

For non-method functions, ``this`` refers different things in different contexts.

<script src="https://gist.github.com/rohan-paul/0ddfcf3b3145ac1daed9f81025bbc80c.js"></script>

So, ``this`` in a normal function refers the ``global`` object. If it's in strict mode, ``this`` will be undefined. And if the function is used with the new operator, a new empty object will be assigned to ``this``.


**Call()**

Each function calls gets its own ``this`` binding. But ``call()`` gives us a way to "borrow" a method from one object to use for another by specifyng what that ``this`` binding would be. ``.call()`` attaches ``this`` into function and executes the function immediately.

<script src="https://gist.github.com/rohan-paul/6d894a29ed0957aa1d0786f9013501c9.js"></script>

In the [ES5 spec](https://es5.github.io/#x15.3.4.4), the ``.call()`` method is described in terms of more low level primitives.


**Apply()**

The limitations of ``call()`` quickly become apparent when you want to write code that doesn't (or shouldn't) know the number of arguments that the functions need.

So that's where apply comes in - the second argument needs to be an array, which is unpacked into arguments that are passed to the called function. e.g. finding the maximum value from an array.

We can not directly use, Math.min or Math.max as we will get NaN.

```
const nums = [1, 2, 3]
console.log(Math.min(nums));    // NaN
console.log(Math.max(nums));    // NaN 
```
That is because Math.min or Math.max expect distinct variables and not an array. Hence we use ``.apply()``


```
var getMaxNumberFromArray = function (arr) {
    return Math.max.apply(null, arr);
};

var getMinNumberFromArray = function (arr) {
    return Math.min.apply(null, arr);
};
```

In the above, the passed in array in the function argument may be programmatically constructed arrays of any size.

Now, both ``call()`` and ``apply()`` are methods we can use to assign the ``this`` pointer for the duration of a method invocation. The ``apply()`` method invokes the function ``Math.max()`` and uses its first parameter as the ``this`` pointer inside the body of the function. In other words - we've told the runtime what object to reference as ``this`` while executing inside of function ``Math.max``, And when ``null`` or ``undefined`` is supplied as the receiver to ``call()`` or ``apply()``, the global object is used as the value for receiver instead, as the value of ``this`` can never be ``null`` or ``undefined`` when a function is called.

**So, why I am passing ``null`` as the first argument to ``.apply()** - first see whats the official mozilla doc says 

``
The apply() method calls a function with a given this value and arguments provided as an array (or an array-like object).
fun.apply(thisArg, [argsArray])
thisArg

The value of this provided for the call to fun. Note that this may not be the actual value seen by the method: if the method is a function in non-strict mode code, null and undefined will be replaced with the global object, and primitive values will be boxed.
``

It's usually more of a design decision, by passing ``null`` (or ``undefined``) I am explicitly saying "The function I am calling is pure, it doesn't/shouldn't need to do anything with a context." It also mostly means - "I can call this function over and over again and it will give me the same result back."  And when I pass ``null`` to one of them, I am always doing so because I know the function in question does not care what the ``this`` binding ends up being, since it doesn't use the ``this``binding.
And for most practical purpose, any occurrence of .call(null, ...), .apply(null, ...) or .bind(null, ...) can safely be shortened to .call(0, ...), .apply(0, ...), and .bind(0, ...), respectively.

Using ``null`` with ``.apply()`` or ``.call()`` is only usually done with functions that are methods for namespace reasons only, not for object-oriented reasons. In other words, the function ``max()`` is a method on the Math object only because of namespacing reasons, not because the Math object has instance data that the method ``.max()`` needs to access.


The main difference between ``.call()`` and ``.apply()`` is, while in ``.call()`` the subsequent arguments are passed in to the function as they are, while ``.apply()`` expects the second argument to be an array that it unpacks as arguments for the called function.

**Bind()**

Now that we know that calling a function is actually applying a set of arguments to a function, is it possible to pass just a few of the arguments, not all of them? 
This Partial functions is a functional programming concept. A partial function takes a function and fewer arguments than normal. It returns a function that takes the remaining arguments. When called the returned function call the original function with both sets of arguments.

``bind()`` is particularly powerful in that, as beyond making our code more concise, with it we can make [partial functions](https://en.wikipedia.org/wiki/Partial_function), also called function Cyrrying. It refers to the process of fixing a number of arguments to a function, producing another function of smaller arity.
Or, put differently: The idea behind partial function application is to create a new function by specifying some (but not all) arguments of an existing function. That will return a new function accepting the remaining arguments. Once that new function is called and all parameters are provided, the original function is called with the complete parameter list.

Lets go through an example..

```
var calc = {
 multiply: function (a, b) { return a * b; },
 multiplyResult: function () { return [].reduce.call(arguments, this.multiply) }
};
```
``calc.multiplyResult`` will multiply every arguments passed to it. 

Now I will define a partial function below out of the total function calc.multiplyResult

```
var partFunc = calc.multiplyResult.bind(calc, 1 , 2, 3);  // Returns 6 = ( 1 * 3 * 2 )
```
In the above ``bind`` literally bound the three arguments 1 , 2, 3 as default arguments. And now we will pass some additional incremental arguments, so the partial function calls the original function with both sets of arguments. The partFunc() function 

```
partFunc(4, 5); // Returns 120 = ( 6 * 4 * 5)
```
``bind()`` provides two opportunities (at different times) for passing the function arguments, by preserving the context of ``this`` for future execution.

Lets look at another examples of ``bind()``:

<p data-height="265" data-theme-id="0" data-slug-hash="gxarwR" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="bind_example_macBook.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/gxarwR/">bind_example_macBook.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

In the above, I am creating ``macBookPro()`` as the partial function or Bound function, and pre-specifying the initial arguments, which is the releaseYear (2017) and screen (true). These arguments follow the provided ``this`` value and are then inserted at the start of the arguments passed to the target function (here the target function is ``macBook()``), followed by the arguments passed to the bound function (here that argument is "MacPro-512GB"), whenever the bound function is called.

What .bind() does is [From MDN: Function.prototype.bind())](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind):


``
The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.
``

When I call ``.bind()``, it returns a function, not it's result, so this function can be used in future. So in the above code, when I call it with ``macBook.bind(null, 2017, true)``, it creates and returns a new function, always receiving ``2017`` as first argument and using global context (Because ``null`` is used as context), just like all regular functions use global context, when we call them without ``new`` operator and not using ``.call()`` or ``apply()`` with specific context.

Overall, Use ``.bind()`` when you want that function to later be called with a certain context, useful in events. Use ``.call()`` or ``.apply()`` when you want to invoke the function immediately, and modify the context.