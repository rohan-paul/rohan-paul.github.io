# Python — List vs Tuple vs Dictionary

Among the basic data types and structures that Python provides are the following:

![](https://cdn-images-1.medium.com/max/1200/1*PxZmvk-Fdlhykr6cwBrC9w.jpeg)

Among the basic data types and structures that Python provides are the following:

*   Logical: `bool`
*   Numeric: `int`, `float`, `complex`
*   Sequence: `list`, `tuple`, `range`
*   Text Sequence: `str`
*   Binary Sequence: `bytes`, `bytearray`, `memoryview`
*   Map: `dict`
*   Set: `set`, `frozenset`

Out of the above, the four basic inbuilt data structures namely **Lists, Dictionary, Tuple and Set** cover almost 80% of the our real world data structures.

#### Lists

Lists is one of the most versatile collection object types available in Python. (dictionaries and tuples being the other two, which in a way, more like variations of lists).

*   A list is a mutable, ordered sequence of items. As such, it can be indexed, sliced, and changed. Each element can be accessed using its position in the list.Python lists do the work of most of the collection data structures found in other languages and since they are built-in, you don’t have to worry about manually creating them.
*   Lists can be used for any type of object, from numbers and strings to more lists.
*   They are accessed just like strings (e.g. slicing and concatenation) so they are simple to use and they’re variable length, i.e. they grow and shrink automatically as they’re used.
*   List variables are declared by using brackets `[ ]` following the variable name.

```
A = [ ] # This is a blank list variableB = [1, 23, 45, 67] # this list creates an initial list of 4 numbers.C = [2, 4, 'john'] # lists can contain different variable types.
```

#### Some useful methods of Lists

**Slicing a List**

In List, we can take portions (including the lower but not the upper limit). List slicing is the method of splitting a subset of a list, and the indices of the list objects are also used for this. e.g. if my List name is **_products_**

**From the first element up to a given position:** products\[:3\]  
**From a given position until the last element:** products\[2:\]  
**Between two given positions in the list**: products\[2:4\]  
**all the members of the list:** products\[:\]

Python lists are upper-bound exclusive, and this means that the last index during list slicing is usually ignored. That is why, for a list \[3, 22, 30, 5.3, 20\]

list\[2:-1\] = \[30, 5.3\]

members of the list from index 2, which is the third element, to the second to the last element in the list, which is 5.3).

So here the summary of the slicing notation

```
a[start:stop]  # items start through stop-1a[start:]      # items start through the rest of the arraya[:stop]       # items from the beginning through stop-1a[:]           # a copy of the whole array
```

#### Deleting list elements

Python has 3 methods for deleting list elements: **list.remove(), list.pop(), and del operator.** **remove method** takes the particular element to be removed as an argument, and **deletes the first occurrence** of number mentioned in its arguments.

And **pop and del** take the index of the element to be removed as an argument. **del\[a : b\]** :- d **deletes all the elements in range** starting from index ‘a’ till ‘b’ mentioned in arguments. For example:

list = \[3, 5, 7, 8, 9, 20\]

To delete 3 (the 1st element) from the list, you could use:

*   list.remove(3) or
*   list.pop(0), or
*   del list\[0\]

To delete 8, the item at index 3, from the list, you could use:

*   list.remove(8), or
*   list.pop\[3\]

**extend(b)** :- This function is used to **extend the list with** the elements present in **another list**. This function takes **another list as its argument**.

**clear()** :- This function is used to **erase all the elements** of list. After this operation, list becomes empty.

#### Difference between Lists and Arrays in Python

The list elements can be anything and each list element can have a completely different type. This is not allowed in arrays. Arrays are objects with definite type and size, hence making the use of Lists extremely flexible. And arrays are stored more efficiently (i.e. as contiguous blocks of memory vs. pointers to Python objects).

The **‘array’** data structure in core python is not very efficient or reliable. Therefore, when we talk about python arrays, we usually mean python Lists.

However, python does provide [Numpy Arrays](https://www.edureka.co/blog/python-numpy-tutorial/) which are a grid of values used in [Data Science](https://www.edureka.co/blog/what-is-data-science/)[.](https://www.edureka.co/blog/python-numpy-tutorial/) You can look into [Numpy Arrays vs Lists](https://www.edureka.co/blog/python-numpy-tutorial/#NumpyVsList) to know more.

Another difference between a list and an array is the functions that you can perform to them. For example, you can divide an array by 3, and each number in the array will be divided by 3 as below.

However, if I try to divide a list by 3, Python will throw error.

x = array(\[3, 6, 9, 12\])  
x/3.0  
print(x)

Output  
\# TypeError: unsupported operand type(s) for /: 'list' and 'float'

Note above, how it takes an extra step to use arrays because they have to be declared explicitly, while lists don’t because they are part of Python’s syntax. However, if we’re going to perform arithmetic functions to our lists, weshould really be using arrays instead. Additionally, arrays will store your data more compactly and efficiently, so if you’re storing a large amount of data, you may consider using arrays as well.

**When should I use Array instead of Lists**

For almost all cases the normal list is the right choice. Arrays are more efficient than lists for some uses. If you need to allocate an array that you KNOW will not change, then arrays can be faster and use less memory. If you’re going to be using arrays, consider the numpy or scipy packages, which give you arrays with a lot more flexibility.

You can cast a list to a numpy array as below.

import numpy as np   
  
u=np.array(\[1,0\])  
  
v=np.array(\[0,1\])

### Tuple

Tuples are used to hold together multiple objects. Think of them as similar to lists, but without the extensive functionality that the list class gives you. One major feature of tuples is that they are _immutable_ like strings i.e. you cannot modify tuples. However, you can take portions of existing tuples to make new tuples. list are declared in square brackets and can be changed while **tuple is declared in parentheses**

**Tuple indexing and splitting**

The indexing and slicing in tuple are similar to lists. The indexing in the tuple starts from 0 and goes to length(tuple) — 1.

The items in the tuple can be accessed by using the slice operator. Python also allows us to use the colon operator to access multiple items in the tuple.

Unlike lists, the tuple items can not be deleted by using the **del** keyword becasuse tuples being immutable. To delete an entire tuple, we can use the **del** keyword with the tuple name.

tupleA = (1, 2, 3, 4)

print(tupleA)

del tupleA\[0\]

print(tupleA)

del tupleA

print(tupleA)

Output of the above is

(1, 2, 3, 4)

Traceback (most recent call last):

File "/home/paul/codeLap/BLOG/Python-testing-code-snippets/test2.py", line 3, in <module>

del tupleA\[0\]

TypeError: 'tuple' object doesn't support item deletion

#### Difference between lists and tuples

List has mutable nature i.e., list can be changed or modified after its creation according to needs whereas tuple has immutable nature i.e., tuple can’t be changed or modified after its creation.

list\_num = \[1,2,3,4\]  
tup\_num = (1,2,3,4)

print(list\_num)  
print(tup\_num)

Output:

\[1,2,3,4\]  
(1,2,3,4)

list\_num\[2\] = 5

print(list\_num)

tup\_num\[2\] = 5

Output:  
\[1,2,5,4\]

Traceback (most recent call last):  
File "python", line 6, in <module>  
TypeError: 'tuple' object does not support item assignment

Also, you can’t use a list as a key for a dictionary. This is because only immutable values can be hashed. Hence, we can only set immutable values like tuples as keys. But if you still want to use a list as a key, you must turn it into a tuple first. Because some tuples can be used as dictionary keys (specifically, tuples that contain immutable values like strings, numbers, and other tuples). Lists can never be used as dictionary keys, because lists are not immutable.

A very fine explanation of Tuples vs List is given in the [**StackOverflow ans**](https://stackoverflow.com/a/626871/1902852)

Apart from tuples being immutable there is also a semantic distinction that should guide their usage. Tuples are heterogeneous data structures (i.e., their entries have different meanings), while lists are homogeneous sequences. Tuples have structure, lists have order.

One example would be pairs of page and line number to reference locations in a book, e.g.:

my\_location = (42, 11)  # page number, line number

You can then use this as a key in a dictionary to store notes on locations. A list on the other hand could be used to store multiple locations. Naturally one might want to add or remove locations from the list, so it makes sense that lists are mutable. On the other hand it doesn't make sense to add or remove items from an existing location - hence tuples are immutable.

#### Among Tuples and List — when to use which

Use a tuple when you know what information goes in the container that it is. For example, when you want to store a person’s credentials for your website.

person=(‘ABC’,’admin’,’12345')

But when you want to store similar elements, like in an array in C++, you should use a list.

groceries=\[‘bread’,’butter’,’cheese’\]

### Dictionary

A dictionary is a **key:value** pair, similar to an associative array found in other programming languages. A dictionary is like an address-book where you can find the address or contact details of a person by knowing only his/her name i.e. we associate _keys_ (name) with _values_ (details). Note that the key must be unique just like you cannot find out the correct information if you have two persons with the exact same name.

#### Iterating a Dictionary

The Python dictionary allows the programmer to iterate over its keys using a simple **for loop**. Let’s take a look:

my\_dict = {1: 'one', 2: 'two', 3: 'three'}  
for key in my\_dict:  
   print(key)

Output  
1  
2  
3

Python 3 changed things up a bit when it comes to dictionaries. In Python 2, you could call the dictionary’s keys() and values() methods to return Python lists of keys and values respectively:

But in Python 3, you will get views returned:

\# Python 3

my\_dict = {1: 'one', 2: 'two', 3: 'three'}

my\_dict.keys() 

\# dict\_keys(\[1, 2, 3\])

my\_dict.values()

\# dict\_values(\['one', 'two', 'three'\])

\# my\_dict.items()

dict\_items(\[(1, 'one'), (2, 'two'), (3, 'three')\])

A view object has some similarities to the range object we saw earlier — it is a lazy promise, to deliver its elements when they’re needed by the rest of the program. We can iterate over the view, or turn the view into a list like this:

my\_dict = {1: 'one', 2: 'two', 3: 'three'}

print(list(my\_dict.keys()))

#\[1, 2, 3\]

The values method is similar; it returns a view object which can be turned into a list:

my\_dict = {1: 'one', 2: 'two', 3: 'three'}

print(list(my\_dict.values()))

\# \['one', 'two', 'three'\]

Alternatively, to get list of keys as an array from dict in Python 3, You can use [\* operator](https://docs.python.org/3/tutorial/controlflow.html#unpacking-argument-lists) to unpack dict\_values:

my\_dict = {1: 'one', 2: 'two', 3: 'three'}

print(\[\*my\_dict.keys()\])

\# \[1, 2, 3\]

my\_dict = {1: 'one', 2: 'two', 3: 'three'}

print(\[\*my\_dict.values()\])

\# \['one', 'two', 'three'\]

The items method also returns a view, which promises a list of tuples — one tuple for each key:value pair:

#### Restrictions on Dictionary Keys

Almost any type of value can be used as a dictionary key in Python, e.g. integer, float, and Boolean objects. However, there are a couple restrictions that dictionary keys must abide by.

First, a given key can appear in a dictionary only once. Duplicate keys are not allowed.

Secondly, a dictionary key must be of a type that is immutable. A tuple can also be a dictionary key, because tuples are immutable:

d = {(1, 1): 'a', (1, 2): 'b', (2, 1): 'c', (2, 2): 'd'}  
d\[(1,1)\]  
'a'  
d\[(2,1)\]  
'c'

However, neither a list nor another dictionary can serve as a dictionary key, because lists and dictionaries are mutable:

#### Restrictions on Dictionary Values

By contrast, there are no restrictions on dictionary values. Literally none at all. A dictionary value can be any type of object Python supports, including mutable types like lists and dictionaries, and user-defined objects, which you will learn about in upcoming tutorials.

There is also no restriction against a particular value appearing in a dictionary multiple times:

#### To check if a key exists in a Python dictionary

there is often a need to extract the value of a given key from a dictionary; however, it is not always guaranteed that a specific key exists in the dictionary.

> _When you index a dictionary with a key that does not exist, it will throw an error._

Hence, it is a safe practice to check whether or not the key exists in the dictionary prior to extracting the value of that key. For that purpose, Python offers two built-in functions:

*   `has_key()`
*   `if`\-`in` statement

However the `has_key` method has been removed from Python3.

The code snippet below illustrates the usage of the `if`\-`in` statement to check for the existence of a key in a dictionary:

Stocks = {'a': "Apple", 'b':"Microsoft", 'c':"Google"}

key\_to\_lookup = 'a'

if key\_to\_lookup in Stocks:

print "Key exists"

else:

print "Key does not exist"

**So, how does Python find out whether the value is or is not in the dictionary without looping through the dictionary somehow?**

Becuase, dictionaries in Python are implemented as hash tables under the hood, which allow you to look up whether an item exists in the table directly in constant run time instead of having to go through every element one at a time to check membership like you would with a normal list or array. Basically, the keys or index values for a dictionary are passed through a complicated “hash function” which assigns them to unique buckets in memory. A given input value for the key or index will always access the same bucket when passed through the hash function, so instead of having to check every value to determine membership, you only have to pass the index through the hash function and then check if anything exists at the bucket associated with it. The drawback of dictionaries is that they are not ordered and require more memory than lists.

#### Aliasing and copying of Dictionaries

Because dictionaries are mutable, we need to be aware of aliasing. Whenever two variables refer to the same object, changes to one affect the other.

If we want to modify a dictionary and keep a copy of the original, use the copy method. For example, in the below original\_dict is a dictionary that contains pairs of original\_dict:

original\_dict = {"up": "down", "right": "wrong", "yes": "no"}  
 alias = original\_dict  
 copy = original\_dict.copy()  # Shallow copy

alias and original\_dict refer to the same object; copy refers to a fresh copy   
of the same dictionary. If we modify alias, original\_dict is also changed:

alias\["right"\] = "left"  
original\_dict\["right"\]

\# Will output  'left'

If we modify copy, original\_dict is unchanged:

copy\["right"\] = "privilege"  
original\_dict\["right"\]

\# Will output 'left'

#### Merging dictionaries

Say you have two dicts and you want to merge them into a new dict without altering the original dicts:

```
x = {'a': 1, 'b': 2}y = {'b': 3, 'c': 4}
```

The desired result is to get a new dictionary (`z`) with the values merged, and the second dict's values overwriting those from the first.

```
z{'a': 1, 'b': 3, 'c': 4}
```

For dictionaries in Python, x and y, z becomes a shallowly merged dictionary with values from y replacing those from x.

In Python 3.5 or greater:

z = {\*\*x, \*\*y}

#In Python 2, (or 3.4 or lower) we have to write a function that will take two dictionary as its argument.

def merge\_two\_dicts(x, y):  
 z = x.copy() # start with x’s keys and values  
 z.update(y) # modifies z with y’s keys and values & returns None  
 return z

#and now:

z = merge\_two\_dicts(x, y)

#### Some implementational differences between Lists and Dictionary

**While Copying — Lists vs Dictionary**

Mutable objects come with one potentially prominent drawback: changes made to an object are visible from every reference to that object. All mutable objects work this way because of how Python references objects, but that behavior isn’t always the most useful. **In particular, when working with objects passed in as arguments to a function, the code that called the function will often expect the object to be left unchanged.** If the function needs to make modifications in the course of its work, i.e. if we want to make changes to an object without those changes showing up elsewhere, we’ll need to copy the object first.

Lists, support slicing to retrieve items from the list into a new list. That behavior can be used to get all the items at once, creating a new list with those same items. Simply leave out the start and end values while slicing, and the slice will copy the list automatically:

a = \[1, 2, 3\]

b = a\[:\]

print(b)

\# Output  
\[1, 2, 3\]  
\# The : operator here, as we know, works as a slice operator with sequence

We could also use `list.copy()` to make a copy of the list.

The **slice() method above** returns a slice object, however, ror dictionaries, by contrast, slice objects aren’t hashable, so dictionaries don’t allow them to be used as keys.

a = {1: 2, 3: 4}

b = a.copy()

b\[5\] = 6

print(b)

\# Output  
{1: 2, 3: 4, 5: 6}

By [Rohan Paul](https://medium.com/@paulrohan) on [November 2, 2019](https://medium.com/p/4a48655c7934).

[Canonical link](https://medium.com/@paulrohan/python-list-vs-tuple-vs-dictionary-4a48655c7934)

Exported from [Medium](https://medium.com) on December 12, 2020.