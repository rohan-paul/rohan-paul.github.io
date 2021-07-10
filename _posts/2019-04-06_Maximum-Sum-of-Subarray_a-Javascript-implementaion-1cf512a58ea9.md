# Maximum Sum of Subarray, a Javascript implementaion

The maximum subarray sum, a very famous and clssic Computer Science problem, that I just faced doing one of Leetcode challenges.

undefined
undefined![](https://cdn-images-1.medium.com/max/600/1*NxZYfKqJ-aAcJx5itKgmTA.jpeg)
undefined

The maximum subarray sum, a very famous and clssic Computer Science problem, that I just faced doing one of Leetcode challenges.

**Original Problem: -**

**Given an array consisting of n integers, find the contiguous subarray of given length k that has the maximum average value. And you need to output the maximum average value.**

**Example 1:  
Input: \[1,12,-5,-6,50,3\], k = 4  
Output: 12.75  
Explanation: Maximum average is (12–5–6+50)/4 = 51/4 = 12.75**

The above is a special case of the general [**Max Subarray**](https://en.wikipedia.org/wiki/Maximum_subarray_problem) problem in computer science — which is the task of finding the contiguous subarray within a one-dimensional array of numbers which has the largest sum. For example, for the sequence of values −2, 1, −3, 4, −1, 2, 1, −5, 4; the contiguous subarray with the largest sum is 4, −1, 2, 1, with sum 6.

**We will solve it under 3 different approaches with different time compexity**

#### **\*\*Brute force approach\*\***

Any subarray will be defined by two indices which are bounded by the size of array. And here, to find the maximum sum contiguous subarray, we would run two loops. The outer loop picks the beginning element, the inner loop finds the maximum possible sum with first element picked by outer loop and compares this maximum with the overall maximum. Finally return the overall maximum.

**_So, time complexity of brute force algorithm would be either O(n²) or O(n³)._**

**In the below solutions we generate all (i, j): i <= j pairs and calculate the sum between. The time complexity is O(N³)**

The difference between the O(N²) and O(N³) functions is that in the O(N²) function, the sum is computed implicitly every time the end index is incremented, while in the O(N³) function, the sum is computed with a third explicit loop between start and end.

undefined
undefined

#### \*\*Divide and conquer approach\*\*

Split the input in half, and consider that the maximum sum subarray can come from three places:

*   **Entirely from left half.**
*   **Entirely from right half.**

It consists of some number of rightmost elements of the left half and some number of leftmost elements of the right half.

Then solve the problem recursively on the left and right halves. I would then know the best subarrays I can get that are entirely in each of the halves, for points 1 and 2 above.

Step1. Select the middle element of the array.  
So the maximum subarray may contain that middle element or not.

Step 2.1 If the maximum subarray does not contain the middle element, then we can apply the same algorithm to the the subarray to the left of the middle element and the subarray to the right of the middle element.

Step 2.2 If the maximum subarray does contain the middle element, then the result will be simply the maximum suffix subarray of the left subarray plus the maximum prefix subarray of the right subarray

Step 3 return the maximum of those three answer.

Now, time complexity:

T(n) = 2\*T(n/2) + O(Max\_Opposite).

If function “Max\_Opposite” is O(n²), then T(n) = O(n²). But if we manage to make it O(n), then

**T(n) = O(nlogn)**

So, I have to save four values for each subarray:

*   **Maximum sum that is contained entirely in the left half ( mss\_l)**
*   **Maximum sum that is contained entirely in the right half ( mss\_r )**
*   **Sum of the whole array ( t )**
*   **Maximum of above values ( mx )**

For the case, that I am checking a subarray of size 1 all of these values are equal to the value of that element. When merging two subarrays (sub\_left, sub\_right) these values will be:

**sub\_left = max( sub\_left.s\_l, sub\_left.t + sub\_right.s\_l )**

**s\_r = max( sub\_right.s\_r, sub\_right.t + sub\_left.s\_r )**

**t = sum( sub\_left.t + sub\_right.t )**

**mx = max( s\_l, s\_r, t, sub\_right.mx, sub\_left.mx, sub\_left.r+sub\_right.l)**

#### \*\*Kadane’s algorithm\*\*

We can easily solve this problem in linear time using Kadane’s;s algorithm. The idea is to maintain maximum (positive sum) sub-array ending at each index of the given array. This subarray is either empty (in which case its sum is zero) or consists of one more element than the maximum subarray ending at the previous index.

**The time complexity of above solution is \*\*O(n)\*\* and auxiliary space used by the program is \*\*O(1)\*\***

Simple idea of the Kadane’s algorithm is to look for all positive contiguous segments of the array (max\_ending\_here is used for this). And keep track of maximum sum contiguous segment among all positive segments (max\_so\_far is used for this). Each time we get a positive sum compare it with max\_so\_far and update max\_so\_far if it is greater than max\_so\_far

Initialize: max\_so\_far = 0  
max\_ending\_here = 0

**Loop for each element of the array**

*   **max\_ending\_here = max\_ending\_here + a\[i\]**
*   **if(max\_ending\_here < 0) max\_ending\_here = 0**
*   **if(max\_so\_far < max\_ending\_here) max\_so\_far = max\_ending\_here**
*   **return max\_so\_far**

#### **\*My Implementation of Kadane’s approach\*\***

undefined
undefined

**Another implementation of Kadane’s Algo with O(n) linear complexity**

![](https://cdn-images-1.medium.com/max/800/1*mXjUuSDwbD2FcX_7gn734Q.png)
undefined

By [Rohan Paul](https://medium.com/@paulrohan) on [April 6, 2019](https://medium.com/p/1cf512a58ea9).

[Canonical link](https://medium.com/@paulrohan/maximum-sum-of-subarray-a-javascript-implementaion-1cf512a58ea9)

Exported from [Medium](https://medium.com) on December 12, 2020.