# Discrete vs Continuous Probability Distributions in context of Data Science

First lets define some terms for clarity

![](https://cdn-images-1.medium.com/max/1200/1*t7lx34vYoJ8lAeNU6un7_g.jpeg)

First lets define some terms for clarity

**The sample space Ω**  
The sample space is the set of all possible outcomes of the experiment,  
usually denoted by Ω. For example, two successive coin tosses have  
a sample space of {hh, tt, ht, th}, where “h” denotes “heads” and “t”  
denotes “tails”.

**The event space A**  
The event space is the space of potential results of the experiment. A  
subset A of the sample space Ω is in the event space A if at the end  
of the experiment we can observe whether a particular outcome ω ∈ Ω  
is in A. The event space A is obtained by considering the collection of  
subsets of Ω, and for discrete probability distributions (Section 6.2.1)  
A is often the power set of Ω.

**The probability P**  
With each event A ∈ A, we associate a number P (A) that measures the  
probability or degree of belief that the event will occur. P (A) is called  
the probability of A.

The probability of a single event must lie in the interval \[0, 1\], and the  
total probability over all outcomes in the sample space Ω must be 1, i.e.,  
P (Ω) = 1. Given a probability space (Ω, A, P ), we want to use it to model  
some real-world phenomenon. In machine learning, we often avoid explic-  
itly referring to the probability space, but instead refer to probabilities on  
quantities of interest, which **we denote by T as the target space** and refer to elements of of T as states.

The term `probability` relates is to an `event` and `probability distribution` relates is to a `random variable`.

It is a _convention_ that the term `probability mass function` refers to the `probability distribution` of a `discrete random variable` and the term `probability density function` refers to the probability function of a `continuous random variable`.

### Understanding Probability Density

First a quick reference on PMF, PDF and CDF

![](https://cdn-images-1.medium.com/max/800/1*7zNkRF94SG1EiluoEuYgsQ.png)
undefined

In order to understand the heart of modern probability, we need to extend the concept of integration from basic calculus.  
To begin, let us consider the following piecewise function

![](https://cdn-images-1.medium.com/max/800/1*_ya9XYPF52QGPwpWKODN4Q.png)
undefined

Applying the fundamental Riemann integration of Calculus we get

![](https://cdn-images-1.medium.com/max/800/1*hNkbryuHYSnG2_EATqcdwQ.png)
undefined

which has the usual interpretation as the area of the two rectangles that make up f (x).

The question is given f (x) = 1, what is the set of x values for which this is true? For our example, this is true whenever x ∈ (0, 1\]. So now we have a correspondence between the values of the function (namely, 1 and 2) and the sets of x values for which this is true, namely, {(0, 1\]} and {(1, 2\]}, respectively. To compute the integral, we simply take the function values  
(i.e., 1,2) and some way of measuring the size of the corresponding interval.

Since areas can be defined by definite integrals, we can also define the probability of an event occuring within an interval \[_a_, _b_\] by the definite integral

![](https://cdn-images-1.medium.com/max/800/1*rtJplzySaA6mBcXjZDSSzg.png)
undefined

where _f_(_x_) is called the probability density function (pdf).

A function _f_(_x_) is called a **probability density function** if

1.  _f_(_x_)≥0 for all _x_
2.  The area under the graph of _f_(_x_) over all the real line is exactly 1
3.  The probability that _x_ is in the interval \[_a_, _b_\] is

![](https://cdn-images-1.medium.com/max/800/1*3ecCpHUQgljVnjYIUCaqiA.png)
undefined

i.e. the area under the graph of _f_(_x_) from _a_ to _b_.

In the problem above, the probability density function _f_(_x_) is called a **uniform (flat) probability density function (pdf)**.

> So fundamentally, what does a probability density at point 𝒙 mean?

> Probability density function’s value at some specific point does not give you probability; it is a measure of how dense the distribution is around that value. It means how much probability is concentrated per unit length (**d**𝒙) near 𝒙, or how dense the probability is near 𝒙.

**For discrete random variables, we look up the value of a PMF at a single point to find its probability P(𝐗=𝒙)   
For continuous random variables, we take an integral of a PDF over a certain interval** to find its probability that **X** will fall in that interval.

### Discrete Random Variable

#### First what is a Random Variable

Given a random experiment with sample space S,a **random variable** X is a set function that assigns one and only one real number to each element s that belongs in the sample space S.

The set of all possible values of the random variable X, denoted x, I am calling here as the **support**, or **space**, of X.

Note that the capital letters at the end of the alphabet, such as W,X,Y, and Z typically represent the definition of the random variable. The corresponding lowercase letters, such as w,x,y, and z, represent the random variable’s possible values.

#### And now what is a Discrete Random Variable

By a discrete random variable, it is meant a function (or a mapping), say X, from a sample space Ω, into the set of real numbers. Symbolically, if ω ∈Ω, then X (ω ) = x, where x is a real number.

A random variable X is a **discrete random variable** if:

*   there are a finite number of possible outcomes of X, or
*   there are a countably infinite number of possible outcomes of X.

**A countably infinite number of possible outcomes means that there is a one-to-one correspondence between the outcomes and the set of integers.**

No such one-to-one correspondence exists for an uncountably infinite number of possible outcomes.

**For a value x of the set of possible outcomes of the random variable X , i.e., x ∈ T , p(x) denotes the probability that random variable X has the outcome x.**

**For discrete random variables, this is written as P (X = x), which is known as the probability mass function. The pmf is often referred to as the distribution”. For continuous variables, p(x) is called the probability density function (often referred to as a density).**

When we say probability distribution it may pertain to a discrete random variable or a continuous random variable, depending on the context.

When the random variable is discrete, probability distribution means, how the total probability is distributed over various possible values of the random variable. Consider the experiment of tossing two unbiased coins simultaneously. Then, sample space _S_ associated with this experiment is:

_S_ \= {_HH_,_HT_,_TH_,_TT_} 

**If we define a random variable X as: the number of heads on this sample space _S_,** then we will have

_X_(_HH_)=2,

_X_(_HT_)=_X_(_TH_)=1,

_X_(_TT_)=0

X(HH)=2,

X(HT)=X(TH)=1,

X(TT)=0.

The probability distribution of _X_X is then given by

![](https://cdn-images-1.medium.com/max/800/1*QJJTQx59XTQY1e-tnoz1sQ.png)
undefined

For a discrete random variable, we consider events of the type {_X_\=_x_} and compute probabilities of such events to describe the distribution of the random variable.

> The Probability Mass Function of a Discrete Random Variable expresses the probability of the variable being equal to each specific value in the range of all potential discrete values defi ned.The sum of these probabilities over all possible values equals 100%.

In mathematical form, the probability that a discrete random variable X takes on a particular value x, that is, P(X=x), is frequently denoted f(x). The function f(x) is typically called the **probability mass function**

Let _X_ be a discrete random variable with possible values denoted _x_1, _x_2, _xi_, x1, x2, xi,…. The probability mass function of _X_, denoted _p_

![](https://cdn-images-1.medium.com/max/800/1*lZ-QmL4hLdAujjGHkXkwvA.png)
undefined

The same above in more general mathematical form, the **probability mass function**, P(X=x)=f(x), of a discrete random variable X is a function that satisfies the following properties:

![](https://cdn-images-1.medium.com/max/800/1*tm9HWK4w0DlbQJ0iYCRfKA.png)
undefined

First item basically says that, for every element x in the support S, all of the probabilities must be positive. Note that if x does not belong in the support S, then f(x)=0. The second item basically says that if you add up the probabilities for all of the possible x values in the support S, then the sum must equal 1. And, the third item says to determine the probability associated with the event A, you just sum up the probabilities of the x values in A.

Since f(x) is a function, it can be presented:

*   in tabular form
*   in graphical form
*   as a formula

#### Some more daily life examples of discrete random variables

If a random variable can take only a finite number of discrete values, then it is  
discrete.

A fair die is a small cube with a natural number from 1 to 6 engraved on each side equally spaced without repetition. The fairness means that a die is made so that its weight is equally spread and, thus, all six faces are equally likely to face when rolled. So, if rolled, the set of numbers { 1,2,3,4,5,6} is the sample space of this experiment.

![](https://cdn-images-1.medium.com/max/800/1*0XijFoykyVdrAOI53HwRYA.jpeg)
undefined

Now let’s consider the experiment of rolling a pair of fair dice. Then, the set of  
possible outcomes, that is, the sample space Ω, contains 36 pairs.

![](https://cdn-images-1.medium.com/max/800/1*sEVOqPUFOAlnQlmURLO9_g.png)
undefined

In each pair, the first element represents the number appearing on one die and the second appearing on the other. We can define a discrete random variable X such that it assigns numbers 1 through 36 to the ordered pairs in Ω from the beginning to the end, respectively, as follows:

![](https://cdn-images-1.medium.com/max/800/1*Wze4Qip1oz4nJsPvMtq_4w.png)
undefined![](https://cdn-images-1.medium.com/max/800/1*Po_F9IzZ8Ap9z_l_q7Wh9A.png)
undefined

Now an actual Python implemention in the below Jupyter Notebook

undefined
undefined

### Continuous Random Variables

A **continuous random variable** differs from a discrete random variable in that it takes on an uncountably infinite number of possible outcomes.

While for a **discrete random variable** X that takes on a finite or countably infinite number of possible values, we determined P(X=x) for all of the possible values of X, and called it the probability mass function (“p.m.f.”). For **continuous random variables**, the probability that X takes on any particular value x is 0. That is, finding P(X=x) for a continuous random variable X is not going to work. Instead, we’ll need to find the probability that X falls in some interval (a,b), that is, we’ll need to find P(a<X<b). We’ll do that using a probability density function (“p.d.f.”).

> The Probability Density Function of a Continuous Random Variable expresses  
> the rate of change in the probability distribution over the range of potential continuous values defined, and expresses the relative likelihood of getting one value in comparison with another.

A nondiscrete random variable X is said to be absolutely continuous, or simply continuous, if its distribution function may be represented as

![](https://cdn-images-1.medium.com/max/800/1*iCWF6FmzF4KLSLQ1VbfdTA.png)
undefined

where the function f (x) has the properties

![](https://cdn-images-1.medium.com/max/800/1*wFsTXGID52aBtXJRq1z0oQ.png)
undefined

It follows from the above that if X is a continuous random variable, then the probability that X takes on any one particular value is zero, whereas the interval probability that X lies between two different values, say, a and b,  
is given by

![](https://cdn-images-1.medium.com/max/800/1*BhniKwIhdw1mSbksK-eIhw.png)
undefined

A function f (x) that satisfies the above requirements is called a probability function or probability distribution for a continuous random variable, but it is more often called a probability density function or simply density function. Any function f (x) satisfying Properties 1 and 2 above will automatically be a density function, and required probabilities can then be obtained from the more general form below

#### Probability Density Function

A function f : RD → R is called a probability density function (pdf ) if  
1\. ∀x ∈ RD : f (x) > 0  
2\. Its integral exists and

![](https://cdn-images-1.medium.com/max/800/1*xIKEeVWI1Y0NIMydbDWwfA.png)
undefined

So observe that the probability density function is any function f that is  
non-negative and integrates to one. And as stated above, we associate a random variable X with this function f by

![](https://cdn-images-1.medium.com/max/800/1*wC4r2WAlFIcFVHnstV3iEw.png)
undefined

As you can see, the definition for the p.d.f. of a continuous random variable differs from the definition for the p.m.f. of a discrete random variable by simply changing the summations that appeared in the discrete case to integrals in the continuous case.

Now at the start of this article we discussed how density histogram (representing frequency) is defined so that the area of each rectangle equals the relative frequency of the corresponding class, and the area of the entire histogram equals 1. That suggests then that finding the probability that a **continuous random variable X** falls in some interval of values involves finding the area under the curve f(x) sandwiched by the endpoints of the interval.

So from a large sample space of Pizza, the probability that a randomly selected Pizza weighs between 0.20 and 0.30 pounds is then this area: (which is what the definite Integral formulae above calculates )

![](https://cdn-images-1.medium.com/max/800/1*GIVV3LjatDxwZxg54pZ0sw.png)
undefined

**Some examples of well known discrete probability distributions include:**

*   Poisson distribution.
*   Bernoulli and binomial distributions.
*   Multinoulli and multinomial distributions.
*   Discrete uniform distribution.
*   The Geometric Distribution
*   The Negative-Binomial Distribution
*   The Hypergeometric Distribution

Some examples of common domains with well-known discrete probability distributions include:

*   The probabilities of dice rolls form a discrete uniform distribution.
*   The probabilities of coin flips form a Bernoulli distribution.
*   The probabilities car colors form a multinomial distribution.

A quick summary

![](https://cdn-images-1.medium.com/max/800/1*51lPbnyMX69R6neu-v8aVA.png)
undefined

Now lets see an simple actual exmaple of Discrete Probability Distribution. Quickly revisit the definition

_The_ **probability distribution** _of a discrete random variable_ _X_ _is a list of each possible value of_ _X_ _together with the probability that_ _X_ _takes that value in one trial of the experiment._

I start with a simple experiment, tossing a fair coin 10 times, and measured how many successes/heads I observe. I can use the number of successes (heads) observed in many ways to understand the basics of probability. For example, I could simply count how many times we see 0 heads, 1 head, 2 heads with our fair coin toss, and so on. Or here, I am just denoting the outcome with ‘H’ or ‘T’ for each experiment.

Now a quick and simple Math example of PDF

Let X be a continuous random variable whose probability density function is:

![](https://cdn-images-1.medium.com/max/800/1*ACqeGOBJ9Tp876tvG77V1g.png)
undefined

First, note again that

![](https://cdn-images-1.medium.com/max/800/1*OyKli-Zap_CJ8o0iES70AQ.png)
undefined

For example,

![](https://cdn-images-1.medium.com/max/800/1*IN-5vBbVjoTW695ZKFkYUA.png)
undefined

which is clearly not a probability! In the continuous case, f(x) is instead the height of the curve at X=x, so that the total area under the curve is 1. In the continuous case, it is areas under the curve that define the probabilities.

**What is P(X=1/2)?**

It is a straightforward integration to see that the probability is 0:

![](https://cdn-images-1.medium.com/max/800/1*lu13KX55uEZnsFclkeQzkw.png)
undefined

In general, if X is continuous, the probability that X takes on any specific value x is 0. That is, when X is continuous, P(X=x)=0 for all x in the support.

An implication of the fact that **P(X=x)=0 for all x when X is continuous** is that you can be less precise about the endpoints of intervals when finding probabilities of continuous random variables. That is:

![](https://cdn-images-1.medium.com/max/800/1*LfGN-ap1IlNA6lodEqi7Uw.png)
undefined

for any constants a and b.

**Further explanation of the above principle**

The probability of observing any single value of the continuous random variable is 0 since the number of possible outcomes of a continuous random variable is uncountable and infinite. That is, for a continuous random variable, we must calculate a probability over an interval rather than at a particular point. This is why the probability for a continuous random variable can be interpreted as an area under the curve on an interval. In other words, we cannot describe the probability distribution of a continuous random variable by giving probability of single values of the random variable as we did for a discrete random variable. This property can also be seen from the fact that

![](https://cdn-images-1.medium.com/max/800/1*hXhSKswuVhJtYxpiatobMA.png)
undefined

for any real **c**

### Why do I need to Integrate over the PDF to get the Probaility

In the case of of continuous random variable, we should not ask for the probability that **_X_** is exactly a single number (since that probability is zero). Instead, we need to think about the probability that **x** is close to a single number.

We capture the notion of being close to a number with a _probability density function_ which is normally denoted by **P(_x_)**. If the probability density around a point **x** is large, that means the random variable **X** is likely to be close to **_x_**. If, on the other hand, **P(_x_)=0** in some interval, then **_X_** won’t be in that interval.

So building on the Integration concept of Calculus

If the probability of **X** being exactly at point 𝒙 is zero, how about an extremely small interval around the point **𝒙**? Say, **\[𝒙, 𝒙+d𝒙\]?**

Let’s assume **d𝒙** is infinitesimally small with a value of 0.00000000001.

Then the probability that **X** will fall in **\[𝒙, 𝒙+d𝒙\]** is the **Area** under the curve **f(𝒙)** sandwiched by **\[𝒙, 𝒙+d𝒙\]**.

**The Area Under a Curve — Integral Calculus Basics**

The area under a curve between two points can be found by doing a definite integral between the two points. To find the area under the curve y = f(x) between x = a and x = b, integrate y = f(x) between the limits of a and b.

To translate the probability density P(x) into a probability, imagine that _Ix_ is some small interval around the point x. Then, assuming P is continuous, the probability that _X_ is in that interval will depend both on the density P(_x_) and the length of the interval

P ( _X_ ∈ _Ix_) ≈ P (_x_) × Length of _Ix_

We don’t have a true equality here, because the density P may vary over the interval Ix. But, the approximation becomes better and better as the interval Ix shrinks around the point x, as P will be come closer and closer to a constant inside that small interval. The probability P ( X ∈ Ix ) approaches zero as Ix shrinks down to an infinitesemally small value to the point x (consistent with our above result for single numbers), but the information about X is contained in the rate that this probability goes to zero as Ix shrinks.

So, to determine the probability that X is in any subset A of the real numbers, we simply add up the values of P(x) in the subset. By “add up,” we mean integrate the function P(x) over the set A.

### Cumulative Distribution Function

> **The Cumulative Distribution Function of a Discrete Random Variable** expresses the theoretical or observed probability of that variable being less than or equal to any given value. **It equates to the sum of the probabilities** of achieving that value and each successive lower value.

#### Example of the Cumulative Distribution Function for Rolling a Single Die

![](https://cdn-images-1.medium.com/max/800/1*4SFG0KbHXD508qm0pxd92Q.png)
undefined

And now the same for Continuous Random Variable

> **The Cumulative Distribution Function of a Continuous Random Variable**  
> expresses the theoretical or observed probability of that variable being less than or equal to any given value. **It equates to the area under the Probability Density Function curve** to the left of the value in question.

Now implementing some very basic PDF with Python and Scipy

undefined
undefined

Another Jupyter Notebook to undersand how PDF is different from Probability

undefined
undefined

By [Rohan Paul](https://medium.com/@paulrohan) on [August 27, 2020](https://medium.com/p/e48c7d40bc0f).

[Canonical link](https://medium.com/@paulrohan/discrete-vs-continuous-probability-distributions-in-context-of-data-science-e48c7d40bc0f)

Exported from [Medium](https://medium.com) on December 12, 2020.