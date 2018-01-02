---
layout: post
title: Algorithm-in-JavaScript-Hash Table
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/Hash-Table-in-JS.jpeg" class="fit image">

A **hash table** (also called a hash, hash map, map, unordered map or dictionary) is a data structure that pairs keys to values. Its a data structure thats used to implement an associative array, a structure that can map keys to values. A Hash Table uses a **hash function** to compute an index into an array of buckets or slots, from which the desired value can be found. From [Wikipedia](https://en.wikipedia.org/wiki/Hash_table).

Although hash tables provide fast insertion, deletion, and retrieval, they perform poorly for operations that involve searching, such as finding the minimum and maximum
values in a data set. For these operations, other data structures such as the binary search tree are more appropriate.

**Complexity** [Source](http://bigocheatsheet.com/)

| | _Average_ | _Worst_ | 
Access | O(1) | O(n)
Insertion | O(1) | O(1)
Deletion | O(1) | O(n)


**Hash Function**

The Hash-function takes a key and converts it to a number which will be the index at which to store it. Output of the hash function is an array of key-value pairs, storing each pair at a numeric index. In my ``hash()`` function below, I am computing a hash value by summing the ASCII value of each string (the argument passed-in) using the JavaScript function ``charCodeAt()`` to return a character’s ASCII value after multiplying the ASCII code by a multiplier H, which in this case, is an odd prime 37. And the reason to choose 37 being, by some empirical research, if we take over 50,000 English words (formed as the union of the word lists provided in two variants of Unix), using the constants 31, 33, 37, 39, and 41 will produce less than 7 collisions in each case, while creating a hasing function.

So, effectively, the hash function changes the passed in argument to a number. so that they can be used as array index. So, if I want to save my name "Paul", just convert this name to integer first. In a super simplied form it will work as below...

```

To insert "Paul":

var k = hashFunc("Paul");

arr[k] = "Paul" // this takes O(1) time


Lookup for Paul:-

var k = hashFunc("Paul") ;

var name = arr[k] ;

console.log(name);  //prints alice 

```

Hash table is like a bucket, represented as an empty array initially. When initializing the hash table we create an array containing a fixed number of these buckets.

```
var allBuckets = [[], [], [], []] 

In order to insert a value inside our buckets, i.e. insert a key “x” with the value 10

1. Use has function to get bucket index
3. Push key-value pair into bucket 

```




**Collision-Resolution**

Even with an efficient hash function, it is possible for two keys to hash (the result of the hash function) to the same value. This is called a collision, and we need a strategy for handling collisions when they occur. In my implementation below, this is taken care of by the ``put()`` function. The ``put()`` function receives the array index value from the ``hash()`` function and stores the ``data`` element in that position. But only after checking that after the k``key`` value is hased (with the ``hash()`` function) there's no collison. And this can be done in two ways - **separate chaining** and **linear probing** (both have been implemented in the below code).

**A generic HashTable Implementation**

<p data-height="2208" data-theme-id="0" data-slug-hash="PEjwEW" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="Generic-Hash_Implementation.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/PEjwEW/">Generic-Hash_Implementation.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


**Some very simple application of HashTable in solving common problems**
<p data-height="982" data-theme-id="0" data-slug-hash="aEWerb" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="aEWerb" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/aEWerb/">aEWerb</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<p data-height="426" data-theme-id="0" data-slug-hash="ZvyGQd" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="hash_implement_removeDuplicate.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/ZvyGQd/">hash_implement_removeDuplicate.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


<p data-height="1500" data-theme-id="0" data-slug-hash="baRRyp" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="google-interview_bizTrip.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/baRRyp/">google-interview_bizTrip.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

**Measuring performance**

Lets design a test that generates 100,000 keys and values, then measures how long it takes to insert (``put()``) and then read (``get()``) them from the hash table.
I took the ``makeid`` function from [StackOverflow](https://stackoverflow.com/a/1349426/1902852) that would generate 100,000 character string composed of characters picked randomly from the set [a-zA-Z0-9].
I use ``console.time`` and ``console.timeEnd`` to measure how fast our hash table is.


<p data-height="694" data-theme-id="0" data-slug-hash="eyRRqy" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="hashTable_performance.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/eyRRqy/">hashTable_performance.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

And it looks like the **Linear Probing technique** runs much faster.

**The great confusion about Hash vs Objects in JavaScript**

I would opine, every object in JavaScript IS a hash. This is a hash of object's properties and methods. Every time I call object's method, property, or just reference any variable, I perform an internal hash lookup.

**Why I need a hashtable instead of just a non-hashed associative array, which definitely will be simpler to implement**

When using a hashtable, I compute the hash code of a given key to find the associated value. The hashcode is an index in the underlying array of the hashtable. This means that finding a key in a hashtable is as fast as accessing an array by index. So, while searching, the hash table mechanism gives me O(1) performance for any given key search. Accessing an array with an index (e.g. myArray[0] ) is instant; it doesn't require any searching, because the runtime of the languages knows exactly where to find this value.

 
And if I do not use a HashTable, I still need some sort of search mechanism. And if I choose, Binary Search Tree, it's  O(log n) not O(1). And if I don't have a search mechanism at all (i.e. I am searching the entire array from top to bottom to find my key), my performance is O(n).