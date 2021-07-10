# Recursion in JavaScript — Call a smaller version of you

The great thing about computers is “In order to understand recursion, one must first understand recursion.” — (Anonymous / Unknown)

![](https://cdn-images-1.medium.com/max/2560/1*6pYsnHbfy4nt3Yyc1yJQbQ.jpeg)
undefined

> The great thing about computers is **“In order to understand recursion, one must first understand recursion.”** — (Anonymous / Unknown)

In computer science, Recursion is the process of repeating a procedure, subject to a condition when one of the steps of the procedure is to call or invoke the procedure itself. More simply “a recursive function is simply a function that in some point calls itself”.

Recursion is best applied when I need to call the same function repeatedly with different parameters from within a loop. While it can be used in many situations, it is most effective for solving problems involving iterative branching, such as fractal math, sorting, search alogorithm or traversing the nodes of complex or non-linear data structures.

Although Javascript doesn’t have a solid [tail call optimization](http://wiki.c2.com/?TailCallOptimization), recursion is sometime the best way to go. And for careful developers, except in edge cases, we’re not going to get call stack overflows. Further, now ECMAScript 6 offers tail call optimization, where you can make some function calls without growing the call stack.

In the debate of choosing Recursion in Javascript as opposed to some other alternative, performance is ofcourse very important, and so is premature optimization. If you think that recursion is more elegant than iteration, then you should use it. On the other hand, if it turns out this is your bottleneck, then you can replace with some simple iteration. Also note, beyond, elegance of your code, readability and maintainability are the other 2 very important factor to consider, and in some cases, recursion does give very easy to read, thus maintainable code.

#### **Relationship between Recursion and Induction — when programming recursively, think inductively**

These 2 are highly related concepts. Lets say, we have to define a `function(n)`

**Recursive way would be** — functionn (n) calls itself until it meets a base terminal condition, while **Induction** is when base condition is met, trying to prove **base ase+1 is also correct.**

The way I approach them is, when defining a recursive function, making sure to write down a clear, concise specification of its behavior, then mentally trying give an inductive proof that my code satisfies the specification. So rule of thumb for me is, **when programming recursively, think inductively**.

A recursive definition defines an object in terms of smaller objects of the same type. Because this process has to end at some point, we need to include explicit definitions for the smallest objects. So a recursive definition always has two parts:

• Base case or cases

• Recursive formula/step

Lets solve few simple but classic problems with Recursion

**Example-1: Raise a number to its power**

undefined
undefined

It is generally helpful in understanding recursive functions by breaking it down all the way to the tail case, like the above.

The key here is that `power` is calling itself exactly in the way it would call any other function. So when it does that, it waits for the function to return and uses its return value. The call to **_power(3, 0)_** will return 1, which is then used by the previous call in the chain, to complete its work and return 3 \* 1 = 3.

In Recursion, we always need to define an abort-condition. Here, that condition is that the exponent becomes 0. So until the function reaches it’s abort-condition or base case it stacks the functions, and when it has a final number and not another function it calculates all the stacked function top to bottom.

So there are few steps we have to perform in designing any recursive solution. -A) Define what could be the base casess of this recursive solution, like in the above case when the exponent is 0. A base case is a specific condition that causes the function to return a value instead of calling itself again. -B) Divide the problem into one or more simpler parts of the main problem. End result of this exercise usually comes out to a mathematical formula expressed in form of a function:

`f(n1) = g(f(n2))`

Where, n1 = Current input to recursive function n2 = Reduced or simpler input passed in next recursive call g = Some pre/post processing operations need to applied to get fn1n1 value from reduced case fn2.

**Example-2: Simple Multiplication**

To multiply a and b recursively, means add a, b number of times

![](https://cdn-images-1.medium.com/max/800/1*PSCyZMT9Om3rbtJ8KONWbQ.png)
undefinedundefined
undefined

And note that in the code above I dont need to test the second parameter to see whether the second parameter is negative a<0a<0. This is because of the mathematical principle **a negative plus a negative is a negative or** −1+−1=−2−1+−1=−2. Essentially, if I know I am multiplying aa by a negative number −b−b, I know I am taking -a and adding it together b number of times; therefore, if the multiplicand or multiplier is negative, I know I can take either of the numbers, make the number negative, and add the number to itself over and over again, e.g. -3 \* 7 could be written −3−3 + −3−3 + −3−3 + −3−3…upto seven times OR −7−7 + −7−7 + −7−7 upto three time.

**Example-3: Factorial of a number**

undefined
undefined

**Complexity of recursive factorial program**

First lets, see the same factorial function written using plain loops

```
function factorial (num) {  let result = 1;  for (var i = 0; i < num; i++) {    result = result * (num - i);  }  return result;}
```

To compare the performance of the 2 approach, I wrote this quick script and see that the recursive solution is way faster than the looping solution.

![](https://cdn-images-1.medium.com/max/800/1*PIOOenYRM_fGxIvnOPZHUQ.png)
undefined

The algorithm is linear, running in **On time**. This is the case because it executes once every time it decrements the value n, and it decrements the value n until it reaches 0, meaning the function is called recursively n times. This is assuming, of course, that both decrementation and multiplication are constant operations.

**Example-4: A scarry looking one, calculating the value of** ππ **using the** [**Wallis Product**](https://en.wikipedia.org/wiki/Wallis_product) **series**

![](https://cdn-images-1.medium.com/max/800/1*v9JW5B69noXxYmaS5JeLaw.png)
undefined

**Thoughts to solve**

A) Start with the fractions 2/1 and 2/3.

![](https://cdn-images-1.medium.com/max/800/1*GzVipsD7lHRl8x9Iz0scKg.png)
undefined

B) Increase each number by 2 to get a new pair of fractions 4/3 and 4/5, and multiply this pair with the previous fractions:

![](https://cdn-images-1.medium.com/max/800/1*24pdA4lY_3rU8y5QEQKbNg.png)
undefined

C) Each new pair is \[2n/2n–12n–1\]\[2n/2n+12n+1\]. Repeat this indefinitely and multiply all terms together. The product, as n goes to infinity, is known as the Wallis product, and it is amazingly equal to π/2 ≈ 1.571.

**A regular iterative solution would be**

undefined
undefined

**Now lets solve it recursively**. First we will simplyfy the above formulae further

![](https://cdn-images-1.medium.com/max/800/1*3Qv0f73CweraNtYm19WgWA.png)
undefinedundefined
undefined

A quick look at the performance of the above 2 methods gives insiginificant difference. Ofcourse for larger value of `num` the rucursive method throws a “Maximum call stack size exceeded” error.

![](https://cdn-images-1.medium.com/max/800/1*qb4hlmaMme46Z7G93ct7FA.png)
undefined

**Example-5: Sum of an array elements**

undefined
undefined

In a recursive approach in the above problem, we need to model the task as reduction to a base case and here the base case is the empty array — at which point, I set my function to return zero. And then, I model the sum of the array as the result of adding the first element to the sum of the remainder of the array elements — at some point, these successive calls will eventually result in a call to `sumArrRecurse([])`, the answer to which has already been set to be 0.

While using Recursion, the part we have to be most careful about, is the call stack. A stack is a block of memory that keeps track of the function’s information. Creating a stack causes the JavaScript engine to work much harder and drain memory. This can cause an error if not coded correctly.

In JavaScript every function call will add a call frame to the call stack. The frame is popped off the call stack when the call returns. However a recursive function does not return immediately. It will make a recursive call before it returns. This will add to the call stack every time. The call stack takes up space, and if I push too many functions, Recursively calling from the tail position sufficently deep onto the call stack, before it can pop them off I will get a ‘Maximum call stack size exceeded’ error.

The CallStack holds a list of functions that have been called to run our code up to the point we have the breakpoint. The call stack is a specific implementation of the stack data structure. It is a LIFO Lastin,firstoutLastin,firstoutdata structure, meaning that function calls placed on the top of the call stack are also the first ones to be popped off.

**Example-6: Reverse a string**

undefined
undefined

**Example-7: Converting Decimal to Binary**

undefined
undefined

**Example-8: Calculate the logarithm of a number with a base of 2**

undefined
undefined

**Example-9: Classic problem of finding the n-th fibonacci number**

The Rule is . In general, fibonnacinn = fibonnacin−2n−2 + fibonnacin−1n−1. By definition, the first two numbers in the Fibonacci sequence are either 1 and 1, or 0 and 1,\* depending on the chosen starting point of the sequence. In the below solution I am assuming, the series starts with zero. That is, fibonacci00 should return 0, not 1. If however, I wanted the series to start from 1, I would put the first condtion as if n<2n<2 { return 1 } So the final series will look like below.

![](https://cdn-images-1.medium.com/max/800/1*780VhPUyfACI-4QzqgoiJg.png)
undefinedundefined
undefined![](https://cdn-images-1.medium.com/max/800/1*f8i_0CJiUDH1rw-1momgEg.png)
undefined

#### **To actually view Call Stack in Chrome Developer Tool**

Run your code file `( the .js or the .html file )` in a browser. Bring up developer tools by pressing F12. All other major browser vendors have their own built-in developer tools. On the top tab, you will see menu labels such as Elements, Profiles, Console, Network, Sources, etc. Click on **Sources**.

**If you are using Chrome, for running any javascript code snippets, that are small scripts, you can execute it within the Sources panel of Chrome DevTools. See this** [**official link**](https://developers.google.com/web/tools/chrome-devtools/snippets)**.**

On the left panel, click on .js or the .html file containing your code. To see all the stack of function that JavaScript is accumulating, we would make use of breakpoint. A breakpoint is a section/line of our code where we want the execution to stop so that we can carefully inspect the execution. So, click on the line no in your .js / .html file where you want to apply that breakpoint. In the below example, I am using the `**mult()**` function, that I defined above, for this purpose and I apply the break points at the line `**alert(mult(-4, 5))**`

To apply the breakpoint, click the line number in the js / html file within the Developer tool and it should change color, in my case it’s blue. And now we have set a breakpoint on our code at that particular line of code. This means that when the function is run, its execution is supposed to stop where the breakpoint is. We should then be able to execute the code line by line, seeing changes made to our variables.

There is an overlay in the browser with a section written `Debugger paused` and above it will be four buttons. The first button with a play-pause-like sign is for resuming the script execution. Clicking it will continue the execution of the script and exit the debugger. The next three buttons are:

**Stepping Over** — This button with a bent arrow pointing downwards and a dot is for stepping over the next function call. This is what you will use most of the time. It simply executes the current line, and stops at the next line.

**Stepping Into** — This button with a downward pointing arrow is for stepping into the next function call. This takes the debugger into a function, if that line calls a certain function.

**Stepping Out** — And this one with an upward facing arrow is for stepping out of the current function.

![](https://cdn-images-1.medium.com/max/800/1*U7QcimfFMAVeG1wABWPxaA.png)
undefined

After each time I click the **Stepping Into** button with my `mult(-4, 5)` function, I will see the actual recursion steps in live action, starting from b = 5 to gradually b = 0. And now see how the Call Stack is growing taller with the `mult()` function.

![](https://cdn-images-1.medium.com/max/800/1*OC2gCQWVyck1Ms0-GOXTfQ.png)
undefined

When working on recursive function, examining the call stack by stepping into each function call via a debugging tool is something you can not avoid, so better get aquainted with these tools at an early stage.

By [Rohan Paul](https://medium.com/@paulrohan) on [July 28, 2019](https://medium.com/p/707263616938).

[Canonical link](https://medium.com/@paulrohan/recursion-in-javascript-call-a-smaller-version-of-you-707263616938)

Exported from [Medium](https://medium.com) on December 12, 2020.