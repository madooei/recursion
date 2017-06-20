---
title: "Factorial Function"
teaching: 10
exercises: 5
questions:
- "What is a recursive function definition?"
objectives:
- "Understand the mechanics of recursion through a recursive implementation of the factorial function."
keypoints:
- "We have a __new way of thinking__ about how to compute the value of $n!$."
- In this new way, we solve the __subproblem__ of computing $(n-1)!$, multiply this result by $n$, and declare $n!$ equal to the result of this product.
- When we're computing $n!$ in this way, we are using __recursion__.
use_math: true
---

To demonstrate the mechanics of recursion, I begin with a simple mathematical example of computing the value of the factorial function. 

The factorial of a non-negative integer $n$, denoted $n!$, is defined as the product of all positive integers less than or equal to $n$. 

For example: $5! = 5\times 4 \times 3 \times 2 \times 1 = 120$. 

Let's put this into a more concrete mathematical notation:

$$
n!=\prod_{k=1}^n k
$$

where $\prod$ is the *product* notation (similar to $\sum$ for summation).

> ## Where do we use the factorial function?
> Factorial function is useful when we're trying to count how many different orders there are for things or how many different ways we can combine things. 
>
> Example: How many flags, with 3 horizontal stripes, can be made if you can choose from 3 different colours (every colour can only be used once)?
>
> Answer: For the first stripe you can choose from 3 colours, for the second stripe you can choose from 2 colours and for the last stripe you can have only the 1 colour that is left: $3 \times 2 \times 1 = 6 = 3!$.
{: .callout}

Note that $0!$ is defined as $1$ (in the same way that $x^0 = 1$ so that the laws of exponents work).

We've seen an implementation of factorial using _for loop_:

~~~
# factorial function
def factorial(n):
    prod = 1
    for i in range(2, n+1):
        prod *= i
    return prod
~~~
{: .python}


~~~
# print n! for n={0,1,2,3,4,5}
for n in range(6):
    print("{}! = {}".format(n,factorial(n)))
~~~
{: .python}

~~~
0! = 1
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
~~~
{: .output}

We make the following observation: $5! = 5\times (4 \times 3 \times 2 \times 1) = 5\times 4!$.

This is true for any __positive__ integer $n$: you can compute the factorial function on $n$ by first computing the factorial function on $n-1$. 

Let's formalize this observation; I can rewrite the definition of factorial function as

$$
n! = f(n)=\left\{\begin{matrix}
1 & n = 0\\ 
n\times f(n-1) & n \geq 1 
\end{matrix}\right.
$$

This is called a **recursive definition** because the function definition is written in terms of itself; The adjective "recursive" (from the Latin verb "__recurrere__"), means "to run back". And this is what a recursive definition or a recursive function does: it is "running back" or returning to itself. Note that there is no circularity in this definition, because each time the function refers to itself, its argument is smaller by one ($n-1$), until it reaches $n=0$ where no further recursions are made.

> ## Recursive Definition
> Recursive definition of a function defines values of that functions for some inputs in terms of the values of the same function for other inputs. 
{: .callout}

So now we have __another way of thinking__ about how to compute the value of $n!$, for all nonnegative integers $n$:

* If $n = 0$, then declare that $n! = 1$.
* Otherwise, $n$ must be positive. Solve the _subproblem_ of computing $(n-1)!$, multiply this result by $n$, and declare $n!$ equal to the result of this product.

> ## Subproblem
> We say that computing $(n-1)!$ is a subproblem that we solve to compute $n!$.
{: .callout}

When we're computing $n!$ in this way, we call the first case ($n=0$), where we immediately know the answer, the __base case__, and we call the second case ($n \geq 1$), where we have to compute the same function but on a different value, the __recursive case__.


~~~
# Recursive implementation of the factorial function
def factorial(n):
    if n == 0:   # base case
        return 1
    else:        # recursive case
        return n * factorial(n - 1)  
~~~
{: .python}


~~~
# print n! for n={0,1,2,3,4,5}
for n in range(6):
    print("{}! = {}".format(n,factorial(n)))
~~~
{: .python}

~~~
0! = 1
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
~~~
{: .output}

Note that the implementation of `factorial(n)` does not use any explicit loops. Repetition is provided by the repeated recursive invocations of the function.  

> ## Discussion
>
> What will happen if we remove the base case from the implementation of `factorial(n)`?
>
>~~~
> def factorial(n):
> 	return n * factorial(n - 1)  
>~~~
>{: .python}
{: .discussion}