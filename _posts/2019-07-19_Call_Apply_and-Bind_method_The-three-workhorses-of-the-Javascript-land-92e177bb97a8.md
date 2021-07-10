# Call(), Apply() and Bind() method — The three workhorses of the Javascript land

To understand Call(), apply(), bind() methods of javascript, the first thing we should know is what this stands for in Javascript function…

![](https://cdn-images-1.medium.com/max/2560/1*V7YAdZYL64W_Mjd1CgoiOw.jpeg)
undefined

To understand **Call(), apply(), bind() methods of javascript**, the first thing we should know is what `**_this_**` stands for in Javascript function.

Basically, `**this**` keyword in Javascript always refers to the ‘owner’ / ‘receiver’ of the function that is being executed or called. If no explicit owner / receiver is defined, then the top most owner, the window object, is referenced.

For non-method functions, `**this**` refers different things in different contexts.

undefined
undefined

So, `this` in a normal function refers the `global` object. If it’s in strict mode, `this` will be undefined. And if the function is used with the new operator, a new empty object will be assigned to `this`.

#### **Call()**

**Each function calls gets its own** `**this**` **binding. But** `**call()**` **gives us a way to “borrow” a method from one object to use for another by specifyng what that** `**this**` **binding would be.** `**.call()**` **attaches** `**this**` **into function and executes thefunction immediately.**

undefined
undefined

One of the features of JavaScript is that functions are first-class objects here. This means that functions can be assigned to a variable and passed around. And if functions are objects and objects can have methods, then functions can have methods. Thus `call` is such a method of functions and we already saw above how its used to borrow methods from other Objects and set `this` value explicitly.

`call` is used when I want to control the scope that will be used in the function called. Each function call gets its own `this` binding, and I might want the `this` keyword to be something else than the scope I assigned the function to, in those cases I need to use `call`or `apply` to bring the right scope into the function. It also allows me to call utility methods outside the scope, but still bring the local scope into the method

Lets check out the below example.

undefined
undefined

That is, effectively `call` allowed me to borrow the function `updatePageView` from another object and set the `this`value in function invocation.

If I wanted to use `apply` method instead of `call` in the above example, the argument passed need to be a single array of values. So the code will need to modified like below..

`Website.updatePageView.apply(profilePage, [5, 6, 7, 8]);`

The numbers 5, 6, 7, 8 may come from any number of sources like an API or whatever.

Now, the semantics of `this` in function invocations is quite confusing for many including me. So, lets look at a very simple example.

undefined
undefined

I invoked `myFunction()` with `this` set to `"Paul"` and a single argument `"world"`. The first parameter given to `call` has a special purpose, which is to set `myFunction()`’s internal `this` value. But any subsequent parameters are treated the same as if `myFunction` had been invoked normally.

When a function is invoked ordinarily (i.e. without using `call`), the `this` value is set implicitly.

If I am not in strict mode and the function isn’t attached to an object, then it will inherit its this from the global object. If the function is attached to an object, its default `this` is the receiver of the method call.

So, in the preceding code, if I just run `myFunction("World");`, the output will be `"[object global] says hello world"`.

It’s important to remember that using `.call()` or `.apply()` actually invokes my function, so instead of doing this:

`myFunction();` // invoke myFunction

I wil have to let `.call()` handle it and chain the method:

`myFunction.call(scope);` // invoke myFunction using `.call()`

**Common usecase of** `**call**` **as a Function-borrowing tool**

The most common use of `call` and `apply` is that they enable re-use of built-in functions, i.e. invoking functions in a different context and is a great way to reuse existing functionality without having to make one object extend or inherit from another.

Consider this code below, where I am re-using or borrowing built-in `Array.prototype.slice()` one the most often borrowed function.

undefined
undefined

In the above, I only wanted to sort the arguments of function `myFunc()` from the `arguments` object that is a property of all JavaScript functions and is an array-like object. So to do that sorting, the eaziest way would have been with a line of code `arguments.sort();`. But `arguments` is an array like object, not a real array. So using `arguments.sort()` would throw `"TypeError: arguments.sort is not a function"`

Consider another example of borrowing built-in functions with `call`

`var arrayLikeObj = {0:"Apple", 1:"Uber", 2:"50", length:3};`

`var convertedRealArray = Array.prototype.slice.call(arrayLikeObj, 0);`

`console.log(convertedRealArray);`

Will output => \[ ‘Apple’, ‘Uber’, ‘50’ \]

**Using** `**.call()**` **to demehodize native JS methods**

We demethodize the `split()` method into a generic function call `demethSplit` using `Function.prototype.call.apply`

![](https://cdn-images-1.medium.com/max/800/1*SuLHVMPRSL1eGMI1k93aDw.png)
undefined

Now note the signature syntax of `.call()` and `.apply()`

`function.call(thisArg, arg1, arg2, ...)`

`func.apply(thisArg, [argsArray])`

So firstly, we are calling `Function.prototype.call` and since `.call()` method calls a function with a given `this` value and arguments provided individually - that means that whatever we pass into the second `.apply()`, is going to become the `thisArg`in the first `.call()`.

Another, slighlty longer way to write the above `demehodize()`function would be …

```
function demethodize(fn) {
```

```
  return function (that, y) {
```

```
  return fn.call(that, y);
```

```
 }}
```

**In the above we are basically writing a function, that takes an argument (which itself is a function), and returns a function, that in turn calls that original argument with** `**Function.prototype.call**` **and passing a spcific** `**this**` **value to that called function.**

The main idea of this kind of trick is to make my code cleaner. The new function `demethSplit` is very much like the old one, except the arguments are “shifted” to the right by one, and the first argument is now what the old function used to expect as the `this` context variable.

**apply()**

The limitations of `call()` quickly become apparent when you want to write code that doesn’t (or shouldn’t) know the number of arguments that the functions need.

So that’s where apply comes in — the second argument needs to be an array, which is unpacked into arguments that are passed to the called function. e.g. finding the maximum value from an array.

We can not directly use, Math.min or Math.max as we will get NaN.

```
const nums = [1, 2, 3]
```

```
console.log(Math.min(nums)); // NaN
```

```
console.log(Math.max(nums)); // NaN
```

That is because Math.min or Math.max expect distinct variables and not an array. Hence we use `.apply()`

```
var getMaxNumberFromArray = function (arr) {
```

```
return Math.max.apply(null, arr);
```

```
};
```

```
var getMinNumberFromArray = function (arr) {
```

```
return Math.min.apply(null, arr);
```

```
};
```

In the above, the passed in array in the function argument may be programmatically constructed arrays of any size.

So what’s the difference? `apply` works in almost exactly the same way as call. The difference is that instead of a series of arguments, apply takes an array of values to use in its invocation. Another thing that `apply` can do which `call` can’t is executing Variable-Arity Functions or variadic functions. These are functions that accept any number of arguments instead of a fixed number of arguments. The arity of a function specifies the number of arguments the function is supposed to accept. A common example of variable-arity function in JavaScript is Math.max() method.

Now, both `call()` and `apply()` are methods we can use to assign the `this` pointer for the duration of a method invocation. The `apply()` method invokes the function `Math.max()` and uses its first parameter as the `this` pointer inside the body of the function. In other words - we’ve told the runtime what object to reference as `this` while executing inside of function `Math.max`, And when `null` or `undefined` is supplied as the receiver to `call()` or `apply()`, the global object is used as the value for receiver instead, as the value of `this` can never be `null` or `undefined` when a function is called.

**So, why I am passing** `**null**` **as the first argument to \`\`.apply()** - first see whats the official mozilla doc says

The apply() method calls a function with a given this value and arguments provided as an array (or an array-like object).

fun.apply(thisArg, \[argsArray\])

thisArg

The value of this provided for the call to fun. Note that this may not be the actual value seen by the method: if the method is a function in non-strict mode code, null and undefined will be replaced with the global object, and primitive values will be boxed.

It’s usually more of a design decision, by passing `null` (or `undefined`) I am explicitly saying “The function I am calling is pure, it doesn’t/shouldn’t need to do anything with a context.” It also mostly means - “I can call this function over and over again and it will give me the same result back.” And when I pass `null` to one of them, I am always doing so because I know the function in question does not care what the `this` binding ends up being, since it doesn’t use the `this`binding.

And for most practical purpose, any occurrence of `.call(null, …),` `.apply(null, …)` or `.bind(null, …)` can safely be shortened to `.call(0, …)`, `.apply(0, …)`, and `.bind(0, …)`, respectively. Although a word of caution, its not purely technically correct, as a `Number` object is not the same as `null` (in strict mode) and the global object (non-strict mode).

Using `null` with `.apply()` or `.call()` is only usually done with functions that are methods for namespace reasons only, not for object-oriented reasons. In other words, the function `max()` is a method on the Math object only because of namespacing reasons, not because the Math object has instance data that the method `.max()` needs to access.

The main difference between `.call()` and `.apply()` is, while in `.call()` the subsequent arguments are passed in to the function as they are, while `.apply()` expects the second argument to be an array that it unpacks as arguments for the called function.

**In conclusion**, `call` and `apply` promote code reuse by allowing runtime function invocation in the context of a sepcific instance - regardless of the hierarchical lineage of the function. While inheritance is great, in most situations simply borrowing a method is a more leaner solution and will let me cherry-pick specific functionality without the baggage of a hierarchy.

**bind()**

Now that we know that calling a function is actually applying a set of arguments to a function, is it possible to pass just a few of the arguments, not all of them?

This Partial functions is a functional programming concept. A partial function takes a function and fewer arguments than normal. It returns a function that takes the remaining arguments. When called the returned function call the original function with both sets of arguments.

`bind()` is particularly powerful in that, as beyond making our code more concise, with it we can make [partial functions](https://en.wikipedia.org/wiki/Partial_function), also called function Cyrrying. It refers to the process of fixing a number of arguments to a function, producing another function of smaller arity.

Or, put differently: The idea behind partial function application is to create a new function by specifying some (but not all) arguments of an existing function. That will return a new function accepting the remaining arguments. Once that new function is called and all parameters are provided, the original function is called with the complete parameter list.

Lets go through an example…

```
var calc = {
```

```
multiply: function (a, b) { return a * b; },
```

```
multiplyResult: function () { return [].reduce.call(arguments, this.multiply) }
```

```
};
```

`calc.multiplyResult` will multiply every arguments passed to it.

Now I will define a partial function below out of the total function calc.multiplyResult

```
var partFunc = calc.multiplyResult.bind(calc, 1 , 2, 3); // Returns 6 = ( 1 * 3 * 2 )
```

In the above `bind` literally bound the three arguments 1 , 2, 3 as default arguments. And now we will pass some additional incremental arguments, so the partial function calls the original function with both sets of arguments. The partFunc() function

```
partFunc(4, 5); // Returns 120 = ( 6 * 4 * 5)
```

`bind()` provides two opportunities (at different times) for passing the function arguments, by preserving the context of `this` for future execution.

Lets look at another examples of `bind()`:

![](https://cdn-images-1.medium.com/max/800/1*meA0pktEQHLywfSvtwaJqA.png)
undefined

In the above, I am creating `**macBookPro()**` as the partial function or Bound function, and pre-specifying the initial arguments, which is the releaseYear (2017) and screen (true). These arguments follow the provided `this` value and are then inserted at the start of the arguments passed to the target function (here the target function is `macBook()`), followed by the arguments passed to the bound function (here that argument is “MacPro-512GB”), whenever the bound function is called.

What **_.bind()_** does is [_From MDN: Function.prototype.bind())_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)_:_

```
The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.
```

When I call `**.bind()**`, it returns a function, not it’s result, so this function can be used in future. So in the above code, when I call it with `macBook.bind(null, 2017, true)`, it creates and returns a new function, always receiving `2017` as first argument and using global context (Because `null` is used as context), just like all regular functions use global context, when we call them without `new` operator and not using `.call()` or `apply()` with specific context.

Overall, Use `**.bind()**` when you want that function to later be called with a certain context, useful in events. Use `**.call()**` **or** `**.apply()**` when you want to invoke the function immediately, and modify the context.

By [Rohan Paul](https://medium.com/@paulrohan) on [July 19, 2019](https://medium.com/p/92e177bb97a8).

[Canonical link](https://medium.com/@paulrohan/call-apply-and-bind-method-the-three-workhorses-of-the-javascript-land-92e177bb97a8)

Exported from [Medium](https://medium.com) on December 12, 2020.