---
title: "Constructing a Recursive Solution"
teaching: 20
exercises: 0
questions:
- "How does a recursive solution gets constructed and executed?"
objectives:
- "Understand the mechanics of constructing a recursive solution, the way it is handled by the computer, in partiular the memory overhead."
keypoints:
- "If a problem can be reduced to smaller instances of the same problem, then a recursive solution is a natural way to solve it."
- When writing a recursive function definition, always confirm that the function will not produce infinite recursion.
- Recursion bears substantial overhead. Each time the program calls a method, the system creates an activation frame and places it on the _stack_. This can consume considerable memory and requires extra time to manage the memory.
use_math: true
---

We have seen a number of examples that employed recursion. It is time to dive deeper. 
Let us remind ourself of what recursion is:

* Algorithmically: a way to design solutions to problems by __divide-and-conquer__.

* Semantically: a programming technique where a __function calls itself__.

> ## Divide and Conquer
> Divide and conquer is an algorithm design paradigm based on multi-branched recursion. 
> It works by recursively breaking down (_reducing_) a problem into (two or more)
> sub-problems of the same (or related type), until these become simple enough 
> to be solved directly. 
> The solutions to the sub-problems are then combined to give a solution to 
> the original problem.
{: .callout}

We can distill the idea of recursion into two simple rules:
* Each recursive call should be on a smaller instance of the same problem, 
that is, a smaller subproblem.
* The recursive calls must eventually reach a base case, 
which is solved without further recursion.

A good way to remember these rules is to think about 
[Matryoshka doll](https://en.wikipedia.org/wiki/Matryoshka_doll) 
(Russian nested dolls).

![](https://giffysnap.com/thewonderforest/wp-content/uploads/2014/08/PZYvwLxOTbX.gif){:width="400px"}

You can see that each doll encloses all a smaller dolls 
(analogous to the recursive case), until the smallest doll that does 
not enclose any others (like the base case).

> ## Pitfall
> If you forget to include a base case, or your recursive cases fail to eventually 
> reach a base case, then 
> __infinite recursion__ happens. 
> Infinite recursion  is a special case of an _infinite loop_ 
> when a recursive function fails to stop recursing.
{: .discussion}

## Recursion Trace

The execution of a recursive function is usually illustrated using a __recursion trace__. 

Below is an example recursive trace for `factorial(4)`:

![]({{ page.root }}/assets/img/2017-06-18_11h20_02.png){:width="400px"}

* Each new recursive function call is indicated by a downward arrow to a new invocation. 
* When the function returns, an arrow showing this return is drawn and the return value may be indicated alongside this arrow. 

The recursion trace outlines the python's execution of the recursion. 

> ## Activation Record
> In Python, each time a function (recursive or otherwise) is called, a structure known as an **activation record** or **frame** is created 
to store information about the progress of that invocation of the function. 
>
> This activation record includes mechanism for storing the function call parameters and local variables among other things.
{: .callout}

> ## Call Stack
> Computers use a structure called a __stack__ to store information about activation records.
> A stack is a last-in/first-out memory structure: 
> the last item placed is the first that can be removed.
{: .callout}

When the execution of a function leads to a nested function call, 
the execution of the former call is _suspended_ and 
its activation record stores the place in the source code at 
which the flow of control should continue upon return of the nested
call. 

This process is used both in the standard case of one function calling a 
different function, or in the recursive case in which a function invokes itself.

To see how this works, walk through the steps of computing `factorial(4)`

<iframe
	align="middle"
	width="100%"
	height="400"
	src="https://pythontutor.com/iframe-embed.html#code=def%20factorial%28n%29%3A%0A%20%20%20%20if%20n%20%3D%3D%200%3A%20%20%20%23%20base%20case%0A%20%20%20%20%20%20%20%20return%201%0A%20%20%20%20else%3A%20%20%20%20%20%20%20%20%23%20recursive%20case%0A%20%20%20%20%20%20%20%20return%20n%20%2A%20factorial%28n%20-%201%29%20%20%0A%0Aprint%28factorial%284%29%29&origin=opt-frontend.js&cumulative=false&heapPrimitives=false&textReferences=false&py=3&rawInputLstJSON=%5B%5D&curInstr=0&codeDivWidth=350&codeDivHeight=400"
	frameborder="0"
	allowfullscreen
></iframe>

If the embedded program is not loaded here, click on this [link](src="https://pythontutor.com/iframe-embed.html#code=def%20factorial%28n%29%3A%0A%20%20%20%20if%20n%20%3D%3D%200%3A%20%20%20%23%20base%20case%0A%20%20%20%20%20%20%20%20return%201%0A%20%20%20%20else%3A%20%20%20%20%20%20%20%20%23%20recursive%20case%0A%20%20%20%20%20%20%20%20return%20n%20%2A%20factorial%28n%20-%201%29%20%20%0A%0Aprint%28factorial%284%29%29&origin=opt-frontend.js&cumulative=false&heapPrimitives=false&textReferences=false&py=3&rawInputLstJSON=%5B%5D&curInstr=0&codeDivWidth=350&codeDivHeight=400") 
to open it in a new tab.
 

> ## StackOverflowError
> Because each recursive call causes an activation record to be placed on the stack,
> infinite recursion can force the stack to grow beyond its limits to 
> accommodate all the activation frames required.
> The result is a stack overflow.
> A stack overflow causes abnormal termination of the program.
>
> In Python, the following error is `StackOverflowError`
>
> `RecursionError: maximum recursion depth exceeded`
{: .callout}


> ## Tail Recursion
> A tail recursive function is one where every recursive call is the 
> last thing done by the function before returning.
> In other words, there is nothing to do after the function returns 
> except return its value.
>
> For example, the `gcd` function is tail-recursive. 
> In contrast, the `factorial` function is not tail-recursive.
>
> Some programming languages treats tail-recursive calls as _jumps_ rather than 
> function calls and are able to execute using constant space. 
> In that case, the program is essentially iterative, 
> equivalent to using imperative language control structures like the "for" 
> and "while" loops.
>
> Python does __not__ optimize tail recursive function.
{: .callout}

## Recursion Tree
A _recursion tree_ is useful for visualizing what happens when a recurrence is iterated. 
It diagrams the tree of recursive calls (and the amount of work done at each call).

Recursion tree is specially useful when the function makes more than one recursive calls. 
For instance, the `fib` function which implements the Fibonacci sequence, makes two recursive calls 
upon each invocation of the function. Here is the recursion tree for `fib(4)`.

![]({{ page.root }}/assets/img/recursion_tree2.png){:width="800px"}

If you pay close attention to the figure, you will recognize a problem with the 
`fib` function: some function calls are repeated multiple times. 
It means some computations are done multiple times. This gets worse as input $n$ grows.

This example illustrates that recursive methods can take more time and consumes 
more memory than iteration, and thus lead to inefficient solutions. 

> ## Interesting and useful resources
> * [Youtube: EXTRA BITS: Recursion and the Stack - Computerphile](https://youtu.be/0pncNKHj-Sc)
> * [Wikipedia: Recursion (Computer Science)](https://en.wikipedia.org/wiki/Recursion_(computer_science))
{: .callout}