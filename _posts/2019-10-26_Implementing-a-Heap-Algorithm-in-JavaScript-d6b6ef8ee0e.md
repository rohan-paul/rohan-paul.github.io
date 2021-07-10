# Implementing a Heap Algorithm in JavaScript

Implementing a Heap Algorithm in JavaScript to find permutation of a set of numbers

Implementing a Heap Algorithm in JavaScript to find permutation of a set of numbers

![](https://cdn-images-1.medium.com/max/1200/1*RNkSbvFBFBWFmfPtQkmOUQ.jpeg)
undefined

**First lets understand the formulae for Permutation**

Let us assume that there are r boxes and each of them can hold one thing. There will be as many permutations as there are ways of filling in r vacant boxes by n objects.

No. of ways first box can be filled: n

No. of ways second box can be filled: (n — 1)

No. of ways third box can be filled: (n — 2)

No. of ways fourth box can be filled: (n — 3)

No. of ways rth box can be filled: (n — (r — 1))

Therefore, no. of ways of filling in r boxes in succession can be given by:

n \* (n — 1) \* (n — 2) \* (n-3) \* . . . \* (n — (r — 1))

which matahematically simplifies to n! / (n — r)!

#### Why we need Heap’s Algorithm at all

To arrive at the permutation, a basic strategy would be to pick an element, then pick from those remaining, and so on until a single permutation is achieved. Then just backtrack one step and change your selection, finish the permutation again, and so on. For example, if you want to permute ABCD :

Select A  
Select B, yielding AB  
Select C, then D, yielding ABCD —

This will be the first permutation.

Now backtrack two positions and select D, then C, yielding ABDC  
Then again, backtrack three positions and select C, then B, then D, yeilding ACBD  
And so on…  
This can be handled with recursion by running collection of element lists (several copies) — however this will be huge memory and CPU intensive, so an in-place solution would be nice, which is what the Heap Algo does. It is small, efficient, and elegant and brilliantly simple in concept.

In our below problem, I am trying to find the permutation of n elements of an array taking all the n at a time, so the result is **!n**

There are many ways to solve, but per [Wikipedia article on Heap’s Algorithm](https://en.wikipedia.org/wiki/Heap%27s_algorithm) : “In a 1977 review of permutation-generating algorithms, Robert Sedgewick concluded that it was at that time the most effective algorithm for generating permutations by computer”. The algorithm is a pretty famous one but even after some try, for me it remained non-intutive. So here, in this post, I am only trying to document my implementation step. Feel free to go through the original [paper](https://academic.oup.com/comjnl/article/6/3/293/360213)

The pseudo-code that I am looking at is below as given in the [**wikipedia page**](https://en.wikipedia.org/wiki/Heap%27s_algorithm)

```
procedure generate(n : integer, A : array of any):    if n = 1 then          output(A)    else        for i := 0; i < n - 1; i += 1 do            generate(n - 1, A)            if n is even then                swap(A[i], A[n-1])            else                swap(A[0], A[n-1])            end if        end for        generate(n - 1, A)    end if
```

It It divides the problem into two parts: a sub-problem of size `(n -1)` and a single remaining element. It then solves the sub-problem of size `(n-1)` by a recursive call (or an iterative decreasing process).

It works with the following steps. For given input sequence 12…n do the following.

1\. Permute the elements in positions 1 to n−1 by applying this algorithm to those elements.

2\. Apply the following steps n−1 times. Increment counter i=1 after each iteration.

3\. Swap element in position n with one of the other elements in the following way.

A. If n is odd, swap elements in positions 1 and n. For odd-length permutations, the n-1 swap is always done with the first element.

B. If n is even, swap elements in position i and n.  For even-length permutations, the n-1 swap is done with the i-th element, where i is a counter from position 0 to n-2 that increments every time we do this.

**The JavaScript implementing the above**

undefined
undefined

**Time Complexity — runs in factorial time O**`**(n!**`**)**

Keep in mind, there are n! number of permutations for a set of n objects. So for a string of three letters there are (3 \* 2 \* 1) or 6 unique permutations.

Consequently, Heap’s algorithm works on the order of O`(n!`), the slowest order of functions. Therefore, as the set gets larger, increases of even one number will cause the algorithm to slow drastically. To put that into perspective, a set with 15 elements, will have over 1.3 Trillion permutations.

**How Heap’s Algo is working**

Heap’s algorithm fixes the element in the last position and generates all permutations for the rest of the elements in place. In other words, it generates (n-1)! permutations of the first n-1 elements, adjoining the last element to each of these. This will generate all of the permutations that end with the last element. In each iteration, the algorithm will produce all the permutations that end with the current last element.

After that, the algorithm swaps the element in the last position with one of the rest and repeats the process recrusively.

After every swap involving position n, a different element is placed in position n

Notice though that the only time the elements of the array are referenced is in the call to the swap function.

Lets take the example sequence \[“a”, “b”, “c”\] :

First Swap generates the other permutation, i.e. other thatn the given sequence in the argument of the function where c is in the last position.

Second Swap moves **a** to the last position to generate one permutation with a in the last position and the next swap, swap 3 generates the other permutation.

Swap 4 moves b to the last position to generate one permutation with b in the last position and swap 5 constructs the other.

By moving each element to the last position and generating the remaining permutations, Heap’s algorithm adjoins each element to all permutations of the other two elements.

So, the key technique of Heap’s algo is the clever way to keep track of which elements we had already removed? Then we could just swap out unremoved elements, so that it is different in each case. The way it works is:

A> If n is odd, then swap the ith element, where i is the counter starting from 0 and the last element and repeat the algorithm till i is less than or equal to n.

B> And if n is even, swap the first and last element.

**How the stack works to build up the Permutation**

We start by defining i and assigning the value 0 to it. We continue to check if it satisfies the condition of the for loop. If it does we step into the loop and execute the first line inside the loop which is the recursive call `permutationHeap(array, result, n-1)` .

Inside the recursive call we have a new stack frame which has no knowledge of the i variable we defined before executing the recursive call, because it is a local variable. So when we get to the loop in the new call we define a new variable i, assigning it 1, (as I am making it to be 1-indexed array) at first and incrementing it as the loop repeats in this stack frame/call instance. When this call finishes we delete the stack frame and resume to the previous stack frame (the one we started with) where i=1 still, and we continue to the next line.

All the calls have access to the array since the function is defined in the same scope as the variables `(inside the function permutationHeap)` so within each call - no matter what the stack frame we are in, the changes made to those are made to the same instances.

By [Rohan Paul](https://medium.com/@paulrohan) on [October 26, 2019](https://medium.com/p/d6b6ef8ee0e).

[Canonical link](https://medium.com/@paulrohan/implemetning-heap-algorithm-to-find-permutation-of-a-set-of-numbers-in-javascript-d6b6ef8ee0e)

Exported from [Medium](https://medium.com) on December 12, 2020.