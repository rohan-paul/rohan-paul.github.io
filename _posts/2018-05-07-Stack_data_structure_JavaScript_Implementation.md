---
layout: post
title: Stack Data Structure-JavaScript Implementation @ The Hacking School-5th-Week
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/stack-data-structure.jpg" class="fit image">

5-th week running at **[The Hacking School](http://thehackingschool.com/)** and we are trying to wrap our head around with one of the most commonly used data structures in web development - **Stacks** that is. 

Lets first see couple of enlightening examples of its daily usage: the 'undo' operation of a text editor uses a stack to organize data.  We can use 2 stacks **Undo/Redo**. When I undo an action it is popped off the action stack and placed on the Redo Stack. If I redo, I then pop it off the Redo Stack and push it onto the action stack. 

An in the browser it keeps a stack of the web pages that I have visited in that window. When I press the back button, the browser peeks element from the top of the stack (list of webpages) and takes me back to the previous page.


The stack follows a **last-in, first-out (LIFO)** data structure and because of which any element that is not currently at the top of the stack cannot be accessed directly (like we do in an array with index no position). To get to an element at the bottom of the stack, I have to dispose of all the elements above it first.


JavaScript already supports a stack out of the box using an array. We can easily push items onto the stack and pop them off. Keep in mind that as you pop values off the stack they'll be in reverse order that you put them on. Further, A stack is a dynamic data structure. It grows in size when a new item is added and it shrinks in size when an item is removed. In this way it differs from the array, which is
a collection of a fixed size. A stack differs from an array in another way also. A stack has an access discipline which dictates that items may only be added to the top and they may only be removed from the top. In contrast, an array is a random access data structure.

Here's a simple class implementation of the Stack class.

<p data-height="986" data-theme-id="0" data-slug-hash="QrOgQX" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="stack-data-structure-1-blog" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/QrOgQX/">stack-data-structure-1-blog</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


## Recursion and Stack Data-structure

The call stack in recursion is a specific implementation of the stack data structure and its a LIFO structure, meaning that function calls placed on the top of the call stack are also the first ones to be popped off. Generally, stacks are used during any function invocation. With each invocation, a new function is pushed into the stack. And, whenever a function returns a value or reaches its end, it is popped out of the stack.

Computers track a program's execution using the call stack. When a function is executed, a new stack frame is added to the stack. That stack frame represents the work that is performed by that function. The CallStack holds a list of functions that have been called to run our code up to the point we have the breakpoint. 


Lets take the following example of finding the factorial of 4

<p data-height="341" data-theme-id="0" data-slug-hash="QMNbGM" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="factorial_recursive.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/QMNbGM/">factorial_recursive.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

 - **First** I start the execution of finding the factorial of 4. I return 4 multiplied by factorial of 3. In order to return the answer, I must put the current task on hold (into the stack of tasks) and find the factorial of 3.

 - **Then** I start the task of finding the factorial of 3. And so I return 3 multiplied by factorial of 2. In order to return this answer, I must put the current task on hold (into the stack of tasks) and find the factorial of 2.

 - Now, start the task of finding the factorial of 2. I return 2 multiplied by factorial of 1 - and put the currrent task (of finding factorial of 2) in the stack of tasks. But at this stage, to find the factorial of 1, I also hit the terminal condition.


 - Now, I need to come back to where I started, now that I have completed all recursive calls. I take the task at the top of the stack, where I had to return the product of 2 and factorial of 1, which is, as we already know, 1. We return 2, and pop it.

 - Then I take the task at the top of the updated stack. We had to return the product of 3 and factorial of 2, which is, as we already know, 2. We return 6. And then pop.

 - And then, I take the task at the top of the stack. We had to return the product of 4 and factorial of 3, which is, as we already know, 6. We return 24.

 - There are no more tasks in the stack of tasks. We have found 24, the factorial of 4.


So, going over it again, fundamentally, when at the first call I have to determine ``4 * factorialize(3)``, how can we multiply 4 by the value of something that hasn't even been determined yet?

And thats why I don't, not until I know the value of factorial(3). When factorial(3) is called, we don't actually do the multiplication yet, until we have a definite answer for that. So the 4 stays there and waits for the result of the function call to come back, but then in the next step, the 3 also waits for factorial(3-1).. etc. Once the final call (i.e. ``factorial(1))`` returns a definitive answer, it multiplies by whatever was waiting for it, and that in turn goes and multiplies with the previous waiting call.

**So I have a top-down call of methods, and when I have the values, a bottom-up of multiplications occur. As illustrated in this diagram:**

<img src="/images/fulls/factorial-stack-blog.gif">

**Now lets look at a very well-known problem using stack, that given a set of parenthesis to determine if its matched i.e. each opening parenthesis has a corresponding closing parenthesis in the given string.**

<p data-height="885" data-theme-id="0" data-slug-hash="odopbM" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="parenthesis-match-stack-blog" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/odopbM/">parenthesis-match-stack-blog</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

**Exaplanation**

 - For each element of the string argument, iterate through each element > check if the current element is any of the opening parenthesis array > If true, push it to stack >> IF NOT >> there could be 3 cases, for each of the parenthesis i.e. ``], ), }``

 - check all the 3 cases switching through them >  and comparing this current closing parenthesis with the top element from the stack. Note, that I have only pushed elements from the opening parenthesis to the stack.

 - If any match is found from in the closingParenthesis array and the top-most element from stack, then just pop that element from stack. And apply break for this iteration. >> So, for each of the

**Explanation - Why I am ONLY checking the top-most element of the stack ( with ``stack.peek()`` ) with each of the closing parenthesis under the switch statement**

By the design of this code, I am only making sure that to return ``true`` from this function, either 
 - A- all the opening parenthesis will come first ( like in this case - "{[( )]}" ) - or 
 - B- corresponding matching parenthesis will come one after another (side by side) ( like in case "{()}[]"  )

**In the first case A** - The first time I hit a closing parenthesis, the switch function will switch cases, through the 3 of them, and first will pop-out the "(" parenthesis - as the closing ")" will be matched.
Then the next iteration will pop-out the straight bracket "[" - as that will be matched. And lastly the left over curly brace "{"

**And then in the case B** - **``{()}[]``** - Its eazy to see that the part ``{()}`` is same as the case-A above, and the part ``[ ]`` is obvious that the peek element will be the only possible matching element to pop out from the stack.

And with the above code, third case, that this code will return false, is when although there are matched parenthesis, but they are NOT arranged like in the above 2 cases (i.e. A> all opening parenthesis first then all corresponding closing parenthesis or B> opening and closing parenthesis side by side)



