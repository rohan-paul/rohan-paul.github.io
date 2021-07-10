# Fundamentals of Multivariate Calculus for DataScience and Machine Learning

Multivariate Calculus is used all around Machine Learning and DataScience ecosystem, so having a first-principle understanding of it, is…

![](https://cdn-images-1.medium.com/max/1200/1*xVAkz19WN6Yhg3QeACRHHA.jpeg)

**Multivariate Calculus is used all around Machine Learning and DataScience ecosystem, so having a first-principle understanding of it, is incredibly useful when you are dealing with some complex Math equations in implementing some ML Algo.**

To start with, as soon as you need to implement multi-variate Linear Regression, you hit multivariate-calculus which is what you will have to use to derive the Gradient of a set of multi-variate Linear Equations i.e. Derivative of a Matrix. Beyond Optimization of Neural networks (NN) cost function and variations of gradient descent algorithm here in NN and a plethora of other optimization are handled with multivariate calculus.

So in this blog, I shall go over the fundamental concepts required multivariate-calculus which we will need to to understand many of the mechanisms of Machine Learning and Data Science

Why we need multivariate vector function in Machine-Learning

In machine learning when we have multiple independent variables to predict a dependent variable, we usually represent all independent variables in a multidimensional space.

For example,

Let’s consider an ML problem in which we have to predict the House Price using the following independent variables-

*   Total Carpet-Area
*   Number of Floors
*   Distance From Nearest Airport

Here, we need to project all those data points of students in a multidimensional space where-

**Dimension 1- Total Carpet-Area**

**Dimension 2 — Number of Floors**

**Dimension 3 — Distance From Nearest Airport**

Now, each independent variable can be represented as a vector with respect to the dependent variable.

Like,

“**Total Carpet-Area**” vector will have a magnitude and positive direction with respect to House Price (House Price will increase if carpet-area also increases and House Price will decrease if carpet-area decreases).

Similarly, “**Number of Floors”** will have a magnitude and positive direction with respect to House Price

However “**Distance From Nearest Airport**” vector will have a magnitude and negative direction with respect to House Price.

Now, if we have a new House data for which we need to predict the Price, then we can represent its data in these 3 vectors, and the point in which the 3 vectors having an orthogonal relationship can be considered as his/her predicted mark in the final examination.

> [Two **vectors** are considered to be **orthogonal**](https://onlinelibrary.wiley.com/doi/pdf/10.1002/cem.2816#:~:text=Two%20vectors%20are%20considered%20to,of%20elements%20in%20each%20vector.) to each other if they are at right angles in n‐dimensional space, where n is the size or number of elements in each **vector**.

### Functions of Several Variables

Mostly all of us are well aware of functions of a single (independent)  
variable. Many familiar quantities, however, are functions of two or more variables. For instance, the work done by a force (W = FD) and the volume of a right circular cylinder ( V =⫽Πr²h) are both functions of two variables. The volume of a rectangular solid (V = ⫽ lwh) is a function of three variables. The notation for a function of two or more variables is similar to that for a function of a single variable. Here are two examples.

![](https://cdn-images-1.medium.com/max/800/1*80lPn5I6YhaWrhafXztZRw.png)
undefined

Similar definitions can be given for functions of three, four, or n variables, where the domains consist of ordered triples (x1, x2, x3), quadruples (x1, x2, x3, x4), and n-tuples (x1, x2, . . . , xn). In all cases, the range is a set of real numbers.

#### Now here is a most general mathematical form of a multivariable function

![](https://cdn-images-1.medium.com/max/800/1*9LtanGx39n1lbRsXUe3Ocw.png)

So the function f above maps n input variables (x1, … to xn) to m output variables (g1, .. to gm). And each of the g1, g2, …. gm are called component functions. And also each of the g1, g2, … gm are really a function of input variables.

Using Mathematical notations

![](https://cdn-images-1.medium.com/max/800/1*ysl8C33WXMCPNIcO0v5-uQ.png)

Note that for a multivariable vector-valued function

![](https://cdn-images-1.medium.com/max/800/1*R6GbOFsLPZnWfbp7h0Q9rA.png)
undefined

the two numbers n and m can be either equal or unequal. For example, we could have functions F : R² → R² and G: R³ → R³

Some actual examples.

![](https://cdn-images-1.medium.com/max/800/1*i5W9fY9rae5FhmOFfLw5Sw.png)
undefined

Functions of several variables can be combined in the same ways as functions of single variables. For instance, you can form the sum, difference, product, and quotient of two functions of two variables as follows.

![](https://cdn-images-1.medium.com/max/800/1*PYERzC2stVRGIeyRpLH1Tg.png)
undefined

### Differentiation of Function of Several Variables

The derivative of a one-variable function measures its rate of change. Mathematically we already know from the first principle, the definition of a derivative of a single variable function definition as follows — Let D ⊆ R and let c be an interior point of D, that is, (c − r, c + r) ⊆ D for some r > 0 . A function f : D → R is said to be differentiable at c if the limit

![](https://cdn-images-1.medium.com/max/800/1*T-o5XZQGygs23uMBa1BETg.png)
undefined

exists. In this case the value of the limit is denoted by f ′ © and is called the  
derivative of f at c.

Now we see how a two-variable function has two rates of change: one as x changes (with y held constant) and one as y changes (with x held constant).

In general, if f is a function of two variables x and y, suppose we let only x vary while keeping y fixed, say y = b, where b is a constant. Then we are really considering a function of a single variable x.

We study the influence of x and y separately on the value of the function f (x, y) by holding one fixed and letting the other vary. This leads to the following definitions of Partial Derivatives of function f with respect to x and y.

For all points at which the limits exist, we define the partial derivatives at the point (a, b) by

![](https://cdn-images-1.medium.com/max/800/1*Sd-XHXb7vOSYHKf2YPKdvA.png)
undefined

Similarly, the partial derivative of f with respect to y at (a, b), is obtained by keeping x fixed (x = a ) and finding the ordinary derivative at b of the function.

![](https://cdn-images-1.medium.com/max/800/1*m5rEYkO9NuWYfr4LwahS5w.png)
undefined

**So, if f is a function of two variables, its partial derivatives are the functions fx and fy defined by**

![](https://cdn-images-1.medium.com/max/800/1*90EC4wycGihtq8XNH8pPaA.png)
undefined

The concept of a partial derivative can be extended naturally to functions of three or more variables. For instance, if **w = f (x, y, z)**, there are three partial derivatives, each of which is formed by holding two of the variables constant. That is, **to define the partial derivative of w with respect to x, consider y and z to be constant and differentiate with respect to x.** A similar process is used to find the derivatives of w with respect to y and with respect to z.

![](https://cdn-images-1.medium.com/max/800/1*hAd7pZPxqaoDe0uziN82CQ.png)

and it is found by treating y and z as constants and differentiating f(x, y, z) with respect to x. If w =f(x, y, z), then **fx = δw/δx** can be interpreted as the rate of change of w with respect to x when y and z are held fixed. But we can’t interpret it geometrically because the graph of f lies in four-dimensional space.

In general, if u is a function of n variables, u = f(x1, x 2, . . . , xn), its partial deriva­tive with respect to the i-th variable x\_i is

![](https://cdn-images-1.medium.com/max/800/1*7f-4L26yphQqxsaEtmNOyQ.png)
undefined

By extension, the second, third, and higher-order partial derivatives of a function of several variables, provided such derivatives exist, are defined as below.

1.  Differentiate twice with respect to x:

![](https://cdn-images-1.medium.com/max/800/1*sLUkwMo6RzZJHbi6Sbvn3Q.png)
undefined

2\. Differentiate twice with respect to y:

![](https://cdn-images-1.medium.com/max/800/1*OJevExZ4fNlsRt1cTqgZMg.png)
undefined

3\. Differentiate first with respect to x and then with respect to y:

![](https://cdn-images-1.medium.com/max/800/1*AjGmXrJpo2A9fCkU4hTUWw.png)
undefined

4\. Differentiate first with respect to y and then with respect to x:

![](https://cdn-images-1.medium.com/max/800/1*r5nFF_aQa5o2TnVOkygpxg.png)
undefined

The numbers 3 and 4 above are called mixed partial derivatives.

### Quick examples and interpretations of Partial Derivative of a Multivariate Function

#### Example-1

Find the partial derivatives fx and fy for the function

![](https://cdn-images-1.medium.com/max/800/1*imi5WCYgpgduv-At-LiY8Q.png)
undefined

**Solution — Considering** **y to be constant** and differentiating with respect to x produces

![](https://cdn-images-1.medium.com/max/800/1*p1NTDsmY987aqmhXTeMuJA.png)
undefined

**Considering x to be constant** and differentiating with respect to y produces

![](https://cdn-images-1.medium.com/max/800/1*AweNl8CNtJ1ivRTvezv85w.png)
undefined

#### Example-2

Find **δz/δx** and **δz/δy** if z is defined implicitly as a function of x and y by  
the equation

![](https://cdn-images-1.medium.com/max/800/1*ZLg4Ee7JzQiZh3-Xq-az7A.png)
undefined

To find **δz/δx**, first we differentiate implicitly with respect to x, treating y as a constant:

![](https://cdn-images-1.medium.com/max/800/1*WPNFcQoFc2BIDt9Em_CWNA.png)
undefined

Solving this equation for **δz/δx**, I get

![](https://cdn-images-1.medium.com/max/800/1*QbqeVmu1j-hp7IW8RKD3eg.png)
undefined

Similarly, differentiation with respect to y i.e. **δz/δy** treating x as constant gives

![](https://cdn-images-1.medium.com/max/800/1*Hlp8NN8EBoUqqAhkVkoJvQ.png)
undefined![](https://cdn-images-1.medium.com/max/800/1*xhEbur572Hrc2Nr09NrxRA.png)
undefined

### Geometry of Multivariate Function and Three-dimensional space

First, a quick refresher on plain-vanilla two-dimensional space, denoted by **R²**, is the familiar Cartesian plane. If we construct two perpendicular lines (the x- and y-coordinate axes), set the origin as the point of intersection of the axes, and establish numerical scales on these lines, then we may locate a point in **R²** by giving an ordered pair of numbers (x, y), the coordinates of the point. Note that the coordinate axes divide the plane into four quadrants.

For any multi-variable Calculus, we need to have some basic understanding of 3-dimensional space.

Three-dimensional space, denoted **R³**, requires three mutually perpendicular  
coordinate axes (called the x-, y- and z-axes) that meet in a single point (called the origin) in order to locate an arbitrary point. Analogous to the case of **R²**, if we establish scales on the axes, then we can locate a point in **R³** by giving an ordered triple of numbers (x, y, z). The coordinate axes divide three-dimensional space into eight octants. It takes some practice to get your sense of perspective correct when sketching points in **R³**.

**Plotting a multivariate function in three-dimensional space**

![](https://cdn-images-1.medium.com/max/800/1*GYuClUtUY5OQNT3jGCmTHg.png)

Imagine three coordinate axes meeting at the origin(0, 0, 0). A vertical axis (z), and two horizontal axes at right angles to each other (x and y). The xy-plane is horizontal, while the z-axis extends vertically above and below the plane. We generally use right-handed axes. This means that if you curl the fingers of your right hand from the positive x-axis to the positive y-axis, then your thumb will point along the positive z-axis.

![](https://cdn-images-1.medium.com/max/800/1*f6nzHo0pNZP2mvkF72DcoA.png)

We identify a point in 3-space by giving its coordinates (x, y, z) concerning these axes.

![](https://cdn-images-1.medium.com/max/800/1*efTNbGXOFT8NrwOZbc4B2w.png)
undefined

We may imagine a picture of a three-dimensional coordinate system in terms of a room. The origin is one of the corners at floor level where two walls meet the floor. The z-axis is the vertical intersection of the two walls; the x- and the y-axis are the intersections of each wall with the floor. Points with  
negative coordinates lie behind a wall in the next room or below the floor.

Now a quick example, **let's see how the graphs of the equations z = 0, z = 3, and z = −1 look like?**

![](https://cdn-images-1.medium.com/max/800/1*nZuvWX8LBxYAiZwAFMTFPg.png)

For graph z = 0, we visualize the set of points whose z-coordinate is zero. Means it must be at the same vertical level as the origin, i.e. in the horizontal plane containing the origin. So the graph of z = 0 is the middle plane in the above figure.

The graph of z = 3 is a plane parallel to the graph of z = 0, but three units above it. The graph of z = −1 is a plane parallel to the graph of z = 0, but one unit below it.

The plane z = 0 contains the x- and the y-coordinate axes, and is called the xy-plane. There are two other coordinate planes. The yz-plane contains both the y- and the z-axis, and the xz-plane contains the x- and the z-axis.

![](https://cdn-images-1.medium.com/max/800/1*4zAf0ztBDdOJhgCqSsSFsA.png)

### Graph of Univariate Function vs Multivariate Function

If _f_ is a scalar-valued function of a single variable, _f_:**R**→**R**f:R→R [(](https://mathinsight.org/function_notation "Function notation: A description of how we denote functions.")notation **R** stands for the real numbers and similarly, **R²** is a two-dimensional vector and also denotes a 2-D coordinate system[)](https://mathinsight.org/function_notation "Function notation: A description of how we denote functions."), then the graph of _f_ is the set of points (_x_, _f_(_x_)) for all _x_ in the domain of _f_. We call this the graph of _y_\=_f_(_x_) since the points are lying in the _xy_\-plane. When plotting the points in the _xy_\-plane, they typically form a curve, such as the graph of _f_(_x_)=x² shown below.

![](https://cdn-images-1.medium.com/max/800/1*6CNgxHuwp9bAKKiiUEoL7Q.png)
undefined

**But, the graphs of functions of two or more variables are examples of surfaces. That is, a set of points (_x_,_y_,_z_) that satisfy an equation relating all three variables is often a surface.**

The graph of a function f of two variables is the set of all points (x, y, z) for which z ⫽ f(x, y) and (x, y) is in the domain of f. This graph can be interpreted geometrically as a surface in space

Now, we define the graph of a scalar-valued function of two variables, _f_:**R²**→**R** in the same way. The graph is the set of points (_x_, _y_, _f_(_x_, _y_)) for all (_x_, _y_) in the domain of _f_. When often call this the graph of _z_\=_f_(_x_, _y_), since the points as lying in _xyz_\-space (instead of only xy-plane). The graph of this _f_(_x_,_y_) is a surface.

Let’s plot an example. You saw above the graph of the single variable function y=_x²_ is a parabola. Now extend it to make a multi-variate function.

_f_(_x_,_y_)= x² + y²

The graph of which is something called a paraboloid, a type of quadric surface.

![](https://cdn-images-1.medium.com/max/800/1*LLhjBmVJQcKZTXfPO25X6w.png)

### Now Geometric Interpretation of Partial Derivatives of a Multivariate Function

The partial derivatives of a function of two variables, z = f(x, y), have a useful geometric interpretation. If y =⫽ y0, then z ⫽ f (x, y0) represents the curve formed by intersecting the surface z = f(x, y) with the plane y = y0, as shown in below figure

![](https://cdn-images-1.medium.com/max/800/1*eAAAeJgQ6X0e6KsNleUTGQ.png)
undefined

Therefore,

![](https://cdn-images-1.medium.com/max/800/1*IM4flgS3z6fHnx5-gor1oA.png)
undefined

represents the slope of the curve given by the intersection of z = f(x, y) and the plane x = x0 at (x0, y0, f(x0, y0))

![](https://cdn-images-1.medium.com/max/800/1*W5AWV57f9FZUXtBtEQKUKw.png)
undefined

To discuss the above further, take a look at the following graph of a surface

![](https://cdn-images-1.medium.com/max/800/1*JQvdWxWoGV4e81h5G3RNNg.png)
undefined

To understand the geometric interpretation of partial derivatives, **we recall that the equation z =f (x,y) represents the surface S (which is the graph of f ).**

If f (a, b) = c, then the point P(a, b, c) lies on S. By fixing y = b, we are restricting our attention to the curve C1 in which the ver­tical plane y =b intersects S. (In other words, C1 is the trace of S in the plane y =b. Likewise, the vertical plane x = a intersects S in a curve C2. Both of the curves C1 and  
C2 passes through point P.

Now, note that the curve C1 is the graph of the function g(x) =f(x, b), so the slope of its tangent T1 at P is g′(a) = fx(a, b).

The curve C2 is the graph of the function G(y) = f(a, y), so the slope of its tangent T2 at P is G′(b) = fy(a,b).

**Thus the partial derivatives fx(a, b), and fy(a, b) can be interpreted geometrically as the slopes of the tangent lines at P(a, b, c) to the traces C1 and C2 of S in the planes y =b and x = a**

**Partial derivatives can also be interpreted as rates of change. If z =f(x, y), then δz/δx represents the rate of change of z with respect to x when y is fixed. Similarly, δz/δy represents the rate of change of z with respect to y when x is fixed**.

So to see a use case of the above implementation, lets take a quick look at the below graph of the surface z = f (x, y) and try to understand y whether each partial derivative is positive or negative.

![](https://cdn-images-1.medium.com/max/800/1*XwsGUBkNMsOymM1IuWdj_A.png)
undefined

**The positive x-axis points out of the page.** So imagine that I am heading off in this direction from the point marked P, where we descend steeply. So the partial derivative with respect to x is negative at P, with quite a large absolute value. The same is true for the partial derivative with respect to y at P since there is also a steep descent in the positive y-direction.

At the point marked Q, heading in the positive x-direction results in a gentle descent, whereas heading in the positive y-direction results in a gentle ascent.

Thus, the partial derivative fx at Q is negative but small (that is, near zero), and the partial derivative fy is positive but small. Basic Rules of Partial Differentiation

**One Quick Exercise**

Find the slopes in the x-direction and in the y-direction of the surface given by

![](https://cdn-images-1.medium.com/max/800/1*LYaFw2e83IfwLDsHNk7dUg.png)
undefined

**Solution**

The partial derivatives of f with respect to x and y are

![](https://cdn-images-1.medium.com/max/800/1*o1YTDJp4DrE8K6_ywc8ElQ.png)
undefined

So, in the x-direction, the slope is

![](https://cdn-images-1.medium.com/max/800/1*UENqZBTfmroMhfAKi6HqVQ.png)
undefined

and in the y-direction, the slope is

![](https://cdn-images-1.medium.com/max/800/1*v7qwvUgF7uCZIPv51513MQ.png)
undefined

Geometrically,

![](https://cdn-images-1.medium.com/max/800/1*uzeLx106vyC1m7Qwjw_1Ug.png)
undefined

### Differentiation Rules for Partial Derivatives

In the multivariate case, the basic differentiation rules that we know from high-school mathematics (e.g., sum rule, product rule, chain rule) still apply. **However, when we compute derivatives with respect to vectors x we need to pay attention: Our gradients now involve vectors and matrices, and matrix multiplication is not commutative** **, i.e., the order matters.**

![](https://cdn-images-1.medium.com/max/800/1*j2Dc0wSLWtbvQR9k5cKyXQ.png)
![](https://cdn-images-1.medium.com/max/800/1*2P91iMNp8We6AToR0XEcaw.png)

### Total Derivative of a Multivariate Function

A **total derivative** of a multivariable function of several variables, each of which is a function of another argument, is the derivative of the function with respect to said argument. it is equal to the sum of the [partial derivatives](https://math.wikia.org/wiki/Partial_derivative "Partial derivative") with respect to each variable times the derivative of that variable with respect to the [independent variable](https://math.wikia.org/wiki/Independent_variable "Independent variable").

In genearl Mathematical form, the total differential of three or more variables is defined as below. **For a function z = f(x, y, .. , u) the total differential is**

![](https://cdn-images-1.medium.com/max/800/1*UdlSeH53b-tDJamZ8-G5Tw.png)
undefined

For example, lets say, a function _f_(_x_,_y_,_z_) which is a continuous function of n variables x, y, z , with continuous partial derivatives ∂w/∂x, ∂w/∂y, ∂w/∂z. And assume x, y, z are differentiable functions x = x(t), y = y(t) , z = z(t). of a variable t. Then the **total derivative** of w with respect to t is given by

![](https://cdn-images-1.medium.com/max/800/1*I83XqVsMy3QxBGgQzLiIIA.png)
undefined

**Let's see a quick example — Find the total differential of the following function**

**w = x³yz + xy + z + 3 at a point (1,2,3)**

The total differential at the point (x0, y0, z0) is

![](https://cdn-images-1.medium.com/max/800/1*z0O9PhaiQPtQaZBcrm8NlQ.png)
![](https://cdn-images-1.medium.com/max/800/1*tmoN0SNrY37huB0DYRNysw.png)
undefined

Substituting the x, y, z values for the point (1, 2, 3) we get: wx(1, 2, 3) = 20, wy(1, 2, 3) = 4, wz(1, 2, 3) = 3 . So final ans is

![](https://cdn-images-1.medium.com/max/800/1*O65B-faTs3_zgx84lKhE_Q.png)
undefined

### Chain Rule of a univariate and multivariate function

#### First, the case when the Inner function is Univariate

![](https://cdn-images-1.medium.com/max/800/1*8yl3Fx7MeLh7AmzUcRuTBw.png)

We already have seen in the above example the implementation of the Chain Rule for the derivative of a composite of two functions

For simple composite function

f(x)=f(g(x))

The derivative is

![](https://cdn-images-1.medium.com/max/800/1*aQwVw0eNKEaHiiPS_eHRkA.png)
undefined

The chain rule says that we should take a derivative of an outside function, keep an inside function untouched, and then multiply everything by the derivative of the inside function.

Which in partial derivative form.

![](https://cdn-images-1.medium.com/max/800/1*yv1R7YlSh2UJ9pcBmFKTkg.png)
undefined

In above equation, both _f_(_x_) and _g_(_x_) are functions of one variable.

The chain rule is conceptually a divide and conquer strategy (like Quicksort) that breaks complicated expressions into subexpressions whose derivatives are easier to compute. Its power derives from the fact that we can process each simple subexpression in isolation yet still combine the intermediate results to get the correct overall result.

Consider a function f : R² → R of two variables x1, x2 . Where, x1(t) and x2(t) are themselves functions of t. To compute the gradient of f with respect to t, we have the chain rule for multivariate functions as below

![](https://cdn-images-1.medium.com/max/800/1*TjZ0v_2s1mdMmgKGfr8Emw.png)

where d denotes the gradient and ∂ partial derivatives.

**Example of the above,**

Consider the following function

![](https://cdn-images-1.medium.com/max/800/1*IXKvXOy1QKdN-KGDsUb7uw.png)
undefined

where x1 = sin t and x2 = cos t, then the corresponding derivative of f with respect to t is the following

![](https://cdn-images-1.medium.com/max/1200/1*QYrIug0xdYFN3vRCYrav4A.png)

#### And now the case when the inner function is multi-variate function.

**That is, If f (x1, x2 ) is a function of x1 and x2, where x1 (s, t) and x2(s, t) are themselves functions of two variables s and t,** meaning the function I want get the derivative of is

f(x1(s, t), x2(s, t))

**In this case, the chain rule yields the following partial derivatives**

![](https://cdn-images-1.medium.com/max/800/1*pAYyn2cEezZBXn-FMJ3AbQ.png)
undefined

Note in above set, we derived a total six partial derivative.

and the gradient is obtained by the matrix multiplication

![](https://cdn-images-1.medium.com/max/800/1*dBMIydZ9j_P8yOeINwMnaQ.png)
undefined

**Let’s see an example of this, i.e. inner function has Two Independent Variables**

Derive ∂_z_/∂_u_ and ∂_z_/∂_v_ using the following functions:

![](https://cdn-images-1.medium.com/max/800/1*piul1a-b6kG6WEAE_k-emA.png)
undefined

Where the inner functions are

![](https://cdn-images-1.medium.com/max/800/1*JeSe7fhgZvS4CnKAEPHP6w.png)
undefined

So following our earlier rule, to implement the chain rule for two variables, we need six partial derivatives — ∂_z_/∂_x_, ∂_z_/∂_y_, ∂_x_/∂_u_, ∂_x_/∂_v_, ∂_y_/∂_u_, and ∂_y_/∂_v_

![](https://cdn-images-1.medium.com/max/800/1*uRBB3h6CXVWtHxOaAnEwgA.png)
undefined

So I just put these above values to our Partial Derivative rule

![](https://cdn-images-1.medium.com/max/800/1*fIOF2OWsEnehLVqnQBqERA.png)
undefined

Next, we substitute _x_(_u_,_v_)=3_u_+2_v_ and _y_(_u_,_v_)=4_u_−_v_

![](https://cdn-images-1.medium.com/max/800/1*WpRYSlpg3-IEx0jgNUhZvA.png)
undefined

Similarly, repeat the above steps for ∂_z_/∂v

![](https://cdn-images-1.medium.com/max/800/1*2ghzJ_c3pbXOljBmiS9B_Q.png)
undefined![](https://cdn-images-1.medium.com/max/800/1*a3KuKlrv9qHxv8Y5_X4Xtw.png)
undefined

So we the final Partial Derivative of the above problem as below.

![](https://cdn-images-1.medium.com/max/800/1*eHhzPMk6fqPEvlVPDFwD6A.png)
undefined

### Gradient-Descent (GD) — the famous algorithm where Multivariate Calculus implementation is required

Without going into much detail of GD, as we know, like the derivative, the gradient represents the slope of a function.

> The gradient points in the direction of the largest rate of increase of the function, and its magnitude is the slope in that direction. And by the GD-algorithm, if we are at a particular value of θ (representing coefficients of the Linear Regression function) and if we want to move to a new value of θ such that the new loss is less than the current loss then we should move in the direction opposite to the gradient.

To understand and implement these gradients we need to return to partial derivatives (of the cost function), which we can reorganize into a row (i.e. horizontal) vector as below:

![](https://cdn-images-1.medium.com/max/800/1*BfmCJgd8dJKXt2Vte51IIA.png)
undefined

The above is what we call the gradient of f(x,y), or ∇ f(x,y)

That is we find the gradient of the function f with respect to x by varying one variable at a time and keeping the others constant. The gradient is then the collection of these partial derivatives.

So what it means is, if we have the gradient for function f(x,y) this is the same as writing the partial derivative of function f with respect to x and the partial derivative with respect to y:

The general Mathematical form will be as below.

For a function f(x), of n variables x1 , . . . , xn we define the partial derivatives as

![](https://cdn-images-1.medium.com/max/800/1*mHkZ2NJu2R15CPlGIvO6BQ.png)
undefined

and collect them in the row vector

![](https://cdn-images-1.medium.com/max/800/1*kVnMOVOP_5gpNX1WidWn0w.png)
undefined

where n is the number of variables

**Let's see an example, For f (x, y) = (x + 2y 3)², we obtain the below two partial derivatives using Chain Rule.**

![](https://cdn-images-1.medium.com/max/800/1*3ANjr6opm-VfgMZclbAxvw.png)
undefined![](https://cdn-images-1.medium.com/max/800/1*yBoaX_yPA492CGHpSCps5Q.png)
undefined

**And so now we get the gradient of the function_∇ f(x,y)_ by organizing these partials into a horizontal (row) vector form**

![](https://cdn-images-1.medium.com/max/1200/1*QWhDL14265lRVbAwXqQvOQ.png)

2(x+2y³) is the change in _f(x,y)_ with respect to a change in _x_, while 12(x+2y³)\*y² is the change in _f(x,y)_ with respect to a change in _y_.

### Difference between a Gradient and a Derivative

By now we have seen, the gradient **holds all the partial derivatives** of a multivariable function. Before we dive deep into Gradient-Descent algorithm below, let’s explore the difference between Derivative and Gradient.

1.  The derivative is a **number**, which shows the rate of change when some point in our function _moves_ in a particular direction. We can _visualize the derivative as a slope_ of a function which _goes along some direction on a graph_. We use the letter **d** to denote the derivative.
2.  The gradient is, on the other hand, a **vector** which _points_ in the direction of the **steepest ascent or** the greatest upward slope whose length is the directional derivative in that direction

We use the symbol ∇ to denote the gradient.

#### So why Gradient is a Vector

The regular derivative gives us the rate of change of a single variable. For example, df/dx tells us how much the function f(x) changes for a change in x. But if a function takes multiple variables, such as x and y, it will have multiple derivatives: the value of the function will change when we change x (df/dx) and also when we change y (df/dy).

So, now we can represent these multiple rates of change in a vector, with one component for each derivative. Thus, a function that takes 3 variables will have a gradient with 3 components each represented by a partial derivative. And just like the regular derivative, the gradient points in the direction of the greatest increase. However, now that we have multiple directions to consider (x, y, and z), the direction of greatest increase is no longer simply “forward” or “backward” along the x-axis, like it is with functions of a single variable.

If we have two variables, then our 2-component gradient can specify any direction on a plane. Likewise, with 3 variables, the gradient can specify and direction in 3D space to move to increase our function.

Now it will make sense of the **below definition of Gradient**. The gradient of vector-valued function

![](https://cdn-images-1.medium.com/max/800/1*T2J-SzMs2qsBGLMetk5nHA.png)
undefined

on real domain it is a row vector represented by a row-matrix

![](https://cdn-images-1.medium.com/max/800/1*lb18KrIgPu9Y2HPFD-v99A.png)
undefined

where N is the number of variables.

An example, for the following function

![](https://cdn-images-1.medium.com/max/800/1*Pu1Is4vJVEIHvTJx6InxUw.png)
undefined

the partial derivatives (i.e., the derivatives of f with respect to x1 and x2 ) are

![](https://cdn-images-1.medium.com/max/800/1*Stwwq9ESc73CTkPZWnEM1A.png)
undefined

and the gradient is then a matrix with one row (or a row-matrix as it is called)

![](https://cdn-images-1.medium.com/max/800/1*VOz61IbXjc-Lny9fHuL9Pw.png)
undefined

**Thank you for reading…**

#### References and sources for more reading on this topic

Total Derivative — [https://en.wikipedia.org/wiki/Total\_derivative](https://en.wikipedia.org/wiki/Total_derivative)

Matrix Calculus- [https://arxiv.org/pdf/1802.01528.pdf](https://arxiv.org/pdf/1802.01528.pdf)

Calculus — [https://ocw.mit.edu/resources/res-18-001-calculus-online-textbook-spring-2005/textbook/](https://ocw.mit.edu/resources/res-18-001-calculus-online-textbook-spring-2005/textbook/)

Calculus — [https://oer.galileo.usg.edu/mathematics-ancillary/15/](https://oer.galileo.usg.edu/mathematics-ancillary/15/)

Exercises in Calculus — [https://oer.galileo.usg.edu/mathematics-ancillary/15/](https://oer.galileo.usg.edu/mathematics-ancillary/15/)

By [Rohan Paul](https://medium.com/@paulrohan) on [September 26, 2020](https://medium.com/p/b2c7e83445ca).

[Canonical link](https://medium.com/@paulrohan/fundamentals-of-multivariate-calculus-for-datascience-and-machine-learning-b2c7e83445ca)

Exported from [Medium](https://medium.com) on December 12, 2020.