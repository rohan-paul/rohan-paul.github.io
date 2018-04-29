---
layout: post
title: Implemetning Heap Algorithm to Find Permutation of a set of Numbers
comments: true
author: Rohan Paul
categories: JavaScript
use_math : false
---
<img src="/images/fulls/Heap-algo-Permutation.jpg" class="fit image">


**First lets understand the formulae for Permutation**

Let us assume that there are r boxes and each of them can hold one thing. There will be as many permutations as there are ways of filling in r vacant boxes by n objects.

No. of ways first box can be filled: n

No. of ways second box can be filled: (n – 1)

No. of ways third box can be filled: (n – 2)

No. of ways fourth box can be filled: (n – 3)

No. of ways rth box can be filled: (n – (r – 1))

Therefore, no. of ways of filling in r boxes in succession can be given by:

n * (n – 1) * (n – 2) * (n-3) * . . . * (n – (r – 1))

which matahematically simplifies to n! / (n - r)!

In our below problem, I am trying to find the permutation of n elements of an array taking all the n at a time, so the result is **!n**

There are many ways to solve, but per [Wikipedia article on Heap's Algorithm](https://en.wikipedia.org/wiki/Heap's_algorithm) : "In a 1977 review of permutation-generating algorithms, Robert Sedgewick concluded that it was at that time the most effective algorithm for generating permutations by computer". The algorithm is a pretty famous one but even after some try, for me it remained non-intutive. So here, in this post, I am only trying to document my implementation step. Feel free to go through the original [paper](https://academic.oup.com/comjnl/article/6/3/293/360213)



**The pseudo-code that I am looking at is below as given in the [wikipedia page](https://en.wikipedia.org/wiki/Heap's_algorithm)**

```

procedure generate(n : integer, A : array of any):
    if n = 1 then
          output(A)
    else
        for i := 0; i < n - 1; i += 1 do
            generate(n - 1, A)
            if n is even then
                swap(A[i], A[n-1])
            else
                swap(A[0], A[n-1])
            end if
        end for
        generate(n - 1, A)
    end if

```

**The JavaScript implementing the above**

<p data-height="599" data-theme-id="0" data-slug-hash="LmRreK" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="Permutation-Heap-Blog.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/LmRreK/">Permutation-Heap-Blog.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


**Time Complexity  - runs in factorial time O`(n!`)**

Keep in mind, there are n! number of permutations for a set of n objects.  So for a string of three letters there are (3 * 2 * 1) or 6 unique permutations.

Consequently, Heap’s algorithm works on the order of O`(n!`), the slowest order of functions.  Therefore, as the set gets larger, increases of even one number will cause the algorithm to slow drastically.  To put that into perspective, a set with 15 elements, will have over 1.3 Trillion permutations.


Now running the function with a simple array..

```
permutationHeap(['a', 'b', 'c'], output);

// will output

[ 'a', 'b', 'c' ]
[ 'b', 'a', 'c' ]
[ 'c', 'b', 'a' ]
[ 'b', 'c', 'a' ]
[ 'c', 'a', 'b' ]
[ 'a', 'c', 'b' ]

```

**How Heap's Algo is working**

Heap’s algorithm fixes the element in the last position and generates all permutations for the rest of the elements in place. In other words, it generates (n-1)! permutations of the first n-1 elements, adjoining the last element to each of these. This will generate all of the permutations that end with the last element. In each iteration, the algorithm will produce all the permutations that end with the current last element.

After that, the algorithm swaps the element in the last position with one of the rest and repeats the process recrusively.

After every swap involving position n, a different element is placed in position n

Notice though that the only time the elements of the array are referenced is in the call to the swap function.

Lets take the example sequence ["a", "b", "c"] :


First Swap generates the other permutation, i.e. other thatn the given sequence in the argument of the function where c is in the last position.

Second Swap moves a to the last position to generate one permutation with a in the last position and the next swap, swap 3 generates the other permutation. 

Swap 4 moves b to the last position to generate one permutation with b in the last position and swap 5 constructs the other. 

By moving each element to the last position and generating the remaining permutations, Heap’s algorithm adjoins each element to all permutations of the other two elements.

So, the key technique of Heap's algo is the clever way to keep track of which elements we had already removed? Then we could just swap out unremoved elements, so that it is different in each case. The way it works is:

 A> If n is odd, then swap the ith element, where i is the counter starting from 0 and the last element and repeat the algorithm till i is less than or equal to n.

 B> And if n is even, swap the first and last element.

 **How the stack works to build up the Permutation**

  We start by defining i and assigning the value 0 to it. We continue to check if it satisfies the condition of the for loop. If it does we step into the loop and execute the first line inside the loop which is the recursive call `permutationHeap(array, result, n-1)` .

  Inside the recursive call we have a new stack frame which has no knowledge of the i variable we defined before executing the recursive call, because it is a local variable. So when we get to the loop in the new call we define a new variable i, assigning it 1, (as I am making it to be 1-indexed array) at first and incrementing it as the loop repeats in this stack frame/call instance. When this call finishes we delete the stack frame and resume to the previous stack frame (the one we started with) where i=1 still, and we continue to the next line. 

  All the calls have access to the array since the function is defined in the same scope as the variables `(inside the function permutationHeap)` so within each call - no matter what the stack frame we are in, the changes made to those are made to the same instances.

