# Vectorizing Gradient Descent — Multivariate Linear Regression and Python implementation

In this article, I shall go over the topic of arriving at the Vectorized Gradient-Descent formulae for the Cost function of the for Matrix…

![](https://cdn-images-1.medium.com/max/1200/1*lav3vWKSf34PvIFPiCaStg.jpeg)

In this article, I shall go over the topic of arriving at the **Vectorized Gradient-Descent formulae for the Cost function of the for Matrix form of training-data Equations.** And along with that the Fundamentals of Calculus (especially Partial Derivative) and Matrix Derivatives necessary to understand the process.

**So our target of this article is to understand the full Mathematics and the flow behind arriving at the below formulae**, which is the Vectorized Gradient of the training-data Matrix

![](https://cdn-images-1.medium.com/max/800/1*JPmxQrvKmjcAxiURYOCYUA.png)
undefined

### First a Refresher on basic Matrix Algebra

A matrix A over a field K or, simply, a matrix A (when K is implicit) is a rectangular array of scalars usually presented in the following form:

![](https://cdn-images-1.medium.com/max/800/1*Z7Y4gBTgQMym-MVR7DZ8pw.png)
undefined

The rows of such a matrix A are the m horizontal lists of scalars:

![](https://cdn-images-1.medium.com/max/800/1*Ev4S0SCYrgKd7SP7wXitzg.png)
undefined

and the columns of A are the n vertical lists of scalars:

![](https://cdn-images-1.medium.com/max/800/1*Hy4T3QGYGiT2YPwzoMdTSQ.png)
undefined

A matrix with m rows and n columns is called an m by n matrix, written m\*n. The pair of numbers m and n is called the size of the matrix. Two matrices A and B are equal, written A = B, if they have the same size and if corresponding elements are equal. Thus, the equality of two m \* n matrices is equivalent to a system of mn equalities, one for each corresponding pair of elements.  
A matrix with only one row is called a row matrix or row vector, and a matrix with only one column is called a column matrix or column vector. A matrix whose entries are all zero is called a zero matrix and will usually be denoted by 0.

### Matrix Multiplication

The below image is taken from Khan Academy’s excellent linear algebra course.

![](https://cdn-images-1.medium.com/max/800/1*jYjtn1J2xM5iror3FXK4xQ.png)

**In above, each entry in the product matrix is the dot product of a row in the first matrix and a column in the second matrix**

**More explanation for higher dimension case** — If the product **AB = C** is defined, where C is denoted by \[cij\], then the  
element cij is obtained by multiplying the elements in the ith row of **A** by the corresponding elements in the jth column of **B** and adding. Thus, if **A** has order k \* n, and **B** has order n \* p then

![](https://cdn-images-1.medium.com/max/800/1*fNTSrRCAcK_j4HvbLslqvA.png)
undefined

**then c11 is obtained by multiplying the elements in the first row of A by the corresponding elements in the first column of B and adding; hence,**

![](https://cdn-images-1.medium.com/max/800/1*7RSD_Z9KmiX5EKemHPL_ww.png)
undefined

**The element c12 is found by multiplying the elements in the first row of A by the corresponding elements in the second column of B and adding; hence,**

![](https://cdn-images-1.medium.com/max/800/1*UrH_UpFOZ6zZmv6BK8pJ4g.png)
undefined

**The element ckp below is obtained by multiplying the elements in the kth row of A by the corresponding elements in the pth column of B and adding; hence,**

![](https://cdn-images-1.medium.com/max/800/1*x779-OyJDcpwkLpunSLusw.png)
undefined

**There are four simple rules that will help us in multiplying matrices, listed here**

1\. Firstly, we can only multiply two matrices when the number of columns in  
matrix A is equal to the number of rows in matrix B.

2\. Secondly, the first row of matrix A multiplied by the first column of matrix B  
gives us the first element in the matrix AB, and so on.

3\. Thirdly, when multiplying, order matters — specifically, AB ≠ BA.

4\. Lastly, the element at row i, column j is the product of the ith row of matrix A and the jth column of matrix B.

The order in which we multiply matters. We must keep the matrices  
in order, but we do have some flexibility. As we can see in the following equation, the parentheses can be moved:

![](https://cdn-images-1.medium.com/max/800/1*abGjdRm7NAazy9_-lmCqVg.png)
undefined

The following three rules also apply for Matrix Operation.

![](https://cdn-images-1.medium.com/max/800/1*ZIYKAV-1BlirjaAPxuKbSA.png)
undefined

Let's see an example of Matrix multiplication

Another example of Matrix multiplication

![](https://cdn-images-1.medium.com/max/800/1*8O9Z0rMLEfl3JyGJnqE_yA.png)
undefined

Implementing a dot production with numpy

import numpy as np

_\# this will create integer as array-elements  
_A = np.random.randint(10, _size_\=(2,3))  
B = np.random.randint(10, _size_\=(3,2))

This will create 2 Matrices as below e.g.  
\[\[9 0 8\] \[3 5 5\]\]

\[\[5 2\]  
 \[0 7\]  
 \[0 7\]  
\]

print(np.dot(A, B))

\# Output   
\[\[45 74\]  
\[15 76\]\]

#### The Hadamard Product — Other Kinds of Matrix Multiplication

Hadamard multiplication is defined for matrices of the same shape as the multiplication of each element of one matrix by the corresponding element of the other matrix. Hadamard multiplication is often denoted by as below, for two matrices A(n×m) and B(n×m) we have

![](https://cdn-images-1.medium.com/max/800/1*MA5IGGGKTYlyqQq_GEwvOA.png)
undefined

### Refresher on Multivariate Linear Regression

#### First, start with the **Simple Linear Regression (SLR)**.

Suppose we have 2 equations as below

10 = 2x + 2y  
18 = 4x + y

So the Matrix form

![](https://cdn-images-1.medium.com/max/800/1*RyDyp9YR3izJPoWOfQN4Vg.png)
undefined

**So in general Mathematic form for the single independent variable case**

![](https://cdn-images-1.medium.com/max/800/1*ks4aGLCIcSpl0OEzS9gW3g.png)
undefined

So the set of equations for all the observation will be as below

![](https://cdn-images-1.medium.com/max/800/1*FbGj_xW0feYo-VzVUCRaSg.png)
undefined

And the Matrix form of which is below

![](https://cdn-images-1.medium.com/max/800/1*pf4gjU4ZPNzTYXBXNh2yUQ.png)
undefined

So **Y** is n \* 1 matrix, **X** is an \* 2 matrix, **β** is 2 \* 1 matrix

![](https://cdn-images-1.medium.com/max/800/1*e8BUHsZOSP3fYxsmnWmJwQ.png)

#### **Multiple Linear Regression (MLR)**

Suppose that the response variable Y and at least one predictor variable xi are quantitative. Then the equation for a specific Y value under the **MLR** model is

![](https://cdn-images-1.medium.com/max/800/1*z_Le5Iw_hnqBNAUtjxCyBg.png)
undefined

for i = 1, . . . , n. Here n is the sample size and the random variable ei is the  
ith error. So

![](https://cdn-images-1.medium.com/max/800/1*mU2qyHAz6l_sAPjiX2vivQ.png)
undefined

Which is in compact notation below

![](https://cdn-images-1.medium.com/max/800/1*b0Mjk-rNWJzZVp1m6IeiRw.png)
undefined![](https://cdn-images-1.medium.com/max/800/1*LXE1X_GBi6w__cwn1DyPrg.png)
undefined

**And now in matrix notation, these n sets of equations become**

![](https://cdn-images-1.medium.com/max/800/1*gytWOZW1m99KNjzv5dCNeg.png)
undefined

where **Y** is the vector of the response variable and is an n × 1 vector of dependent variables, **X** is the matrix of the k independent/explanatory variables (usually the first column is a column of ones for the constant term) and is an n × p matrix of predictors, **β** is a p × 1 vector of unknown coefficients, and e is an n × 1 vector of unknown errors. Equivalently,

![](https://cdn-images-1.medium.com/max/800/1*xG70qjyrg_rOpk-mb0sfGg.png)
undefined

Where  
**Y** : is output vector for n **training examples.  
X** : is matrix of size n**\*p** where each ith row belongs to ith training set.  
**β** : is weight vector of size p for p training features.

### Different types of Matrix Differentiation

#### **1-Differentiation with Respect to a Scalar**

Differentiation of a structure (vector or matrix, for example) with respect to a scalar is quite simple; it just yields the ordinary derivative of each element of the structure in the same structure. **Thus, the derivative of a vector or a matrix with respect to a scalar variable is a vector or a matrix, respectively, of the derivatives of the individual elements**.

**1.A-Derivatives of Vectors with Respect to Scalars**

The derivative of the vector y(x) = (y1 , . . . , yn ) with respect to the scalar x  
is the vector

![](https://cdn-images-1.medium.com/max/800/1*qC_wogA-j6jLveKF64dEXg.png)
undefined

The second or higher derivative of a vector with respect to a scalar is likewise a vector of the derivatives of the individual elements; that is, it is an array of higher rank.

**1.B-Derivatives of Matrices with Respect to Scalars**

The derivative of the matrix **Y(x)** defined as below

![](https://cdn-images-1.medium.com/max/800/1*ubqLah4DbO64Bulfd45-ag.png)
undefined

with respect to the scalar, x is the matrix

![](https://cdn-images-1.medium.com/max/800/1*rAlOr-wUTrAleLRYUmF8Aw.png)
undefined

The second or higher derivative of a matrix with respect to a scalar is  
likewise a matrix of the derivatives of the individual elements.

#### 1.C — Derivatives of Functions with Respect to Scalars

Differentiation of a function of a vector or matrix that is linear in the elements  
of the vector or matrix involves just the differentiation of the elements, fol-  
lowed by application of the function. For example, the derivative of a trace of  
a matrix is just the trace of the derivative of the matrix. On the other hand,  
the derivative of the determinant of a matrix is not the determinant of the  
derivative of the matrix

#### 1.D — Higher-Order Derivatives with Respect to Scalars

Because differentiation with respect to a scalar does not change the rank of the object (“rank” here means rank of an array or “shape”), higher-order derivatives

![](https://cdn-images-1.medium.com/max/800/1*utNbveSxIpgXJUJqX9FZpw.png)
undefined

with respect to scalars are merely objects of the same rank  
whose elements are the higher-order derivatives of the individual elements.

#### 2-Differentiation with Respect to a Vector

Differentiation of a given object with respect to an n-vector yields a vector for each element of the given object. The basic expression for the derivative, from formula

![](https://cdn-images-1.medium.com/max/800/1*q-p7BNUvoqlugltTiGtCVw.png)
undefined

for an arbitrary conformable vector y. The arbitrary y indicates that the derivative is omnidirectional; it is the rate of change of a function of the vector in any direction.

#### 2.A — Derivatives of Scalars with Respect to Vectors; The Gradient

The derivative of a scalar-valued function with respect to a vector is a vector  
of the partial derivatives of the function with respect to the elements of the  
vector. If f (x) is a scalar function of the vector x = (x1 , . . . , xn ),

![](https://cdn-images-1.medium.com/max/800/1*oP3EN-tXI9TxwyYr2hu25A.png)
undefined

if those derivatives exist. This vector is called the gradient of the scalar-valued  
function, and is sometimes denoted by ∇f (x)

#### 2.B — Derivatives of Vectors with Respect to Vectors; The Jacobian

The derivative of an m-vector-valued function of an n-vector argument consists of nm scalar derivatives. These derivatives could be put into various structures. Two obvious structures are an n × m matrix and an m × n matrix.

For a function,

![](https://cdn-images-1.medium.com/max/800/1*RypMapKwOWkXzAmZ7lvtcA.png)
undefined

we define

![](https://cdn-images-1.medium.com/max/800/1*VpZYNCbTNSCnpXBxUW4zHA.png)
undefined

to be the n × m matrix, which is the natural extension of ∂/∂x applied to a scalar function. The above the notation is more precise because it indicates that the elements of f correspond to the columns of the result.

![](https://cdn-images-1.medium.com/max/800/1*WUY3HJZ1eqHfCwceNvYDgA.png)
undefined

if those derivatives exist. This derivative is called the matrix gradient and  
is denoted by ∇f for the vector-valued function f . (Note that the ∇  
symbol can denote either a vector or a matrix, depending on whether the  
function being differentiated is scalar-valued or vector-valued.)

**The m × n matrix is called the Jacobian of f and is denoted by Jf as below**

![](https://cdn-images-1.medium.com/max/800/1*lxwoMi34O7yt9u4ac2yY7w.png)
undefined

### General Form of the Jacobian

For arriving at the general Mathematical form of Jacobian I would refer a quite [well-recognized Paper](https://arxiv.org/pdf/1802.01528.pdf) in this field.

Let **y = f(x)** be a vector of m scalar-valued functions that each take a vector x of length **n = |x|** where |x| is the cardinality (count) of elements in x. Each fi function within **f** returns a scalar just as in the previous section:

![](https://cdn-images-1.medium.com/max/800/1*uy7W1fPZHsBIRP68F-cplQ.png)
undefined

For instance,

![](https://cdn-images-1.medium.com/max/800/1*wXAxCx51hA7gkj71OEIvdg.png)
undefined

So the set of equations are as below

![](https://cdn-images-1.medium.com/max/800/1*qVogW73Z3IYAjy1BND1d8Q.png)
undefined

So we have m = n functions and parameters, in this case. Generally speaking, though, **the Jacobian matrix is the collection of all m × n possible partial derivatives (m rows and n columns), which is the stack of m gradients with respect to x:**

![](https://cdn-images-1.medium.com/max/1200/1*_VbzYvXGjMZVe4jNlcfaWg.png)
undefined

Each of these

![](https://cdn-images-1.medium.com/max/800/1*eTRerCjUn84a5t9Eyx4b8Q.png)
undefined

is a horizontal n-vector because the partial derivative is with respect to a vector, x, whose length is n = |x|. The width of the Jacobian is n if we’re taking the partial derivative with respect to x because there are n parameters we can wiggle, each potentially changing the function’s value. Therefore, the Jacobian is always m rows for m equations.

### Now the Cost Function under Multivariate Linear Regression

The equation for the hypothesis function is as follows

![](https://cdn-images-1.medium.com/max/800/1*qp_vtZgLiCSNoU41L0eGow.png)
undefined

The general notations that I will use for extending the above function

![](https://cdn-images-1.medium.com/max/800/1*M7dNnqhtPHAWJekLPzo-8Q.png)
undefined

So if we are predicting house-price with the above MLR equation, then _θ_0 will be the basic/base price of a house, then _θ_1 as the price per room, _θ_2 as the price per KM-distance from the nearest Airport.

Using the definition of matrix multiplication, our multivariate hypothesis function can be concisely represented as:

![](https://cdn-images-1.medium.com/max/800/1*asQQI3lFzXRdBfrtX5y1MQ.png)
undefined

This is a vectorization of our hypothesis function for one training example;

Now, using the fact that for a vector z, we have that

![](https://cdn-images-1.medium.com/max/800/1*XgaPuVFe817NZW4nlRDcuQ.png)
undefined

Applying the above identity to the right-hand-side of the Cost function (below)

![](https://cdn-images-1.medium.com/max/800/1*B-qvrdYQDmdKxKminrxH-Q.png)
undefined

So now the Cost function takes the following form

![](https://cdn-images-1.medium.com/max/800/1*WUsD8CFsC5H78aouLZFs3w.png)
undefined

And our target is to

![](https://cdn-images-1.medium.com/max/800/1*6do1HGRtbK1C_u0nW5h58Q.png)
undefined

Wher the thetas θ are the weights, and the above partial derivative for any weights wj will be as below

![](https://cdn-images-1.medium.com/max/800/1*sZi1c1yL_8gupQrJtgfazA.png)
undefined

So the Gradient-Descent process for Multivariate case becomes

![](https://cdn-images-1.medium.com/max/800/1*QluVOgLvQ4RB2_IBwE0EkQ.png)
undefined

Where θ and x are column vector given by

![](https://cdn-images-1.medium.com/max/800/1*Sej0ld0Ahn2jsvVv0t6-OQ.png)
undefined

And that's why we take the transpose of θ to multiply with column-vector x to get the hypothesis (as earlier mentioned in this article)

![](https://cdn-images-1.medium.com/max/800/1*asQQI3lFzXRdBfrtX5y1MQ.png)
undefined

### Refresher — Matrix-Derivative Identities required for the Mathematical Derivation of the Gradient of a Matrix w.r.t. to Vectors

The derivative of a matrix is usually referred to as the gradient and denoted as ∇. Consider a function

![](https://cdn-images-1.medium.com/max/800/1*Ba_SG3OaqvgLJz0MO4oEwQ.png)
undefined

That is the Matrix will be as below

![](https://cdn-images-1.medium.com/max/800/1*UeisP-Ui6Na1LXX2ACqRCw.png)
undefined

Then,

![](https://cdn-images-1.medium.com/max/800/1*2CVL0vtFozL8yFRja5-D1w.png)
undefined

Thus, the gradient ∇Af(A) is itself an m-by-n matrix, whose (i, j)-element is

![](https://cdn-images-1.medium.com/max/800/1*Mq9Lgie0lCtdNBf46FRzSA.png)
undefined

For example, lets take a look at a very simple case. Suppose

![](https://cdn-images-1.medium.com/max/800/1*sYpBmF4XVcpdI9b8VzP-OA.png)
undefined

is a 2-by-2 matrix, and the function

![](https://cdn-images-1.medium.com/max/800/1*kvUQK-Ndj6bkZfkaRZVp4Q.png)
undefined

is given by

![](https://cdn-images-1.medium.com/max/800/1*fqtrwQBn10hY3dD2tpPNpg.png)
undefined

Here, Aij denotes the (i, j) entry of the matrix A. We then have

![](https://cdn-images-1.medium.com/max/800/1*UoBIW03p-QR0EB8qaQkmvw.png)
undefined

### Matrix Transpose Identities

Each of the below identities can be proved separately mathematically proved.

![](https://cdn-images-1.medium.com/max/800/1*h6bwL_K_X97XdffTZxkR2w.png)
undefined

Another related one, If 𝐴 and 𝐵 are two matrices of the same order, then

![](https://cdn-images-1.medium.com/max/800/1*YbUQQ6xfwwf_e8xqs5K4Fg.png)
undefined

#### The Trace: tr(·) of a Matrix

The sum of the diagonal elements of a square matrix is called the trace of the  
matrix. We use the notation “tr(A)” to denote the trace of the matrix A:

![](https://cdn-images-1.medium.com/max/800/1*Pxqs5JaBfecbVkKFM1OMuw.png)
undefined![](https://cdn-images-1.medium.com/max/800/1*q6zf6VbQackvsBSmpujWYQ.png)
undefined

Because of the associativity of matrix multiplication, this relation can be  
extended as

![](https://cdn-images-1.medium.com/max/800/1*_pcySNgZxp8kqVAv5Onibg.png)
undefined

### Now proceed to find the Gradient of the Cost function.

#### First a Refresher on the Gradient Descent Algorithm

To implement Gradient Descent, you need to compute the gradient of the cost function with regard to each model parameter θj. In other words, you need to calculate how much the cost function will change if you change θj just a little bit. This is called a partial derivative. It is like asking “What is the slope of the mountain under my feet if I face east?”. and then asking the same question facing north.

Now you would recognize the very well-known cost function

![](https://cdn-images-1.medium.com/max/800/1*bvUpqMmn3Ui0xgXWtrOlcg.png)
undefined

And the following the Jacobian identity discussed above the Gradient vector of the cost function will be

![](https://cdn-images-1.medium.com/max/800/1*tc5cu5BRRmrB47FjtVAwAg.png)
undefined

Notice that this formula involves calculations over the full training set **X**, at each Gradient Descent step! This is why the algorithm is called **Batch Gradient Descent**: it uses the whole batch of training data at every step.

Once you have the gradient vector, which points uphill, just go in the opposite direction to go downhill. This means subtracting ∇θMSE(θ) from θ. This is where the learning rate η comes into play:5 multiply the gradient vector by η to determine the size of the downhill step

![](https://cdn-images-1.medium.com/max/800/1*XIm_W2QAYrqJftCE42t0Dg.png)
undefined

**Now repeating below section of the Matrix form of the training dataset, from our earlier part of this article —**

The general form of multiple linear regression (MLR) model is

![](https://cdn-images-1.medium.com/max/800/1*1SkKxwZLZy6SQvpuWkiQFA.png)
undefined

for i = 1, . . . , n. Here n is the sample size and the random variable ei is the  
ith error. I**n matrix notation, these n sets of equations become**

![](https://cdn-images-1.medium.com/max/800/1*gytWOZW1m99KNjzv5dCNeg.png)

The **Y** vector is the response variable and is an n × 1 vector of dependent variables, **X** is the matrix of the k independent/explanatory variables (usually the first column is a column of ones for the constant term) and is an n × p matrix of predictors, **β** is a p × 1 vector of unknown coefficients, and e is an n × 1 vector of unknown errors. Equivalently,

![](https://cdn-images-1.medium.com/max/800/1*xG70qjyrg_rOpk-mb0sfGg.png)
undefined

Where  
**Y** : is output vector for n **training examples.  
X** : is matrix of size n**\*p** where each ith row belongs to ith training set.  
**β** : is weight vector of size p for p training features.

Note that β in the above is not a scalar, but a vector.

Now we have the RSS defined as

![](https://cdn-images-1.medium.com/max/800/1*typfSXiJDet6UwB2-9Vq-Q.png)
undefined

### Alternative-1 — Vectorized Calculation of Gradient of our Matrix form training data

Note the above is directly derived from using the identity that for a vector z, we have

![](https://cdn-images-1.medium.com/max/800/1*HRZOm73aCQkTwcHM8U817Q.png)
undefined![](https://cdn-images-1.medium.com/max/800/1*fkkkh9pthCgqJvZ29YuL4A.png)
undefined

Now let,

![](https://cdn-images-1.medium.com/max/800/1*aw74yCdXcb8-GYx18UBGJA.png)
undefined

Then for the following assumption

![](https://cdn-images-1.medium.com/max/800/1*wCTijrmwU43BeoFujfX_eg.png)
undefined![](https://cdn-images-1.medium.com/max/800/1*Z13CkDXJtTTptNd9feSHGQ.png)
undefined

Therefore,

![](https://cdn-images-1.medium.com/max/800/1*dqxnBP7PXrycxImjvHXaeg.png)
undefined

**We have, for each of _k_\=1,…, _p_**

![](https://cdn-images-1.medium.com/max/800/1*iEoYbHv2PyKuOPy5GFe_zA.png)
undefined

Then for the whole matrix (i.e. the whole set of training data set or the whole set of Hypothesis Equation ), we will get

![](https://cdn-images-1.medium.com/max/800/1*CJ7VQA1EMOFGKmBuAHsGYw.png)
undefined

Which in the final Vector form

![](https://cdn-images-1.medium.com/max/800/1*RWEJfod0BlYgMSylSwr0MA.png)

Note, that in the last equality, I had to get the Transpose of **X** because when doing matrix multiplication — that's a dot product of rows of the first matrix to columns of the second matrix. The number of columns of the 1st matrix must equal the number of rows of the 2nd matrix.

So by transposing the _p-_th column of **X** ends up being the _p-_th row of the X-Transposed. Thus, when doing a dot product between the p-th row of **X-Transposed with (y — Xβ) it will match perfectly as I am** using all of the entries of the _p-_th column of **X**

### Alternative-2 — And below is an alternative calculation for arriving at the same Vectorized Gradient formulae for training-data in Matrix form.

Here, I am denoting the coefficients with **θ or Theta (instead of β that we used above in our Alternative-1 Gradient Calculation — only to make the presentation differentiable)**

Again assume we have our Training Set of data as below

![](https://cdn-images-1.medium.com/max/800/1*7CBrI6xXmiIGsP-BddHLDw.png)
undefined

Also, let **y** be the m-dimensional vector containing all the target values from the training set:

![](https://cdn-images-1.medium.com/max/800/1*mTYp0mTm1eoBjGhl_oD70w.png)
undefined

And we have the Predicted Value or the Hypothesized value as below

![](https://cdn-images-1.medium.com/max/800/1*rcj6DVtHGYImNIlAPgtMBA.png)
undefined

So we can say the below

And now again, we need to use the same **vector identity** mentioned above, that for a vector z, we have

![](https://cdn-images-1.medium.com/max/800/1*HRZOm73aCQkTwcHM8U817Q.png)
undefined

Using the above we have the below relation for the Cost function

![](https://cdn-images-1.medium.com/max/800/1*3dmZKXEIrhcpKhyYvky3fw.png)
undefined

We have already introduced the trace operator of a Matrix, written as **“tr.”** Now we need to use a couple of more **matrix derivatives Identities** (that I am just stating below here, and they all have robust Mathematical proofs, the details of which I am not including here).

So below 2 M**atrix Derivative Identities hold true and we need to use them to arrive at the Gradient Calculation.**

![](https://cdn-images-1.medium.com/max/800/1*RwqJFodt5c5xx9QWo0_mJg.png)

Combining the above two Equations or Identities we derive

![](https://cdn-images-1.medium.com/max/800/1*6sTQO3HWCvb5kBK0SjaRrQ.png)
undefined

So now Final Gradient Calculation will be as below

![](https://cdn-images-1.medium.com/max/800/1*8vk4kGAQ1nr9uRTcRqtSQw.png)
undefined

In the third step above, we used the fact that the trace of a real number is just the real number; the fourth step used the fact that

![](https://cdn-images-1.medium.com/max/800/1*UmbyevOF6r-gTCmBvJMc0g.png)
undefined

And the fifth step used below equation that we already mentioned

![](https://cdn-images-1.medium.com/max/800/1*GBVfdoY691FvRI-g81uxew.png)
undefined

And also the below Matrix Identity

![](https://cdn-images-1.medium.com/max/800/1*3H0jOMR9C4U-ykwsjZHZ1Q.png)
undefined

With

![](https://cdn-images-1.medium.com/max/800/1*BFMwcbJMLRGmSesT5QpaJw.png)
undefined

Take a note of the final result of the Gradient, which is the same form that we arrived at earlier under the Alternative-1 calculation of Gradient

![](https://cdn-images-1.medium.com/max/1200/1*0hjVo1G2QsfGjmj48vCe2Q.png)

And above is the exact formulae that we will implement in Python/Numpy very soon below.

### **So now let's go back to the original Cost Function**

![](https://cdn-images-1.medium.com/max/800/1*a6HUYDO5WRfUu4dpcSkwaA.png)

**Which in Vectorized Form for the Mean Squared Error is defined as below**

![](https://cdn-images-1.medium.com/max/800/1*hABj8H0iq2G-S1qC-pjirQ.png)

And after calculating the Gradient of this MSE in Vectorized form, which we did above the Gradient-Descent Algorithm will update the **weights (θ / Theta values** ) as below

![](https://cdn-images-1.medium.com/max/800/1*ZHzoW49vH-a5c45lrjABSw.png)

Compare the above with the Gradient-Descent formulae for the Numerical case

![](https://cdn-images-1.medium.com/max/800/1*QhGYPsTKDMuN2XXV2-9tzA.png)
undefined

### A manual example of the Gradient-Descent implementation

Let's say for simple single variable training dataset we have the following values

```
x,y1,12,23,34,4
```

Further, assume,

α (learning rate) = 1

m (number of training examples) =4

Setting _θ_0 to 0 and _θ_1 to 1

So we have the linear equation

![](https://cdn-images-1.medium.com/max/800/1*WbmMq0A-rS-DiWWWzWMVrQ.png)
undefined

Now by convention,

![](https://cdn-images-1.medium.com/max/800/1*pcm_T-khvSgPtg3-vgjd1g.png)
undefined

at the _i_−th row

Further I denote,

![](https://cdn-images-1.medium.com/max/800/1*F0yXIAbfID8ZbIsnQicwdw.png)
undefined

(i.e. after _k_ repetitions of the GD algorithm).

So after a single update, with GD algorithm, i.e. applying the below

![](https://cdn-images-1.medium.com/max/800/1*ZHzoW49vH-a5c45lrjABSw.png)
undefined![](https://cdn-images-1.medium.com/max/800/1*qZniIowC3-qj4PM_MBXiCQ.png)
undefined

So, regardless of how many times I apply the GD algorithm, the value of θ1 will be constantly equal to 1, since at every iteration we have _θ_0=0 and _θ_1=1

#### Now extend the above to a multivariable case,

Assume theta values have been picked at random as below

![](https://cdn-images-1.medium.com/max/800/1*DEyr8pXY-YGRS0VCSDhkNQ.png)
undefined

And the training-dataset is

![](https://cdn-images-1.medium.com/max/800/1*JFmJrQcKJWhSO6qi5hShpw.png)
undefined

So here, first, to calculate the hypothesis Equation, I need to transpose _θ_ to give our initial vector _θ_

![](https://cdn-images-1.medium.com/max/800/1*ppUW7PoQ4khUZDd47M0GKw.png)
undefined

And the GD-Algorithm is,

![](https://cdn-images-1.medium.com/max/800/1*JPmxQrvKmjcAxiURYOCYUA.png)
![](https://cdn-images-1.medium.com/max/800/1*2hJfGKtavtUlQKXF_mpVbw.png)
undefined

And for applying the GD algorithm again, I need to evaluate

![](https://cdn-images-1.medium.com/max/800/1*l06ChICjtfOkpXtnUZN5Tg.png)
undefined

### Python/Numpy Implementation

Please refer to the jupyter notebook

First, **generate a training dataset** in Matrix form

undefined
undefined

**NumPy zeros()** function in above **—** you can create an array that only contains only zeros using the NumPy zeros() function with a specific shape. The shape is row by column format. Its syntax is as below

![](https://cdn-images-1.medium.com/max/800/1*6Z-1jRbnX3VlRDGWEZzTKw.png)
undefined

For example, the code to generate a Matrix of 2 by 3 (2 rows and 3 columns)

import numpy as np

A = np.zeros(shape = (2, 3))  
print

\# Output below  
\[\[0. 0. 0.\]  
 \[0. 0. 0.\]\]

Which produces an array like the following:

![](https://cdn-images-1.medium.com/max/800/1*qK3b2NWHDEB8p6hHbvB0gw.png)
undefined

If I run the above **gen\_data()** function above for a set of 5 training data-set as below with bias and variance of 20 and 10 respectively

gen\_data(5, 20, 10)

I will have the following form of output

(array(\[\[1., 0.\],  
 \[1., 1.\],  
 \[1., 2.\],  
 \[1., 3.\],  
 \[1., 4.\]\]),   
 array(\[22.38023816, 24.18406356, 28.01360908, 26.80051617, 29.30101971\])  
 )

And now the function for Gradient-Descent implementing the Grdient formulae for a Mactrix that we derived above

![](https://cdn-images-1.medium.com/max/800/1*JPmxQrvKmjcAxiURYOCYUA.png)
undefined
undefined

And now finally invoke the above 2 functions to create some linear data and run the gradient-descent and also plot it to a graph.

undefined
undefined

And the resultant graph of the Linear line will be this

![](https://cdn-images-1.medium.com/max/800/1*RX0EWhuHKFWqEuKpWS9fxQ.png)
undefined

**The** [**full notebook is here**](https://gist.github.com/rohan-paul/de3999b34b480d5ee26acc9105bf50a9)

undefined
undefined

#### Reference and Further Reading

**Matrix Multiplication** — [https://en.wikipedia.org/wiki/Matrix\_multiplication](https://en.wikipedia.org/wiki/Matrix_multiplication)

**Matrix-Calculus —** [https://en.wikipedia.org/wiki/Matrix\_calculus](https://en.wikipedia.org/wiki/Matrix_calculus)

**Vector\_Field** — [https://en.wikipedia.org/wiki/Vector\_field](https://en.wikipedia.org/wiki/Vector_field)

**Matrix Transpose Properties** -[https://en.wikipedia.org/wiki/Transpose#Properties](https://en.wikipedia.org/wiki/Transpose#Properties)

**Matrix Cookbook —** [https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf)

**Online Calculation of Matrix Derivative** — [http://www.matrixcalculus.org/](http://www.matrixcalculus.org/)

By [Rohan Paul](https://medium.com/@paulrohan) on [October 7, 2020](https://medium.com/p/e12758bc31b2).

[Canonical link](https://medium.com/@paulrohan/vectorizing-gradient-descent-multivariate-linear-regression-and-python-implementation-e12758bc31b2)

Exported from [Medium](https://medium.com) on December 12, 2020.