# Tackling common JavaScript Interview Questions

Part 1: A series for the most common JavaScript Interview questions taken from real interviews

![](https://cdn-images-1.medium.com/max/1200/1*oyY0ly8aZXgVrrute7Z1bw.jpeg)
undefined

#### “this” keyword in JavaScript

In my experience, this is THE-MOST common question in any sort of JavaScript interviews and I think its very reasonably so as the proper usage of **‘this’** in javascript is really one of the most crucial topic to master and hugely useful in daily work in any framework including React or Angular. Hence as a JS-Pro we absolutely need to master this concept.

**‘this’** is fundamentally the JavaScript **context object** in which the current code is executing. In the global execution context (outside of any function), this refers to the global object whether in strict mode or not.

Inside a function, the value of this depends on how the function is called.

Case-1 — WITHOUT STRICT MODE:

If a function is not in strict mode, and if the value of this is not set by the call, this will default to the global object, which is window in a browser or global in Node environment

Case-2 — WITH STRICT MODE:

However, the value of this remains at whatever it was set to when entering the execution context, ‘this’ will default to undefined. So, in strict mode, if this was not defined by the execution context, it remains undefined. The global object refers to ‘undefined’ in place of the windows object.

// Case-1 - WITHOUT STRICT MODE - Since the following code is not in strict mode, and because the value of this is not set by the call, this will default to the global object, which is window in a browser, or global in node environment.

function f1() {  
  return this === global  
}

console.log(f1()) // => true

// In a browser:  
f1() === window // true

// In Node:  
f1() === global // true

// Case-2 - In strict mode, however, the value of this remains at whatever it was set to when entering the execution context, so, in the following case, this will default to undefined. So, in strict mode, if this was not defined by the execution context, it remains undefined. And also, f2 was called directly and not as a method or property of an object (e.g. window.f2())

function f2() {  
  "use strict"  
  return this === undefined  
}

console.log(f2()) // true

Now the case when “this” refers to a new instance — here, \`this\` will refer to an object that inherits from \`instance.prototype\` . Meaning with exact JS syntax

Object.getPrototypeOf(instance)

OR

instance.\_\_proto\_\_

(I can print the above 2 expression to get the prototype of the instance)

When a function is invoked with the new keyword, then the function is known as a constructor function and returns a new instance. In such cases, the value of this refers to a newly created instance.

For example:

function Person(first, last) {  
  this.first = first  
  this.last = last

this.displayName = function() {  
    console.log(\`Full name : ${this.first} ${this.last}\`)  
  }  
}

// Note in above, I can not use arrow function as I can not create a constructor function with arrow syntax

let person1 = new Person("John", "Reed")  
let person2 = new Person("Rohan", "Paul")

person1.displayName() // Full name : John Reed  
person2.displayName() // Full name : Rohan Paul

In the case of person1.displayName, this refers to a new instance of person1, and in case of person2.displayName(), this refers to person2 (which is a different instance than Person).

I can check what the the prototype object of person1 with below:

console.log(person1.prototype) // undefined

Object.getPrototypeOf(person1) // Person {}

person1.\_\_proto\_\_ // Person {}

#### **Some more ways to think about ‘this’**

> “this” Refers to an Invoker Object (Parent Object)

> When an object’s method is invoked, then this refers to the object which contains the method being invoked.

> \`this\` (aka “the context”) is a special keyword inside each function and its value only depends on how the function was called, not how/when/where it was defined. It is not affected by lexical scopes like other variables (except for arrow functions, see below). Here are some examples:

function foo() {  
  console.log(this)  
}

// normal function call  
foo() // \`this\` will refer to \`window\` or global in node environment

// as object method  
var obj = { bar: foo }  
obj.bar() // \`this\` will refer to \`obj\`

// as constructor function  
new foo() // \`this\` will refer to an object that inherits from \`foo.prototype\`

Some more super simple examples. Below we’re going to use the method foo as defined in the first example.

function foo() {  
  console.log("Simple function call")  
  console.log(this === global)  
}

let user = {  
  count: 10,  
  foo: foo,  
  foo1: function() {  
    console.log(this === global)  
  },  
}

user.foo()   
// Prints false because now “this” refers to user object instead of global object.

let fun1 = user.foo1  
fun1()   
// Prints true as this method is invoked as a simple function.

user.foo1()   
// Prints false on console as foo1 is invoked as a object’s method, and the 'this' refers to the containing object NOT 'window' or 'global'

But if I do the following instead of user.foo():

foo()

Then it prints ‘true’ — Because now the simple function foo is in the global execution context and so the ‘this’ refers to global

But then agin if my foo() function is in strict mode:

function foo() {  
  "use strict"  
  console.log("Simple function call")  
  console.log(this === global)  
}

Then **foo()** will give me false as the default this in strict mode is ‘undefined’

#### **Now again look at the below part of the code**

fun1()   
// Prints true as this method is invoked as a simple function.

user.foo1()   
// Prints false on console as foo1 is invoked as a object’s method,

The function definition of foo1 is the same, but when it’s being called as a simple function call, then this refers to a global object. And when the same definition is invoked as an object’s method, then this refers to the parent object. So the value of this depends on how a method is being invoked.

#### Implementing “this” to write a custom-Array-Prototype-method

See here when I am adding a new custom Prototype function to Array — **_this_** in the function below refers to the array on which I will invoke this custom function.

// let you have an array like below  
const a = \[1, 2, 3, 4, 5\];

// Implement a function 'multiply()' which will return result like this  
a.multiply();  
console.log(a); // \[1, 2, 3, 4, 5, 1, 4, 9, 16, 25\]

// Solution  
Array.prototype.multiply = function () {  
  let result = \[\]  
  for (let i = 0; i < this.length; i++) {  
    result.push(this\[i\] \*\* 2)  
  }  
  return \[...this, ...result\]  
}

let myArr = \[1, 2, 3, 4, 5\]

console.log(myArr.multiply())

/\*  
\[  
  1, 2, 3,  4,  5,  
  1, 4, 9, 16, 25  
\]

\*/

#### Bind() function — why it is needed

The bind() method creates a new function that, when called, has its \*\*this\*\* keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

The value of `this` is determined by _how_ a function is _called_. If it is _you_ who calls the function then there is usually no need to use `.bind`, since you have control over how to call the function, and therefore its `this` value.

However, often it is _not_ you who calls the function. Functions are passed to other functions as callbacks and event handlers. They are called by _other_ code and you have no control over _how_ the function is called, and therefore cannot control what `this` will refer to.

If your function requires `this` to be set to a specific value and you are not the one calling the function, you need to `.bind` the function to a specific `this` value.

In other words: `.bind` allows you to set the value of `this` without calling the function _now_.

The bind() function creates a new bound function, which is an exotic function object (a term from ECMAScript 2015) that wraps the original function object. Calling the bound function generally results in the execution of its wrapped function.

let boundFunc = func.bind(thisArg\[, arg1\[, arg2\[, ...argN\]\]\])

**“this” argument in bound function**

The value to be passed as the **this** parameter to the target function func when the bound function is called. The value is ignored if the bound function is constructed using the new operator. When using bind to create a function (supplied as a callback) inside a setTimeout, any primitive value passed as thisArg is converted to object. If no arguments are provided to bind, the this of the executing scope is treated as the thisArg for the new function.

global.x = 9

const obj = {  
  x: 70,  
  getX: function() {  
    return this.x  
  },  
}

console.log(obj.getX()) // => 70  
const retrieveX = obj.getX

console.log(retrieveX()) // => 9 Because here the function gets invoked at the global scope

// But now I will change the  
const boundX = retrieveX.bind(obj)  
console.log(boundX()) // => 70

#### Explain the Mechanism with examples of call() and apply() function

> With call(), an object can use a method belonging to another object.

**First some basic rules of this:**

1.  “this” always refers to an object.
2.  “this” refers to an object which calls the function it contains.
3.  In the global context “this” refers to either window object or is undefined if the ‘strict mode’ is used.

[From w3school](https://www.w3schools.com/js/js_function_call.asp)

> In JavaScript all functions are object methods.

> If a function is not a method of a JavaScript object, it is a function of the global object (see previous chapter).

var person = {  
  firstName: "John",  
  lastName: "Doe",  
  fullName: function() {  
    return this.firstName + " " + this.lastName  
  },  
}  
person.fullName()

In a function definition, this refers to the “owner” of the function.

In the example above, this is the person object that “owns” the fullName function.

In other words, this.firstName means the firstName property of this object.

The call() method can be used to invoke (call) a method with an owner object as an argument (parameter).

var person = {  
  fullName: function() {  
    return this.firstName + " " + this.lastName  
  },  
}  
var person1 = {  
  firstName: "John",  
  lastName: "Doe",  
}  
var person2 = {  
  firstName: "Mary",  
  lastName: "Doe",  
}  
person.fullName.call(person1) // Will return "John Doe"

So **call()** : A function with argument provided individually. If you know the number of arguments to be passed or there are no argument to pass you can use call.

**apply()** : Calls a function with argument provided as an array. You can use apply if you don’t know how many arguments are going to be passed to the function.

When calling a function of the form **foo.bar.baz()**, the object **foo.bar** is referred to as the receiver. When the function is called, it is the receiver that is used as the value for this. If there is no explicit receiver when a function is called, then the global object becomes the receiver.

Because functions are first-class objects in JavaScript, they can have their own methods. All functions have the methods call() and apply() which make it possible to redefine the receiver (i.e., the object that **this** refers to) when calling the function.

There is a advantage of using apply() over call(), we don’t need to change the number of argument only we can change an array that is passed.

There is not big difference in performance. But we can say call is bit faster as compare to apply because an array need to evaluate in apply method.

The call() method invokes the function and uses its first parameter as the this pointer inside the body of the function. In other words — we’ve told the runtime what object to reference as ‘this’ while executing inside of function f().

The apply() method is identical to call(), except apply() requires an array as the second parameter. The array represents the arguments for the target method.

Both call() and apply() are methods we can use to assign the **this** pointer for the duration of a method invocation.

global.x = 10  
/\* To run this file in my vs-code or in terminal (i.e. where I am in node env),  
I have to use global . where as < var x = 10 > will work in browser dev-tool

var x = 10 \*/

var o = { x: 15 }

function f() {  
  console.log(this.x)  
}

f() // => 10

f.call(o) // => 15

**Importantly note, the call() method as above will NOT work in arrow function. And this.x will produce undefined. Because, Unlike regular functions, arrow functions do not have their own ‘this’**

global.x = 10

const obj = {  
  x: 15,  
  func: () => console.log(this.x),  
  func2: function() {  
    console.log(this.x)  
  },  
}

const func = () => console.log(this.x)

func() // => undefined  
func.call(obj) // => undefined  
obj.func.call(obj) // => undefined  
// But the following will work as expected  
obj.func2.call(obj) // => 15, accessing the

The first invocation of f() will display the value of 10, because this references the global object. The second invocation (via the call method) however, will display the value 15.

15 is the value of the x property inside object obj.

#### What is the difference between call, apply, and bind?

At a very high level, call and apply execute a function immediately. Bind returns a new function.

Call and apply are very similar in that they allow you to invoke a function. The difference is that call takes arguments one by one, apply takes in arguments as an array.

Consider the following examples.

var john = {  
  favoriteFood: 'pizza'  
}

var bob = {  
  favoriteFood: 'spaghetti'  
}

var favFood = function(eatAction, afterEatAction) {  
  console.log('It\\'s time to ' + eatAction + ' ' + this.favoriteFood + '! Then ' + afterEatAction + '.')  
}

bob.favFood('scarf down', 'sleep')

// bob.favFood is not a function...  
// Results in error, favFood is not a method on bob  
// In order to use this method for bob, I need to use call or apply

favFood.call(bob, 'scarf down', 'sleep') //It's time to scarf down spaghetti! Then sleep.

favFood.apply(john, \['scarf down', 'sleep'\]) //It's time to scarf down pizza! Then sleep.

favFood.call(john, \['scarf down', 'sleep'\]) //It's time to scarf down,sleep pizza! Then undefined.

// Notice this is not what we want, but doesn't hard error out.

// On the other hand, if I invoke apply() without passing the arguments as an array

favFood.apply(bob, 'scarf down', 'sleep') //Uncaught TypeError... hard error

**Bind is used to return a function that can be invoked at a later time.**

var eatThenSomething = favFood.bind(bob)  
eatThenSomething('gobble', 'nap')   
//It's time to gobble spaghetti! Then nap.

Next example of bind():

const obj  = {  
    x: 42,  
    getX: function() {  
        return this.x;  
    }  
}

const unBoundX = obj.getX  
console.log(unBoundX()); // => undefined

// But to get it to work  
const boundX = unBoundX.bind(obj)  
console.log(boundX()); // => 42

And there we have it. Hopefully you now feel a bit more confident about the topics we have discussed!

By [Rohan Paul](https://medium.com/@paulrohan) on [July 16, 2020](https://medium.com/p/7ebba74bc8af).

[Canonical link](https://medium.com/@paulrohan/tackling-common-javascript-interview-questions-part-1-7ebba74bc8af)

Exported from [Medium](https://medium.com) on December 12, 2020.