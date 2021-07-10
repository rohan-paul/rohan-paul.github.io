# Quick-Sort Algorithm in JavaScript

If you’re interviewing for Software Engineering position, one of the more intimidating questions to deal with is explaining how the…

Quick-Sort Algorithm in JavaScript

![](https://cdn-images-1.medium.com/max/2560/1*4h-p1NCFaAzQAMyAkNzmpg.jpeg)
undefined

If you’re interviewing for Software Engineering position, one of the more intimidating questions to deal with is explaining how the **Quicksort** algorithm works.

**The simplest algorithmic steps for Quicksort is:**

1.  Pick a pivot element that divides the list into two sublists. We can select a random element as the pivot.
2.  Reorder the list so that all elements less than the pivot element are placed before (towards its left) the pivot and all elements greater than the pivot are placed after it (towards its right).
3.  Repeat steps 1 and 2 on both the smaller and larger list. That is, Recursively apply the above steps to the sub-array of elements with smaller values and separately to the sub-array of elements with greater values.

Below is a basic implementation without usuing **swap function** and **partition function**.

undefined
undefined

There are many approaches to calculating the pivot value. While, some algorithms select the first item as a pivot, that’s not the best selection because it gives worst-case performance on already sorted arrays.

**Key features of Quick-sort**

*   Its in the category of divide and conquer algorithms and in average cases it has a performance of **O(n log n)**, and worst case performance is **O(n²)**
*   It is not a stable sort algorithm, which means that the original order of the elements is not preserved. As an example this would mean that if you placed multiple 5’s in an array, there’s no way to guarantee that the 5’s will all be in the same order that you placed them in the array after it’s been sorted, as shown here.

**Now more traditional and elaborate implementation steps of Quick-sort:**

#### **Quick-sort under Hoare partition scheme**

[**The original partition scheme**](https://en.wikipedia.org/wiki/Quicksort#Hoare_partition_scheme) described by C.A.R. Hoare uses two indices that start at the ends of the array being partitioned, then move toward each other, until they detect an inversion: a pair of elements, one greater than or equal to the pivot, one lesser or equal, that are in the wrong order relative to each other. The inverted elements are then swapped. When the indices meet, the algorithm stops and returns the final index.

The indices i and j run towards each other until they cross, which always happens at pivot. This effectively divides the array into two parts: A left part which is scanned by i and a right part scanned by j. Now, a swap is done exactly for every pair of “misplaced” elements, i.e. a large element (larger than pivot, thus belonging in the right partition) which is currently located in the left part and a small element located in the right part.

**The steps**

*   Call Quick sort, passing the array and left-pointer and right-pointer to the quickSort function. For the first call, left-pointer would be the index of the first element which is 0 and right-pointer would be the index of the last element which would be (length -1).
*   Select Pivot, as the last index of the array. The key process in quickSort is `partition()`. Target of partitions is, given an array and an element x of array as pivot, put x at its correct position in sorted array and put all smaller elements (smaller than x) before x, and put all greater elements (greater than x) after x. All this should be done in linear time.
*   Swap function: A helper function to swap values of the array.
*   Call Partition function: After calculating the pivot, we send the pivot to the `partitionHoare()` function. This function moves all the items smaller than the pivot value to the left and larger than pivot value to the right with the swap function. Then the function updates and returns the value of the left-pointer, which is indeed used as the partitionIndex.
*   partitionIndex: In the `partitionHoare()` function, we keep moving all the items smaller than the pivot value to the left and larger than pivot value to the right. We have to keep track of the position of the partition. so that we can split the array into two parts in the next step. This tracking of the index where we partition the array is done by using partitionIndex variable. the initial value is left-pointer. And this initial value gets updated by the `partitionHoare()` function
*   Inside the `partitionHoare()` function, we swap values for misplaced elements. That is, if an element is larger than the pivot position element, but is placed on the left side of the pivot, we swap it.
*   Repeat the process: Now come back to quickSort function. when I get the updated partitionIndex, apply quickSort for the left side of the array and right side of the array. keep doing it until left is smaller than right.
*   So, after the first 2 segments (segmented by pivot) are scanned with the `partitionHoare()` function, the next two segments that the main algorithm recurs on are \[left…pivot - 1\] and \[pivot…right\]

Below is the code under Hoare scheme

undefined
undefined

**Quick-sort under Lomuto partition scheme**

This scheme chooses a pivot that is typically the last element in the array. The algorithm maintains index i as it scans the array using another index j such that the elements `left` through i (inclusive) are less than or equal to the pivot, and the elements i+1 through j-1 (inclusive) are greater than the pivot.

So, we start from the leftmost element and keep track of index of smaller (or equal to) elements as j. While traversing, if we find a smaller element, we swap current element with arr\[j\]. Otherwise we ignore current element.

Below is code under Lomuto scheme:

undefined
undefined

**On Performance — between the two schemes — Hoare’s scheme is more efficient than Lomuto’s partition**

Hoare’s scheme is more efficient than Lomuto’s partition scheme because it does three times fewer swaps on average, and it creates efficient partitions even when all values are equal. Like Lomuto’s partition scheme, Hoare partitioning also causes Quicksort to degrade to O(n2) when the input array is already sorted; it also doesn’t produce a stable sort. The algorithms behave very similar on random permutations

Both algorithms use two pointers into the array that scan it sequentially. Therefore both behave almost optimal w.r.t. caching.

On an array that is already sorted, Hoare’s method never swaps, as there are no misplaced pairs, whereas Lomuto’s method still does its roughly n/2 swaps.

**Quick Sort — General Performance & Memory Access Pattern**

Just like in merge sort, for a given recursive call the time on an n-element subarray is Θ(n). In the case of merge sort, that was the time for merging, while in the case of quicksort it’s the time for partitioning.

#### Worst-case running time

When quicksort has the most unbalanced partitions possible, then the original call takes _cn_ time for some constant c, the recursive call on **_n−1_** elements takes **_c(n-1)_** time, the recursive call on **_n−2_** elements takes **_c(n-2)_** time, and so on.

_cn_+_c_(_n_−1)+_c_(_n_−2)+...+2_c_​=_c_(_n_+(_n_−1)+(_n_−2)+...+2)=_c_((_n_+1)(_n_/2)−1) 

**In big-Θ notation, quicksort’s worst-case running time is Θ(n²).**

#### Best-case running time

Quicksort’s best case occurs when the partitions are as evenly balanced as possible: their sizes either are equal or are within 1 of each other.

The former case occurs if the subarray has an odd number of elements and the pivot is right in the middle after partitioning, and each partition has **_(n-1)/2_** elements. And the second case occurs if the subarray has an even number of elements and one partition has **_n/2_** elements with the other having **_n/2 –1_** elements . In either of these cases, each partition has at most **_n/2_** elements

**And the best case performance is Θ(_n_log​_n_)**

**Therefore:** **_best case_** performance of **Quick Sort**

**T(n) = partition(n) + 2\*T(n/2)       // partition(n) = n          
         = n + 2\*T(n/2)  
	 = 2\*T(n/2) + n**

**_Solving for the recurrence relation:_**

**T(n) = 2\*T(n/2) + n              // T(n/2) = 2\*T(n/4) + (n/2)      
  
        = 2\*\[ 2\*T(n/4) + n/2 \] + n  
	= 22\*T(n/4) + n + n  
	= 22\*T(n/4) + 2n             // T(n/4) = 2\*T(n/8) + (n/4)  
  
	= 22\*\[ 2\*T(n/8) + (n/4) \] + 2n  
	= 23\*T(n/8) + 22\*(n/4) + 2n  
	= 23\*T(n/8) + n + 2n  
	= 23\*T(n/8) + 3n  
  
	= 24\*T(n/16) + 4n  
	and so on....  
  
	= 2k\*T(n/(2k)) + k\*n     
// Keep going until: n/(2k) = 1  <==> n = 2k      
  
	= 2k\*T(1) + k\*n  
	= 2k\*1 + k\*n  
	= 2k + k\*n               // n = 2k  
	= n + k\*n  
	= n + (lg(n))\*n  
        = n\*( lg(n) + 1 )  
       ~= n\*lg(n))**

Also, while researching for this blog, found this beautiful site showing all [sorting algorithms with Animation](https://www.toptal.com/developers/sorting-algorithms/).

By [Rohan Paul](https://medium.com/@paulrohan) on [September 27, 2019](https://medium.com/p/5cf5ab7d251b).

[Canonical link](https://medium.com/@paulrohan/quick-sort-algorithm-in-javascript-5cf5ab7d251b)

Exported from [Medium](https://medium.com) on December 12, 2020.