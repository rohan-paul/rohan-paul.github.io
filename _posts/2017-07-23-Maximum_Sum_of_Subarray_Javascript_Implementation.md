---
layout: post
title: Maximum Sum of Subarray, a Javascript implementation
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/Max-Subarray-Sum-Kadane.jpeg" class="fit image">

The maximum subarray sum, a very famous and classic Computer Science problem, that I just faced doing one of [Leetcode challenges](https://leetcode.com/problems/maximum-average-subarray-i/#/description).

**Original Problem: -**

Given an array consisting of n integers, find the contiguous subarray of given length k that has the maximum average value. And you need to output the maximum average value.

Example 1:
Input: [1,12,-5,-6,50,3], k = 4
Output: 12.75
Explanation: Maximum average is (12-5-6+50)/4 = 51/4 = 12.75

Below is my code to solve this using Kadane's Algorithm.

<script src="https://gist.github.com/rohan-paul/812601b55ff02bfc7fdf201febedbbd1.js"></script>


Now the above Leetcode challenge is a special case of the general [**Max Subarray**](https://en.wikipedia.org/wiki/Maximum_subarray_problem) classic problem in computer science - which is the task of finding the contiguous subarray within a one-dimensional array of numbers which has the largest sum. For example, for the sequence of values −2, 1, −3, 4, −1, 2, 1, −5, 4; the contiguous subarray with the largest sum is 4, −1, 2, 1, with sum 6.

We will solve it under 3 different approaches with different time compexities:-

**Brute force approach to solve - Most inefficient time complexity**

Any subarray will be defined by two indices which are bounded by the size of array. And here, to find the maximum sum contiguous subarray, we would run two loops. The outer loop picks the beginning element, the inner loop finds the maximum possible sum with first element picked by outer loop and compares this maximum with the overall maximum. Finally return the overall maximum. Basically, we try all possible combinations and add them up, so we have totally Cn1 + Cn2 + ... + Cnn = n! combinations.

So, time complexity of brute force algorithm would be either O(n^2) or O(n^3).

In the below solutions we generate all (i, j): i <= j pairs and calculate the sum between. The time complexity is O(N^3)

The difference between the O(N^2) and O(N^3) functions is that in the O(N^2) function, the sum is computed implicitly every time the end index is incremented, while in the O(N^3) function, the sum is computed with a third explicit loop between start and end.


<script src="https://gist.github.com/rohan-paul/f1d5fc300939068950329f50c33a87e3.js"></script>

**Divide and conquer approach to solve - Much improved time-complexity**

First, the nature of divide-and-conquer is to solve problems recursively as such:

- Divide the problem into subproblems;
- Conquer the subproblems by solving them recursively(if their size are small enough, we will solve them straightforward)
- Combine the subproblem solutions into a solution for the main problem.


Split the input in half, and consider that the maximum sum subarray can come from three places:

- A. Entirely from left half.
- B. Entirely from right half.
- C. It consists of some number of rightmost elements of the left half and some number of leftmost elements of the right half.


Then solve the problem recursively on the left and right halves.  I would then know the maximum subarrays I can get that are entirely in each of the halves, for case A and B above.

And for case C (such that the final maximum subarray crosses the midpoint), we note that the number of elements to take from the left half and the right half should be chosen to maximize their contribution to the sum. So then, we are simply looking for the largest prefix sum in the right subarray (i.e. having max sum of its leftmost elements), and the largest suffix sum in the left subarray (i.e. having max sum of its rightmost elements). We can solve these 2 problems trivially in O(n) time and O(1) space.

**We can easily find the crossing sum in linear time. The idea is simple, find the maximum sum starting from mid point and ending at some point on left of mid, then find the maximum sum starting from mid + 1 and ending with sum point on right of mid + 1. Finally, combine the two and return.**

It's then easy to compare options A, B, and C, and find the maximum.


Suppose we want to find a maximum subarray of the subarray A[low .. high]. Divide-and-conquer suggests that we divide the subarray into two subarrays of as equal size as possible. That is, we find the midpoint, say mid, of the subarray, and consider the subarrays A[low..mid] and A[mid+1..high. Any contiguous subarray A[i.. j] of A[low..high] must lie in exactly one of the following places:

- entirely in the subarray A[low..mid], so that low <= i<= j<=mid,
- entirely in the subarray A[mid+1..high], so that mid < i<= j<=high, or
- crossing the midpoint, so that low <= i <=mid < j<=high.

We can find the maximum subarray of A[low…mid] and A[mid+1…high] recursively because they are subproblems and thus smaller instances of the main problem. Consequently, all there is left to do is finding a maximum subarray which crosses the midpoint and take a subarray with the largest sum of the three. 

Isolating a subarray crossing the midpoint can be achieved in time linear in the size of the subarray A[low…high]. However, this specific problem is not a smaller instance of the original problem due to the condition that the subarray must cross the midpoint. As a result, any subarray crossing the midpoint is itself made of two subarrays A[i…mid] and A[mid+1…j], where 
low ≤ i ≤mid and mid < j ≤ high. 
It therefore becomes a simple matter of finding the maximum subarrays of the form A[i…mid] and A[mid+1…j] and combining the two.


Step1. Select the middle element of the array.
So the maximum subarray may contain that middle element or not.

Step 2.1 If the maximum subarray does not contain the middle element, then we can apply the same algorithm to the the subarray to the left of the middle element and the subarray to the right of the middle element.

Step 2.2 If the maximum subarray does contain the middle element, then the result will be simply the maximum suffix subarray of the left subarray plus the maximum prefix subarray of the right subarray

Step 3 return the maximum of those three answer.

Here's the code

<script src="https://gist.github.com/rohan-paul/be2aef2124c1931c1e4a584fccf9ffa9.js"></script>

**Now, Analyzing the time complexity of the divide and conquer approach:**

Now we want to know how good is our approach, in terms of computer time using asymptotic efficiency. We’ll use the formula from the top:

**T(n) = T(base case) * (way input was divided) + T(combining solutions).**

 **A. 2T(n/2)**: The input is divided by two each time, T is the time working on each half, which we’re yet to know, and 2 comes from having to work with each half (we don’t work with one half only, we work with both).Note how this term is in terms of T since it’s the time it’ll take within itself (the calls that the method will do to itself). That’s why it’s not bounded on an asymptotic notation, we’re not sure yet how this will behave. That’s exactly what we’re trying to solve.

 **B. O(n)**: The time of going through the array to find the crossing subarray, this step will just go through the whole array once, that’s why it can be bounded and expressed as a term of asymptotic notation. Recall that even if the term would have been O(2n) we can ignore the 2.

 **C. O(1)**: The time of dividing, kind of trivial doing so by indexes. It’s constant time, and the same as above, even if the term would have been O(4500) we simplify it to O(1).


Fitting the above in the formula:

T(n) = O(1) + 2T(n/2) + O(n).

We’ll rearrange the terms so there are from greatest to smallest:

**T(n) = 2T(n/2) + O(n) + O(1)**

Using the Master Theorem, will leave us with an algorithm that is bounded on **O(n log n)**.



**Kadane's algorithm - The most efficient time-complexity**

We can easily solve this problem in linear time using Kadane's;s algorithm. The idea is to maintain maximum (positive sum) sub-array ending at each index of the given array. This subarray is either empty (in which case its sum is zero) or consists of one more element than the maximum subarray ending at the previous index.

The time complexity of above solution is **O(n)** and auxiliary space used by the program is **O(1)**

Simple idea of the Kadane's algorithm is to look for all positive contiguous segments of the array (max_ending_here is used for this). And keep track of maximum sum contiguous segment among all positive segments (max_so_far is used for this). Each time we get a positive sum compare it with max_so_far and update max_so_far if it is greater than max_so_far

Initialize: max_so_far = 0
max_ending_here = 0

Loop for each element of the array
(a) max_ending_here = max_ending_here + a[i]
(b) if(max_ending_here < 0) max_ending_here = 0
(c) if(max_so_far < max_ending_here) max_so_far = max_ending_here

return max_so_far

**My Implementation of Kadane's approach**


<script src="https://gist.github.com/rohan-paul/2663fd64a693ae18b07dfbd04e8412c2.js"></script>
