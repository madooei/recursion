---
title: "Conclusion"
teaching: 10
exercises: 15
questions:
- "What did we learn in this lesson?"
objectives:
- "To conclude this lesson."
keypoints:
- "You can do really cool things with recursion!"
use_math: true
---

> ## Activity
> At the end of this lesson, make sure you are able
> * To describe what a recursive method is and the benefits of using recursion.
> * To develop recursive methods for recursive mathematical functions.
> * To explain how recursive method calls are handled in a call stack.
> * To solve problems using recursion.
> * To solve the Tower of Hanoi problem using recursion.
{: .checklist}

A recursive method is one that invokes itself. For a recursive 
method to terminate, there must be one or more base cases.

A recursive solution is one that solves a _subproblem_ that is a 
**smaller instance** of the **same problem**, and then uses the 
solution to that smaller instance to solve the original problem.

Recursion is an alternative form of program control. It is essentially repetition without a loop control. 
It can be used to write elegant, minimal, and clear solution for inherently recursive 
problems that would otherwise be difficult to solve.

Recursion often leads to code that is easier to write, understand and analyze formally.

In some cases, recursion can also be less efficient:
* more functional calls and stack operations 	
* Running out of stack space leads to failure deep recursion 

> ## Interesting and useful resources
> * [Wikipedia: Induction](https://en.wikipedia.org/wiki/Mathematical_induction)
> * [Wikipedia: Primitive recursive function](https://en.wikipedia.org/wiki/Primitive_recursive_function)
> * [Youtube: The Most Difficult Program to Compute? - Computerphile](https://youtu.be/i7sm9dzFtEI)
> * [Wikipedia: Ackermann function](https://en.wikipedia.org/wiki/Ackermann_function)
>
> These are related but beyond the scope of this lesson.
{: .callout}


And finally, these resource are related but beyond the scope of this lesson:
