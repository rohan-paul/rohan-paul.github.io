# Axes and Dimensions in Numpy and Pandas Array

As a career Data-Scientist, all through your life you have to deal with Matrix form of data where data in Numpy or Pandas or TensorFlow…

![](https://cdn-images-1.medium.com/max/1200/0*ulOAcQhhDRMA6shJ)

As a career Data-Scientist, all through your life you have to deal with Matrix form of data where data in **Numpy or Pandas or TensorFlow where Axis and Dimensions are the fundamental structural concept.**

**Basic Attributes of the ndarray Class**

![](https://cdn-images-1.medium.com/max/800/1*dkts4gxzZ9WNR5fqC-fUew.png)

### What is the Shape of an Array

Let's consider the below array

![](https://cdn-images-1.medium.com/max/800/1*shQXN122fHKMY0_-jXyrTQ.png)
undefined

The “shape” of this array is a tuple with the number of elements per axis (dimension). In our example, the shape is equal to (6, 3), i.e. we have 6 lines and 3 columns.

Numpy has a function called “shape” which returns the shape of an array. The shape is a tuple of integers. These numbers denote the lengths of the corresponding array dimension.

x = np.array(\[ \[67, 63, 87\],  
               \[77, 69, 59\],  
               \[85, 87, 99\],  
               \[79, 72, 71\],  
               \[63, 89, 93\],  
               \[68, 92, 78\]\])

print(np.shape(x))  
\# (6, 3)

### What is the Axis of a Numpy Array

In _Mathematics/Physics_, dimension or dimensionality is informally defined as the minimum number of coordinates needed to specify any point within a space. But in _Numpy_, according to the [**numpy doc**](https://docs.scipy.org/doc/numpy/user/quickstart.html#the-basics), it’s the same as axis/axes:

> **_In Numpy dimensions are called axes. The number of axes is rank._**

In the most simple terms, when you have more than 1-dimensional array than the concept of the Axis is comes at all. For example, the 2-D array has 2 Axis.

The first axis ( i.e. axis-0 ) is running vertically downwards across rows, and the second (axis-1) running horizontally across columns.

![](https://cdn-images-1.medium.com/max/800/0*fr4sfcnDc0KsXA8P.png)
undefined

**What may make more sense from a pure programming perspective of action on array elements :**

> Axis 0 will act on all the ROWS in each COLUMN

> Axis 1 will act on all the COLUMNS in each ROW

#### Basically simplest to remember it as _0=down_ and _1=across_.

**So a** `**mean**` **calculation on axis-0 will be the mean of all the rows in each column, and a mean on axis-1 will be a mean of all the columns in each row.**

Also explaining more, by definition, the axis number of the dimension is the index of that dimension within the array’s `shape`. It is also the position used to access that dimension during indexing.

For example, if a 2D array `a` has shape (5,6), then you can access `a[0,0]` up to `a[4,5]`. Axis 0 is thus the first dimension (the "rows"), and axis 1 is the second dimension (the "columns").

Let's see a quick example, knowing that in Numpy **dimensions are called axes**. The number of axes is rank. So lets calculate dimensions.

```
import numpy as npx = 10print('Dimension of x is ', np.ndim(x))# Dimension of x is  0
```

Then

```
x = [10]print('Dimension of x is ', np.ndim(x))# Dimension of x is  1
```

Then add more elements to the array

```
x = [10, 20, 30 ]print('Dimension of x is ', np.ndim(x))# Dimension of x is  1
```

Dimension is still 1

Now

```
x = [[10, 20, 30 ],     [40, 50, 60 ],     [70, 80, 90 ]    ]print('Dimension of x is ', np.ndim(x))# Dimension of x is  2
```

And now its 2. This is a 2-D array with 3 rows and 3 columns. It has two axis.

In numpy arrays, dimensionality refers to the number of axes needed to index it, not the dimensionality of any geometrical space. For example, you can describe the locations of points in 3D space with a 2D array:

```
array([[0, 0, 0],       [1, 2, 3],       [2, 2, 2],       [9, 9, 9]])
```

Which has shape of (4, 3) and dimension 2. But it can describe 3D space because the length of each row (axis 1) is three, so each row can be the x, y, and z component of a point’s location.

Below an example of 4-Dimensional Array

x = np.random.rand(2,2,3,4)  
print(x)

\# \[\[\[\[0.35353063 0.74159724 0.10044333 0.24097507\]  
\#    \[0.45468341 0.78535831 0.47709755 0.71166768\]  
\#    \[0.27320856 0.23407929 0.49007815 0.71109382\]\]

\#   \[\[0.40494498 0.02846311 0.71847693 0.58137252\]  
\#    \[0.06055296 0.49865532 0.81249606 0.01929813\]  
\#    \[0.06488715 0.10685807 0.25234418 0.88467255\]\]\]

\#  \[\[\[0.78281399 0.96009791 0.07635568 0.79308308\]  
\#    \[0.85729111 0.38536264 0.3186728  0.86997288\]  
\#    \[0.6905416  0.61498    0.89561499 0.78104924\]\]

\#   \[\[0.21998329 0.15469029 0.30835747 0.67322408\]  
\#    \[0.80263922 0.61499647 0.05525479 0.10840465\]  
\#    \[0.10502035 0.80419067 0.9515601  0.87058243\]\]\]\]

### concatenate Numpy Array wtih `np.c_`

An important application of the concept of axis comes into play while concatenating Numpy arrays with the class function **.c\_**

> **the c\_ function concatenates your first array into the last dimension (i.e. axis) of your last array in the function.**

So, under this method,

> **All you have to do is add along second axis.**

So checkout with arrays of the shape of (3, 1) In below both the input arrays has the shape of (3,) But note, there is no second axis. So we mentally add one. So that the shape now of both of the arrays becomes (3,1).

```
x = np.c_[np.array([1,2,3]), np.array([4,5,6])]
```

```
print(x)
```

```
# [[1 4]#  [2 5]#  [3 6]]
```

**Explanations** — following the rule of adding along the second axis — The resultant shape would be (3, 1+1) which is (3,2). which is the shape of the result -

```
[[1 4] [2 5] [3 6]]
```

Now checkout with arrays of shape of (2, 3) — In below both the input arrays has shape of (2, 3)

> **We already noted, that the c\_ function concatenates your first array into the last dimension (axis) of your last array in the function.**

For example:

```
# both are 2 dimensional arraya = array([[1, 2, 3], [4, 5, 6]])b = array([[7, 8, 9], [10, 11, 12]])
```

Now, let’s take a look at `np.c_(a, b)`:

First, let’s look at the shape:

The shape of both a and b are `(2, 3)`. Concatenating a (2, 3) into the last axis of b (3), while keeping other axises unchanged (1) will become

```
(2, 3 + 3) = (2, 6)
```

That’s the new shape. Now, let’s look at the result:

In b, the 2 items in the last axis are:

```
1st: [7, 8, 9]2nd: [10, 11, 12]
```

Adding a to it means:

```
1st item: [1,2,3] + [7,8,9] = [1,2,3,7,8,9]2nd item: [4,5,6] + [10,11,12] = [4,5,6,10,11,12]
```

So, the result is

```
[  [1,2,3,7,8,9],  [4,5,6,10,11,12]]
```

It’s shape is (2, 6)

### Sum of two Numpy Array

Let’s take a look at how NumPy axes work inside of the NumPy sum function.

When trying to understand axes in NumPy sum, you need to know what the axis parameter actually controls.

The axis argument can be an integer, which specifies the axis to aggregate values over. In many cases, the axis argument can also be a tuple of integers, which specifies multiple axes to aggregate over.

#### NUMPY SUM WITH AXIS = 0

Here, we’re going to use the NumPy sum function with axis = 0.

First, we’re just going to create a simple NumPy array.

2d\_array = np.arange(0, 6).reshape(\[2,3\])

The above 2d\_array, is a 2-dimensional array that contains the values from 0 to 5 in a 2-by-3 format.

Let's print to see the whole matrix

print(2d\_array)  
\# \[\[0 1 2\]  
\#  \[3 4 5\]\]

Now sum the 2d\_array

np.sum(2d\_array, axis = 0)  
\# \[3, 5, 7\]

**When we set axis = 0, the function actually sums down the columns.**

Why? Because, the axis parameter indicates which axis gets collapsed.

![](https://cdn-images-1.medium.com/max/800/1*vFeM6u_RQH4EkZoIbeQjUw.png)
undefined

**Remember that axis 0 refers to the vertical direction across the rows. That means that the code np.sum(2d\_array, axis = 0) collapses the rows during the summation.**

![](https://cdn-images-1.medium.com/max/800/1*6vOXYklmCfYhNIR7ENScYg.png)
undefined

**So when we set axis = 0, we’re not summing across the rows. Rather we collapse axis 0.**

#### NUMPY SUM WITH AXIS = 1

Again start with our earlier same array `np_array_2d`

```
print(np_array_2d)[[0 1 2] [3 4 5]]
```

we’re going to use the sum function, and we’ll set the axis parameter to axis = 1.

```
np.sum(np_array_2d, axis = 1)
```

And here’s the output:

```
array([3, 12])
```

**axis 1 refers to the horizontal direction across the columns. That means that the code np.sum(np\_array\_2d, axis = 1) collapses the columns during the summation.**

![](https://cdn-images-1.medium.com/max/800/1*lw_xogT86kMxm_ovf-aPEA.png)
undefined

#### So Generally

If you do `.sum(axis=n)`, for example, then dimension `n` is collapsed and deleted, with each value in the new matrix equal to the sum of the corresponding collapsed values. For example, if `b` has shape `(5,6,7,8)`, and you do `c = b.sum(axis=2)`, then axis 2 (dimension with size 7) is collapsed, and the result has shape `(5,6,8)`.

So now lets see an example with 3-by-3 Numpy Array Matrix

import numpy as np

data = np.arange(1,10).reshape(3,3)  
\# print(data)

\# \[\[1 2 3\]  
\#  \[4 5 6\]  
\#  \[7 8 9\]\]

And now sum them across various axis.

print(data.sum())  
\# 45

print(data.sum(axis=0))  
\# \[12 15 18\]

print(data.sum(axis=1))  
\# \[ 6 15 24\]

![](https://cdn-images-1.medium.com/max/800/1*WDhM1xApLZuIfkTTaIsS4Q.png)
undefined

Similarly, while executing another Numpy aggregation function **np.mean()**, we can specify what axis we want to calculate the values across.

For axis=0, we apply a function along each “column”, or all values that occur vertically.

For axis=1, we apply a function along each “row”, or all values horizontally.

```
values = np.array([[10, 20, 30, 40],[50, 60, 70, 80],])
```

```
# Axis=0# along each "column"print np.mean(values, axis=0)# [30, 40, 50, 60]# the fisrt value 30 is derived as (50+10)/2
```

```
# Axis=1# along each "row"print np.mean(values, axis=1)# [25, 65]# the fisrt value 25is derived as (10+20+30+40)/4 
```

So if you are having problem in remembering the above, while executing aggregating functions on Numpy array — just remember that you are removing that dimension when you run mean or sum on that axis.

**axis=0 removes the row dimension**

**axis=1 removes the column dimension**

And here’s a quick list of various Numpy Aggregation Array Function

![](https://cdn-images-1.medium.com/max/800/1*a-SQDUiZC2Ib04UYA5vs6A.png)

By default, the functions in the above Table aggregate over the entire input array. Using the axis keyword argument with these functions, and their corresponding ndarray methods, it is possible to control over which axis in the array aggregation is carried out. The axis argument can be an integer, which specifies the axis to aggregate values over. In many cases the axis argument can also be a tuple of integers, which specifies multiple axes to aggregate over.

### Same Array and Axis concept apply to Pandas as well

Which is

#### 0=down and 1=across.

**So a** `**mean**` **calculation on axis-0 will be the mean of all the rows in each column, and a mean on axis-1 will be a mean of all the columns in each row.**

df = pd.DataFrame(\[\[10, 20, 30, 40\], \[2, 2, 2, 2\], \[3, 3, 3, 3\]\], _columns_\=\["col1", "col2", "col3", "col4"\])

```
>>> df   col1  col2  col3  col40     1     1     1     11     2     2     2     22     3     3     3     3
```

So if I call `df.mean(axis=1)`, we'll get a mean across the rows:

```
>>> df.mean(axis=1)0    251    22    3
```

By [Rohan Paul](https://medium.com/@paulrohan) on [October 16, 2020](https://medium.com/p/a2490f72631c).

[Canonical link](https://medium.com/@paulrohan/axes-and-dimensions-in-numpy-and-pandas-array-a2490f72631c)

Exported from [Medium](https://medium.com) on December 12, 2020.