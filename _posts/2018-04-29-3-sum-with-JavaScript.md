---
layout: post
title: After Two-Sum there's Three-Sum - Solving Leetcode Challenge in JavaScript
comments: true
author: Rohan Paul
categories: JavaScript
use_math : true
---
<img src="/images/fulls/3-sum-Leetcode.jpg" class="fit image">


**Solving the Three-Sum Problem with JavaScript**

A great and classic challenge, **[3-Sum](https://leetcode.com/problems/3sum/description/)**, I faced in Leetcode, and can be solved with variying level of efficiency and beauty.

**Problem Statement - Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero. Note: The solution set must not contain duplicate triplets. Example: Given array nums = [-1, 0, 1, 2, -1, -4]**

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]


**Solution-1-My Brute Force Solution**

The requirement is to find all unique triplets in the array. That is, the solution set must not contain duplicate triplets. This means in the resultant array, no number would be repeated. So, I wil achieve this with the following 2 techniques.

A> First sort the array. So if there are multiple numbers, like two 5's they will come sequentially.

B> Then, within the loop, I will check for each iteration that the current element that I am taking is not === previous element.

My First native brute force solution of traversing the array 3 times checking for all combinations of 3 numbers to produced zeros. Although the code produced the correct result in my local machine, but it took so long in Leetcode's exhaustive test-cases, that it gave me "TIME LIMIT EXCEEDED" error in Leetcode.


