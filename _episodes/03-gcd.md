---
title: "Greatest Common Divisor"
teaching: 15
exercises: 15
questions:
- "How can recursion be used to find the greatest common divisor of two positive integers?"
objectives:
- "Understand the problem of finding the greatest common divisor."
- Explain why the direct method is too slow.
- Describe the alternative faster Euclid algorithm.
- Implement Euclid method using recursion.
keypoints:
- "Recursion is good for solving the problems that are inherently recursive like the Euclid method for finding greatest common divisor."
use_math: true
---

Reclusive definitions are especially useful in the design of algorithms.

The design and analysis of algorithms is the core subject matter of Computer Science.

> ## Algorithm
> An algorithm is a sequence of _unambiguous_ instructions for solving a problem, that is, for obtaining a required output for any _legitimate_ input in a _finite_ amount of time.
{: .callout}

> ## Design and Analysis of Algorithms
> We want two things from a computer algorithm: 
> * Given an input to a problem, it should always produce a __correct solution__ to the problem, and
> * it should use computational resources __efficiently__ while doing so.
>
> Often there are different solutions (algorithms) to a problem. 
{: .callout} 

Let's see an example of this.

## Greatest Common Divisor

For two (positive) integers, $a$ and $b$, 
their greatest common divisor or $gcd(a,b)$ is the largest integer $d$ 
so that $d$ divides both $a$ and $b$.

Okay! how can we compute it? 

Well, we can find all the divisors of $a$, say by testing all the numbers from $2$ to $a$, 
and all the divisors of $b$. Then, we pick the largest common divisor.

Example: $gcd(258,60)=?$
* divisors of $258$ are $1,\; 2,\; 3,\; 6,\; 86,\; 129,\; 258$
* divisors of $60$ are $1,\; 2,\; 3,\; 4,\; 5,\; 6,\; 10,\; 12,\; 15,\; 20,\; 30,\; 60$
* Accordingly, $gcd(258, 60) = 6$

> ## Exercise
>
> What is the $gcd(10,4)$?
>
> > ## Solution
> > $gcd(10,4)=2$
> {: .solution}
>
> What is the $gcd(375,234)$?
>
> > ## Solution
> > $gcd(375,234)=3$
> {: .solution}
{: .challenge}

It is not easy to come up with all the factors (divisors) 
of numbers such as 375 and 234. But we have computers! 
Let computer, compute this for us! 

Here is an idea: let's have the computer to go through all the numbers from 1 to 234 
(the minimum of 234 and 375) and check for every number if it divides both 234 and 375.
Throughout this process, we have the computer to store and update the largest of the common divisors. 

Here is an implementation of this naive solution:

~~~
# PRE: a and b are non-negative integers
# POST: return value is the 
#       Greatest Common Divisor of a and b
# Direct Method
def gcd(a,b):
    d_max = 1
    for d in range(1,min(a,b)+1):
        if a%d==0 and b%d==0 and d>d_max:
            d_max = d
    return d_max
~~~
{: .python}

A slightly more efficient implementation of this idea is:

~~~
# PRE: a and b are non-negative integers
# POST: return value is the 
#       Greatest Common Divisor of a and b
# Direct Method: More efficient
def gcd(a,b):
    for d in range(min(a,b),1,-1):
        if a%d==0 and b%d==0:
            return d
~~~
{: .python}

~~~
# Let's test our method on some inputs
print("gcd(375,234)={}".format(gcd(375,234)))
print("gcd(10,4)={}".format(gcd(10,4)))
print("gcd(258,60)={}".format(gcd(258,60)))
print("gcd(3918848,1653264)={}".format(gcd(3918848,1653264)))
~~~
{: .python}

~~~
gcd(375,234)=3
gcd(10,4)=2
gcd(258,60)=6
gcd(3918848,1653264)=61232
~~~
{: .output}


To find the $gcd(375,234)$ the for loop will iterate (at most) $234$ times.

> ## Activity
> 
> How many times the loop will iterate for $gcd(3918848,1653264)$?
>
> > ## Solution
> > 1,653,264 times
> {: .solution}
>
> How many times it will iterate if we input two numbers with 10 digits?
>
> > ## Solution
> > $10^{10} times$
> {: .solution}
>
> How many times it will iterate if we input two numbers with 20 digits?
>
> > ## Solution
> > $10^{20} times$
> {: .solution}
>
> A typical computer handles $10^9$ operations per second. 
> How long it will take for our solution to find the answer given two numbers 
> each with 20 digits.
>
> > ## Solution
> > $\frac{10^{20}}{10^9}=10^{11}$ seconds.
> > That's approximately 3,168 years!
> {: .solution}
{: .challenge}

If you've worked through the activity, you know that our solution does not scale well for large inputs.
	
## GCD: is there a better solution?

Let's make an observation: Suppose $a'$ is the remainder when $a$ is divided by $b$, then

$$
gcd(a,b)=gcd(a',b)=gcd(b,a)
$$

Example: $gcd(10,4)=gcd(2,4)=gcd(4,2)=2$

> ## Challenge
> Prove our observation is true.
>
> Hint: Suppose $a=bq+a'$ for some integer $q$. Then, $d$ divides $a$ and $b$ if and only if $d$ divides $a'$ and $b$.
{: .challenge}

This observation will lead to a very neat solution: suppose we want to compute $gcd(3918848,1653264)$.
We replace the largest number with the remainder of their division, that is, instead of the original 
problem, we solve $gcd(1653264,612320)$. But wait a minute! we can use this trick again:

$$
\begin{align}
& gcd(3918848,1653264) \\
&= gcd(1653264,612320) \\
&= gcd(612320,428624) \\
&= gcd(428624,18369) \\
&= gcd(18369,61232) \\
&= gcd(61232,0) = 61232
\end{align}
$$

Note $gcd(61232,0)=61232$ because the greatest common divisor of any number and zero is that number. 

So, we have a new algorithm which reduced our computation from (potentially) 1,653,264 steps to 6 steps!
Each step reduces the size of numbers by about a factor of 2.

~~~
# PRE: a and b are non-negative integers
# POST: return value is the 
#       Greatest Common Divisor of a and b
# Euclid Method: Recursive version
def gcd(a,b):
    if b==0: 
        return a
    else:
        return gcd(b, a%b)
~~~
{: .python}


~~~
# Let's test our method on some inputs
print("gcd(375,234)={}".format(gcd(375,234)))
print("gcd(10,4)={}".format(gcd(10,4)))
print("gcd(258,60)={}".format(gcd(258,60)))
print("gcd(3918848,1653264)={}".format(gcd(3918848,1653264)))
~~~
{: .python}

~~~
gcd(375,234)=3
gcd(10,4)=2
gcd(258,60)=6
gcd(3918848,1653264)=61232
~~~
{: .output}

> ## Pause here ...
> ... and make sure you understand the recursive implementation of `gcd` function. 
{: .discussion}

The previously describe technique to find GCD is known as the Euclidean algorithm (Euclid method).

> ## Euclid (323-283 BCE)
> Euclid of Alexandria was a Greek mathematician who is known as the "father of geometry". 
> Among his work, the book titled "Elements" is considered as 
> one of the most influential works in the history of mathematics. 
> Read more about Euclid on [Wikipedia](https://en.wikipedia.org/wiki/Euclid)
{: .callout} 


> ## Challenge
>
> Can you implement a non-recursive version of Euclid algorithm?
>
> > ## Solution
> > ~~~
> > # PRE: a and b are non-negative integers
> > # POST: return value is the 
> > #       Greatest Common Divisor of a and b
> > # Euclid Method: Iterative version
> > def gcd(a,b):
> >     while b!=0:
> >         c = b
> >         b = a%b
> >         a = c
> >     return a
> > ~~~
> > {: .python}
> {: .solution}
{: .challenge}

> ## Interesting and useful resources
> * [Khan Academy: Recursive Algorithms](https://www.khanacademy.org/computing/computer-science/algorithms#recursive-algorithms)
{: .callout}