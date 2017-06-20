---
title: "Fibonacci Sequence"
teaching: 10
exercises: 15
questions:
- "How recursion can be used to find each term in the Fibonacci sequence?"
objectives:
- "Define the Fibonacci sequence using a recurrence relation."
- "Implement a recursive function to find each term in the Fibonacci sequence."
keypoints:
- "A recursive definition is the most natural way to define a recurrence relation."
use_math: true
---

When it comes to recursive programming, a classic example is computing the Fibonacci sequence:

$$
0,\; 1,\; 1,\; 2,\; 3,\; 5,\; 8,\; 13,\; 21,\; 34,\; 55,\; \dots
$$

In the Fibonacci sequence, each number is found by adding up the two numbers before it, except for the first two terms 

> ## FIBONACCI (1170-1250) 
> Fibonacci was one of the greatest European mathematicians of the Middle Ages (born in the city of Pisa--now in Italy). 
> He introduced the European world to Arabic notation for numerals and algorithms for arithmetic.
>
> The Fibonacci sequence considers the growth of an idealized (biologically unrealistic) rabbit population.
> A single pair of rabbits are not fertile during their first month of life but thereafter give birth to one new male/female pair at the end of every month. 
> Assuming, we begin a year with a single pair, and no rabbits die, how many rabbits will there be at the end of the year?
>
> The Fibonacci sequence turns up in surprising places in the nature and in Mathematics. Read more on [Wikipedia](https://en.wikipedia.org/wiki/Fibonacci_number)
{: .callout}

> ## Sequences
> A sequence is an ordered list of values.
> For example the sequence of powers of two: $1,\; 2,\; 4,\; 8,\; 16,\; \dots$
>
> We can think of a sequence as a _function_ from the set of natural numbers $N=\{0,\; 1,\; 2,\; 3,\; \dots \}$ into the values in the sequence.
> For the sequence of powers of two, we can consider the function $f$ such that $f(0)=1$, $f(1)=2$, $f(2)=4$, $f(3)=8$, $\dots$ 
>
> When a function is specified as a sequence, using subscripts to denote the input to the function is more common.
> For the sequence of powers of two, we can use $f_k$ instead of $f(k)$, so $f_0=1$, $f_1=2$, $f_2 =4$, and so on.
{: .callout}

> ## Explicit Form
> A sequence can be specified by an _explicit formula_ showing how the value of _term_ $f_k$ depends on $k$. 
> For the sequence of powers of two: $f_k = 2^k$ for $k\geq 0$. 
{: .callout}

## Recurrence Relation
Some sequences are most naturally defined by specifying one or more initial terms and then giving a rule for determining 
subsequent terms from earlier terms in the sequence. 

A rule that defines a term $f_k$ as a function of previous terms in the sequence is called a __recurrence relation__. 
Recursive definition are the most natural way to define recurrence relations.

We can define the Fibonacci sequence using the following recurrence relation:

$$
f_k=\left\{\begin{matrix}
0 & k = 0\\ 
1 & k = 1\\ 
f_{k-1}+f_{k-2} & k \geq 2 
\end{matrix}\right.
$$

Let's implement this function:

~~~
# POST: return value is the n-th
#       Fibonacci number F(n)
def fib(n):
    if n==0:
        return 0
    elif n==1:
        return 1
    else:
        return fib(n-1) + fib(n-2)
~~~
{: .python}


~~~
# Print the first 10 terms in Fibonacci sequence
for n in range(11):
    print(fib(n))
~~~
{: .python}

~~~
0
1
1
2
3
5
8
13
21
34
55
~~~
{: .output}

Here are two interesting facts about `fib(n)`:
* It has two base cases, and
* makes two recursive calls upon each invocation.

> ## Activity
>
> Can you implement a non-recursive version of `fib(n)`?
>
> > ## Solution
> > ~~~
> > def fib(n):
> >    a, b = 0, 1
> >    for i in range(0, n):
> >         a, b = b, a + b
> >    return a
> > ~~~
> > {: .python}
> {: .solution}
{: .challenge}


> ## Challenge
>
> Can you implement a non-iterative version of `fib(n)`?
>
> Hint: Look up the _closed-form_ expression for finding the Fibonacci number [here](https://en.wikipedia.org/wiki/Fibonacci_number#Closed-form_expression_).
>
> > ## Solution
> > ~~~
> > def fib(n):
> >    phi = (1 + 5**(1/2)) / 2
> >    numerator = phi**n - ((-phi)**(-n)) 
> >    denominator = 2*phi - 1
> >    return  int(numerator/denominator)
> > ~~~
> > {: .python}
> {: .solution}
{: .challenge}