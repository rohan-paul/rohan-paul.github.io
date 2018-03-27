---
layout: post
title: Quick-Sort Algorithm in JavaScript
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/sorting-algo.jpeg" class="fit image">


If you’re interviewing for developer position, one of the more intimidating questions that can be asked is explaining how the **Quicksort** algorithm works.

**The simplest algorithmic steps for Quicksort is:**

1. Pick a pivot element that divides the list into two sublists. We can select a random element as the pivot.

2. Reorder the list so that all elements less than the pivot element are placed before (towards its left) the pivot and all elements greater than the pivot are placed after it (towards its right).

3. Repeat steps 1 and 2 on both the smaller and larger list. That is, Recursively apply the above steps to the sub-array of elements with smaller values and separately to the sub-array of elements with greater values.

Below is a basic implementation without usuing **swap function** and **partition function**.

<p data-height="985" data-theme-id="0" data-slug-hash="KZooPm" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="quick-sort-basic.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/KZooPm/">quick-sort-basic.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

There are many approaches to calculating the pivot value. While, some algorithms select the first item as a pivot, that’s not the best selection because it gives worst-case performance on already sorted arrays.

**Key features of Quick-sort**

- Its in the category of divide and conquer algorithms and in average cases it has a performance of **O(n log n)**, and worst case performance is **O(n^2)**

- It is not a stable sort algorithm, which means that the original order of the elements is not preserved. As an example this would mean that if you placed multiple 5’s in an array, there’s no way to guarantee that the 5’s will all be in the same order that you placed them in the array after it’s been sorted, as shown here.


**Now more traditional and elaborate implementation steps of Quick-sort:**


**Quick-sort under Hoare partition scheme**

**[The original partition scheme](https://en.wikipedia.org/wiki/Quicksort#Hoare_partition_scheme)** described by C.A.R. Hoare uses two indices that start at the ends of the array being partitioned, then move toward each other, until they detect an inversion: a pair of elements, one greater than or equal to the pivot, one lesser or equal, that are in the wrong order relative to each other. The inverted elements are then swapped. When the indices meet, the algorithm stops and returns the final index.

The indices i and j run towards each other until they cross, which always happens at pivot. This effectively divides the array into two parts: A left part which is scanned by i and a right part scanned by j. Now, a swap is done exactly for every pair of “misplaced” elements, i.e. a large element (larger than pivot, thus belonging in the right partition) which is currently located in the left part and a small element located in the right part.

**The steps**

- Call Quick sort, passing the array and left-pointer and right-pointer to the quickSort function. For the first call, left-pointer would be the index of the first element which is 0 and right-pointer would be the index of the last element which would be (length -1).

- Select Pivot, as the last index of the array. The key process in quickSort is ``partition()``. Target of partitions is, given an array and an element x of array as pivot, put x at its correct position in sorted array and put all smaller elements (smaller than x) before x, and put all greater elements (greater than x) after x. All this should be done in linear time.

- Swap function: A helper function to swap values of the array.

- Call Partition function: After calculating the pivot, we send the pivot to the ``partitionHoare()`` function. This function moves all the items smaller than the pivot value to the left and larger than pivot value to the right with the swap function. Then the function updates and returns the value of the left-pointer, which is indeed used as the partitionIndex.

- partitionIndex: In the ``partitionHoare()`` function, we keep moving all the items smaller than the pivot value to the left and larger than pivot value to the right. We have to keep track of the position of the partition. so that we can split the array into two parts in the next step. This tracking of the index where we partition the array is done by using partitionIndex variable. the initial value is left-pointer. And this initial value gets updated by the ``partitionHoare()`` function

- Inside the ``partitionHoare()`` function, we swap values for misplaced elements. That is, if an element is larger than the pivot position element, but is placed on the left side of the pivot, we swap it.

- Repeat the process: Now come back to quickSort function. when I get the updated partitionIndex, apply quickSort for the left side of the array and right side of the array. keep doing it until left is smaller than right.

- So, after the first 2 segments (segmented by pivot) are scanned with the ``partitionHoare()`` function, the next two segments that the main algorithm recurs on are [left...pivot - 1] and [pivot...right]

Below is the code under Hoare scheme
<p data-height="1448" data-theme-id="0" data-slug-hash="YYLyLP" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="quick-sort-Hoare.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/YYLyLP/">quick-sort-Hoare.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

**Quick-sort under Lomuto partition scheme**

This scheme chooses a pivot that is typically the last element in the array. The algorithm maintains index i as it scans the array using another index j such that the elements ``left`` through i (inclusive) are less than or equal to the pivot, and the elements i+1 through j-1 (inclusive) are greater than the pivot.

So, we start from the leftmost element and keep track of index of smaller (or equal to) elements as j. While traversing, if we find a smaller element, we swap current element with arr[j]. Otherwise we ignore current element.

Below is code under Lomuto scheme:
<p data-height="1211" data-theme-id="0" data-slug-hash="GydpGZ" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="quick-sort-Lomuto.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/GydpGZ/">quick-sort-Lomuto.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

**Performance & Memory Access Pattern**

Hoare's scheme is more efficient than Lomuto's partition scheme because it does three times fewer swaps on average, and it creates efficient partitions even when all values are equal. Like Lomuto's partition scheme, Hoare partitioning also causes Quicksort to degrade to O(n2) when the input array is already sorted; it also doesn't produce a stable sort. The algorithms behave very similar on random permutations

Both algorithms use two pointers into the array that scan it sequentially. Therefore both behave almost optimal w.r.t. caching.

On an array that is already sorted, Hoare's method never swaps, as there are no misplaced pairs, whereas Lomuto's method still does its roughly n/2 swaps.

While researching for this blog, found this beautiful site showing all [sorting algorithms with Animation](https://www.toptal.com/developers/sorting-algorithms/).