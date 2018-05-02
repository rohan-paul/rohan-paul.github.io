---
layout: post
title: After Two-Sum there's Three-Sum - Solving Leetcode Challenge in JavaScript
comments: true
author: Rohan Paul
categories: JavaScript
use_math : false
---
<img src="/images/fulls/3-sum-Leetcode.jpg" class="fit image">


**Solving the Three-Sum Problem with JavaScript**

A great and classic challenge, **[3-Sum](https://leetcode.com/problems/3sum/description/)** and  extremely popular for interviews. It can be solved with variying level of efficiency and beauty. Its a simple problem on the face of it, but there are a couple things that make it tricky. Evidence how much 3-Sum is loved: 


- This [Quora thread](https://bit.ly/2FrFQaL)

- More than 1.5 million submissions, 320k Accepted answers and 1500+ upvotes on [leetcode](https://leetcode.com/problems/3sum/description/).


**Among some of the unresolved problems in Computer Science on planet earth, there remains this one - [Can 3SUM be solved in O(n^{1.99999}) time i.e. better than O(n^2) time ?](https://www.quora.com/What-are-the-unsolved-problems-of-computer-science)**

It’s conjectured to be impossible to find an **``O(n^(2−ϵ))``** algorithm for solving 3SUM, for any positive constant ϵ. Although, a pretty recent research, shaved off some log factors for the 3SUM problem giving a sub-quadratic deterministic algorithm : [Threesomes, Degenerates, and Love Triangles](https://ieeexplore.ieee.org/document/6979047/?arnumber=6979047&reload=true) and probably the best known general algo for 3-Sum at the moment.


**The Problem Statement - Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero. Note: The solution set must not contain duplicate triplets. Example: Given array nums = [-1, 0, 1, 2, -1, -4]**

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]


**Solution-1-Brute Force**

The requirement is to find all unique triplets in the array. That is, the solution set must not contain duplicate triplets. This means in the resultant array, no number would be repeated. So, I wil achieve this with the following technique.

A> The most simple way to remove duplicates is to stop considering the same value for the same index more than once. Don’t let i, j or k refer to the same value in the array twice. Let x refer to some value in the array. Once we have set i=x and found all y, z such that x+y+z=0, we never have to consider setting i=x again.

B> First sort the array. So if there are multiple numbers, like two 5's they will come sequentially. So, whilte iterating for say ``i``, I will hit the two sequential ``5's`` in my two sequential iteration. So I can skip over the second iteration, usingn ``continue``.

C> So after sorting the array, will check for each iteration that the current element that I am taking is not === previous element. That is if ``nums[i] === nums[i-1]) continue``

D> An example below of stopping duplicates.. 

<img src="/images/fulls/3-sum-inside-the-post-image.png">

**My First native brute force solution below** of traversing the array 3 times checking for all combinations of 3 numbers if their sum produces zero. Although the code produced the correct result in my local machine, but it took so long in Leetcode's exhaustive test-cases, that it gave me "TIME LIMIT EXCEEDED" error there in Leetcode.


<p data-height="659" data-theme-id="0" data-slug-hash="NMdgOM" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="3-sum-brute force blog" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/NMdgOM/">3-sum-brute force blog</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Note, the 2nd and 3rd for loops (indexed by j and k) stars from ``i+1`` and ``j+1`` respectively, skipping over indices that are already used, instead of 0. This is so, because, suppose we found a triplet, ``a0, a1, a2`` with j pointing to ``a0`` and i pointing to ``a1`` with ``j < i``. Then, we would have found the same triplet with ``i`` pointing to ``a0`` and ``j`` pointing to ``a1`` on a previous iteration giving us a duplicate output.


**Complexity Analysis**

**Time complexity : O(n^3)**

Because each of these nested loops executes n times, the total time complexity is **O(n^3)**, where n is a size of an array.

Space complexity : **O(n^2)**. If we assume that resultant array is not considered as additional space we need **O(n^2)** space for storing triplets in a set.


**Solution-2- First iteration at finding a ``"2-pointer-solution"`` that runs in ``O(n^2)`` Time**


<p data-height="675" data-theme-id="0" data-slug-hash="pVRdaO" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="3-Sum-2Pointer-Solution-2ndBest-Blog" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/pVRdaO/">3-Sum-2Pointer-Solution-2ndBest-Blog</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


This runs hugely faster vs the Brute Force one, however, there's still little room for optimization to the above code. If I put the a line **``if (arr[aIndex] > 0) return result``** immediately after I start the first for loop (i.e immediately inside ``for (let indexA = 0; indexA < arr.length - 2; indexA++)``) in the above code, it will gain some performance, because I will be skipping many iterations as soon as I hit the first element-value of the required result-array to be more than zero. So, below is my little more optimized version of a 2-pointer solution.


**Solution-3- Even Faster than above after little optimization ``"2-pointer-solution"`` in ``O(n^2)`` Time**

## The Algo

A> As a first step, just like the previous solution, since we're dealing with an array of numbers it makes great sense to sort things before diving into algorithm mechanics. 

B> Instead of 3 nested for loops, here I will use 1 for-loop and one while-loop. Fix the first element as array[i] where i is from 0 to array size – 2. After fixing the first element of triplet, find the other two elements using a ``while`` loop.

C> I will invoke the for-loop ``for (let indexA = 0; indexA < nums.length - 2; indexA++)`` and the first statement in my for-loop will allow the first iteration to run and skip over any subsequent iterations that would lead to the same calulation. So, if my array becomes [1,1,1,3,3] and I am looking at the 0 element 1 - I no more iterate any further.

D> Now with each iteration of the first for loop, keeping ``a's`` value constant, I am looping the values of the next 2 variables ``b`` and ``c`` to check if ``(a + b + c = 0)``. ``b`` starting from indexA + 1 and increasing and ``c`` starting from the end of the array ``nums.length - 1`` and decreasing.

Since the number array is sorted, the plan is to pull b and c closer together - checking each time for the sum array values to equal 0.

E> If I see that the result (sum of Triplets i.e a + b + c) is more than zero, I can decrement from right-most elements to reach zero.

And if the result is less than zero, I increment the b's (middle element in the triplet) value to reach zero. Pictorially..

<img src="/images/fulls/3-sum-inside-the-post-image-2.png">

F> After all values of ``b`` and ``c`` are exhausted, I go back to the next iteration of the for loop for the next value of ``a``.

 Given ``a`` has the lowest value after sorting the array, and with each value of ``a``, I have already iterated through checking for all values of ``b`` and ``c``  to check if sum is zero - as soon as ``a`` becomes > 0 I can return from the function. Because it means I have reached a point where all the subsequent values of the array will be higher than zero. **So no point in iterating further when ``a > 0``**.

G> And ``a`` will only take values upto ``(nums.length - 3)`` as the last 2 values will be taken by the subsequent 2 variables ``b`` and ``c``

<p data-height="756" data-theme-id="0" data-slug-hash="bMgRQv" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="3-sum-Solution-2-HashMap-Blog" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/bMgRQv/">3-sum-Solution-2-HashMap-Blog</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


And see the huge performance difference with the 3 approaches to soution, running the code for an array with 2000 randomly generated elements.

```
var arr = Array.from({length: 2000}, () => Math.floor(Math.random() * 3000));

console.time("Solution-1-Brute Force");
threeSum_Brute(arr);
console.timeEnd("Solution-1-Brute Force");

console.log("***************************************************");

console.time("Solution-2-Sub-Obtimal-2-Pointer");
threeSum2 (arr);
console.timeEnd("Solution-2-Sub-Obtimal-2-Pointer");

console.log("***************************************************");

console.time("Solution-3-Optimized-Two-Pointer");
threeSumTwoPointer(arr);
console.timeEnd("Solution-3-Optimized-Two-Pointer");

```
**And output of the above the test above**

```

Solution-1-Brute Force: 2772.645ms
***************************************************
Solution-2-Sub-Obtimal-2-Pointer: 8.542ms
***************************************************
Solution-3-Optimized-Two-Pointer: 1.221ms

```