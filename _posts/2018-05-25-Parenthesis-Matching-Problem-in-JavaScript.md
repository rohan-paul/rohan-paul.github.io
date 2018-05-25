---
layout: post
title: ExpressJS - Parenthesis Matching Problem in JavaScript @ The Hacking School Hyd

comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/Parenthesis-Matching.jpg" class="fit image">


A classic problem — Write a **balancedParens** function that takes a string as input and returns a boolean — if the parentheses in the input string are ‘balanced’, then return true, else return false.

Here’s my simple implementation with stack data structure.


<script src="https://gist.github.com/rohan-paul/b196a23b71e2c2474134df907f29357c.js"></script>

First, I assign an object with opening and closing key-value pair. Then under the for loop, while traversing the given string argument passed to the function, I push to the ‘stack’ array, only the closing braces for each opening braces found in the passed-in argument. So, the stack array will be an array of closed braces.

If a ‘closed’ char is found, check if the matching closed parenthesis of the last element that is popped off the stack (i.e. for the last open parenthesis symbol found in the passed-in string) is not equal to the current chr. And if the chr isn’t the matching closed parenthesis for the last open parenthesis from the stack, then we return false because we have an imbalanced input.

**Time Complexity**

The above solution iterates the length of the input string, so our time cost will grow in linear proportion to the growth of the string length. Or, for each character that is added to the input string, the algorithm will take 1 time unit longer to complete (worst case). All other operations in our algorithm are constant time because we are using object-property lookup to find our comparison values and the pop method of a Stack data structure to keep track of all the parenthesis pairs that get opened.

And then below is a much more beautiful and shorter solution using **Array.reduce()** method.

<script src="https://gist.github.com/rohan-paul/76d3a1272f5cd6f6d434f67ea6aec390.js"></script>

Basically, the above function just increments the variable uptoPrevChar for each opening parenthesis and reduces it for each closing parenthesis. And ultimately I should just get zero for a balance string.

However, I have to return a boolean output — so I put a logical not ( ! ) at the very front of str. So for numerical return value of 0 for variable ‘uptoPrevChar’ — I return true.

Just paste this code, in Chrome DevTool > Source > Snippets > Put a breakpoint at the return value of ‘uptoPrevChar’ on the second else if iteration > right click on the snippets and run > The continue clicking on ‘resume script execution’

And another variation of the above using ES6

<script src="https://gist.github.com/rohan-paul/56dc0fad7e9f7c0581eb3eb1b9cb2f1e.js"></script>