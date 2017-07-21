---
layout: post
title: A very nice use of bitwise XOR Operators in JavaScript in solving a CodeWar challenge
comments: true
author: Rohan Paul
categories: JavaScript
cover:  "assets/call-function.jpg"
---

Writing this post about bitwise XOR operator while I was solving a challenge or "Kata" as they call each challenge at [CodeWars](https://www.codewars.com) (the great site for coding challanges and getting a global peer rank). Lets refer directly to the problem (the below gist) along with my solution, so we can see how beautifully and efficently the XOR operatior could solve the same.

<script src="https://gist.github.com/rohan-paul/72c4b31c6394f5fe0539a3811583e568.js"></script>

And then, after submitting my solution, [CodeWars](https://www.codewars.com) showed me the most efficient solution (submitted by somebody else) of the same above problem, which is below.

<script src="https://gist.github.com/rohan-paul/1897ba275d3e7883f22378692bbf7feb.js"></script>

And I was amazed at this single liner. Its almost an work of art.

While generally its true that, code golfing, which is essentially performing a task in as few characters as possible, is not always the best solution for production environment, as other things like maintainability, human-readibility etc are very important as well. But in this case, the one liner solution by making use of the bitwise XOR (exclusive OR) operator was way better than mine.


**Explanation of bitwise XOR operator**

The [Mozilla official doc](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#Bitwise_XOR) defines the bitwise XOR operator as - "Performs the XOR operation on each pair of bits. a XOR b yields 1 if a and b are different. Returns a one in each bit position for which the corresponding bits of either but not both operands are ones."

The bitwise operators work with 32-bit integers. That is, they convert their operands to 32-bit integers (i.e. zeroes and ones), then perform their operations on such binary representations, but they return standard JavaScript numerical values. XOR returns a one (true) in each bit position for which the corresponding bits of either but not both operands are ones.

XOR is a boolean operator, like && and \|\|, but with the following logic:

**It is successful if the expression on either side is true (i.e. resulting in 1 ) (like \| \|), but false (i.e. resulting in 0 ) if both sides are true (like !( x && y )).**

**So if I try to represent the above in equation format, the XOR operator would  work like this (the result is true if exactly one of the operands is true):**

**a ^ b === (a && !b) \| \| (!a && b)**

In the above, one boolean operation (0 is false, 1 is true) per bit of each number is performed.


**Explanation of the code using XOR operator**

So in this case here, in the one liner solution above, the bitwise XOR operator ("^") cancels out (i.e. results in 0) every number once it occurs twice. So, e.g. when a number occurs once, its not cancled, but if it occurs twice, it get cancelled-out. Then if it occurs a 3rd time, that 3rd time occurance does not get cancelled but on 4th time occurance, it gets cancelled. And the process continues - leaving the final odd number occurance.

XOR is both commutative ( e.g. a × b = b × a.) and associative (i.e. ( a × b ) × c = a × ( b × c ) ), and also the identities **X ^ X == 0** and **X ^ 0 = X** holds true. So, I can rearrange a sequence like: a ^ b ^ c ^ b ^ d ^ d ^ a any way I want, just like addition or multiplication.

So I might do: a ^ a ^ b ^ b ^ c ^ d ^ d. Since any two pairs become 0, this simplifies to 0 ^ 0 ^ c ^ 0, which is simply c.

**So, here in this example, by simply repeatedly using XOR on all the elements of the array, any duplicates naturally filter themselves out and finally I am left with the one non-repeated integer.**

The solution is great for performance, being super fast. The code go through the array list just once, and the only additional memory I need is a single value. No forEach(), no creation of an extra ``count`` object and then no additional processing loops over tht ``count`` object.

With a quick script below I tested the execution time difference (in
milliseconds) between my code and the one with the XOR operation. And the one
liner code got executed around 44-53 ms earlier.

Now if 44millisecond sounds too insignificant you should read about (from a 2008 study, which probably is even more relevant today than ever) how Amazon found every 100ms of latency cost them 1% in sales and Google found an extra .5 seconds in search page generation time dropped traffic by 20%. There's a huge cost of latency.

<script src="https://gist.github.com/rohan-paul/bac5818a64b1c3aafd2554c6601e5ad4.js"></script>

However, to remember here, this methodology only really works if one and only one unique value is appearing an odd number of times.