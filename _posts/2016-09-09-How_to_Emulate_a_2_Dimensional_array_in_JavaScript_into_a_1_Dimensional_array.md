---
layout: post
title: The case for emulating a 2-Dimensional array (i.e. a matrix) in JavaScript into a 1-Dimensional array having a single list of elements
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/Eloquent-Ch-7-2D-Array.jpg" class="fit image">

This quick post was motivated by the issue I faced while doing the project in Chapter-7 (Electronic Life) of the book [Eloquent Javascript](http://eloquentjavascript.net/07_elife.html).

Here, the book-author chooses to convert a 2-dimensional array (created by using nested one-dimensional arrays) into a 1-dimensional one for the grid construction required in this project.

**Excerpts from the book :**


<script src="https://gist.github.com/rohan-paul/830d928481154ac60787b1fb96e12ede.js"></script>


**Explanation of the above formula**

Multidimensional arrays ( "arrays of arrays" ) are a natural first step towards building systems for solving numerical problems, and is a very useful data type, with serious applications in graphics, vision and game programming. Especially convenient for programming sketches that involve some sort of "grid" or "board."

To begin with the basic arithmetic behind, in most programming languages, the general conversion formula used to emulate a 2-dimensional array (i.e. a matrix) into a 1-dimensional one is the below:

Assume, the matrix has size n by m; and remember indices begin from 0. That is, i goes from 0 to (n-1) and j from 0 to (m-1).

With 'i' representing the row number (i.e. height or distance from the top of the Matrix), and 'j' the column number (i.e. width or distance from the left of the Matrix). If I start numbering from i,j = (0,0) representing a postition at row 0 and column 0, for a 'row-major' ordering (consecutive elements of the rows of the array are contiguous in memory). [0][1] is the second item on the top row, [1][n] is on the second row and so forth.

Then to get the index (i, j) use the below formula:

**2d[i][j] = 1d[ j + i * Total_number_of_columns_in_the_matrix ]**

In another form

**2d[height][width] = 1d[ width + height * Total number_of_columns_in_the_Matrix ]**


So, in the above, excerpts from the book, the grid variable is a matrix of 2 * 3 format (2 rows and 3 columns) as below.

| top left   |      top middle     |  top right |
|----------|:------------:|------:|
| bottom left | bottom middle | bottom right |

Now say, I want to know the index of the item at (row 1, column 2) in the 1-dimensional array representation. That is, the position at (1, 2) which is "bottom right". And that's the item at 5th position in 1-dimensional representation of the array.

So, applying the formulae..

grid[ 2 + ( 1 * 3) ] would give me grid[5] which is indeed "bottom right".


**A conceptual understanding of how the above formula works to give me the correct index number**

The array elements here in a single row are in groups of 3. So, I need to find the count of where the group I want starts. That's what ``i * width_of_grid`` does. Once I find where the group I want starts, then I add ``j`` to get the count of the cell I want.

So, as I move along the first row (which represents i=0) of our above matrix of 2 rows and 3 columns, (i.e. increase the column number), I am just counting up, so the Array indices are 0,1,2.

Then, I get to the second row (i = 1), now, because, I already have 3 entries from the first row, so I need to start with indices 1*3 + 0,1,2

Then if I had a third row in the matrix, I have 2*3 entries already from first and second row, thus in the third row, the indices need to be counted as 2*5 + 0,1,2

For higher dimensions, this idea can be generalized. So for a 3D matrix L by N by M:

matrix[ i ][ j ][ k ] = array[ i * (N*M) + j * M + k ]


For more mathematical details: [http://en.wikipedia.org/wiki/Row-major_order](https://en.wikipedia.org/wiki/Row-major_order)


**Benefit to convert a 2D arrays-of-arrays into a 1D-Array**

While, the syntax for accessing elements in a 2-dimensional array-of-arrays is quite simple:

var x = A[i][j]

And also, not requiring introduction of any extra data structures, but generally for large number of elements, and when I have to jump rows frequently, 1-dimensional flattened array is slightly better from performance perspective.

The most obvious problem is that to allocate a d-dimensional array-of-arrays with O(n) elements per each element (linear time complexity), I need to store O(n^{d-1}) extra bytes worth of memory in pointers and intermediate arrays. A 2D+ array will first have to have one index looked up, then in the resulting array yet another and so forth.

However, for the best possible gain in pure performance [Typed Arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays) are the best. These are actual low-level byte arrays and we can use them with 8-bit, 16-bit, 32-bit and 32/64 bits float values.
