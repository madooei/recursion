---
title: "Towers of Hanoi"
teaching: 20
exercises: 40
questions:
- "How recursive thinking can help us to solve Towers of Hanoi?"
objectives:
- "Develop a recursive solution to the puzzle."
- Describe the time (number of moves) it takes to solve the puzzle as a function of $n$ (number of disks).
keypoints:
- "Recursion is the essential technique that is by far the easiest way to explain how to solve this puzzle."
use_math: true
---

You are given a set of three pegs and $n$ disks, with each disk a different size. 
Let's name the pegs $A$, $B$, and $C$, and let's number the disks from 1, 
the smallest disk, to $n$, the largest disk. 
At the outset, all $n$ disks are on peg $A$. 
The goal is to move all $n$ disks from peg $A$ to peg $B$:


![](https://s3.amazonaws.com/ka-cs-algorithms/hanoi-5-init.png){:width="400px"}


Sounds easy, right? It's not quite so simple, because you have to obey two rules:
* You may move only one disk at a time.
* No disk may ever rest atop a smaller disk. 
For example, if disk 3 is on a peg, 
then all disks below disk 3 must have numbers greater than 3.

> ## Towers of Hanoi
> In 1883 a French mathematician, Edouard Lucas, invented the puzzle of The
> Tower of Hanoi. The original puzzle had eight disks. 
> The directions to the puzzle claimed it was based on an old Indian legend:
> > _On the steps of the altar in the temple of Benares, for many, many years Brahmins_
> > _have been moving a tower of 64 golden disks from one pole to another;_ 
> > _one by one, never placing a larger on top of a smaller._
> > _When all the disks have been transferred the Tower and the Brahmins will fall,_
> > _and it will be the end of the world._
> 
> Humm! If the Brahmins monks are correct, should we be panicking in the streets?
{: .callout}

You might be wondering why we discuss recursion in the context of Towers of Hanoi puzzle. 
Because recursion is the essential technique that is by far the easiest way to 
explain how to solve the puzzle. 

> ## Activity
> Before going on and spoiling the fun, try yourself 
> to think of a way to define the pattern you use to solve the Towers of Hanoi puzzle.
>
> I recommend trying to solve the puzzle for $n=1$, $n=2$ and $n=3$ before you proceed with this module. 
{: .challenge}

Okay, now please watch this short but excellent excerpt from Prof. Eric Grimson's lecture at MIT. 
<iframe width="560" height="315" src="https://www.youtube.com/embed/rVPuzFYlfYE" frameborder="0" allowfullscreen></iframe>
If the video is not loading here, please refer to this [link](https://youtu.be/rVPuzFYlfYE) to open it on Youtube.

At this point, you might have picked up two patterns. First, you can solve the Towers of Hanoi problem recursively. 
If $n = 1$, just move disk 1. Otherwise, when $n \geq 2$, solve the problem in three steps:
1. Recursively solve the _subproblem_ of moving disks 1 through $n-1$ from whichever peg they start on to the spare peg.
2. Move disk $n$ from the peg it starts on to the peg it's supposed to end up on.
3. Recursively solve the _subproblem_ of moving disks 1 through $n-1$ from the spare peg to the peg they're supposed to end up on.

Let us implement this idea.

~~~
# Helper function to print out the moves
def printMove(source, destination):
    print("Move disc from {} to {}".format(source, destination))

# Recursive solution to the Towers of Hanoi puzzle
def moveDisks(n, source, destination, spare):
    if n==1:   # Base case
        printMove(source, destination)
    else:      # Recursive case
        moveDisks(n-1, source, spare, destination);  # Step 1 above
        moveDisks(1, source, destination, spare);    # Step 1 above
        moveDisks(n-1, spare, destination, source);  # Step 1 above
~~~
{: .python}

~~~
# Let's test our method for n=3 disks
moveDisks(3, "peg A", "peg B", "peg C");
~~~
{: .python}

~~~
Move disc from peg A to peg B
Move disc from peg A to peg C
Move disc from peg B to peg C
Move disc from peg A to peg B
Move disc from peg C to peg A
Move disc from peg C to peg B
Move disc from peg A to peg B
~~~
{: .output}

I leave it to you to verify the answer. 

## How long the world will last?

So let's ask the most important question to be asked: 
if the Brahmins monks use our program, how long will the world last?

To answer this, we make the observation that when $n=3$ disks, 
it took us 7 moves to solve the puzzle. 
Assume it will take Brahmins monks a second to make each move and they work 24/7 all year round!
How long does it take to solve the puzzle for $n=64$?

Let's look at the size of the solution as the number of disks grows.

> ## Activity
> 
> Try the code and see how many moves does it take to solve the puzzle when:
>
> $n = 1$
>
> > ## Solution
> > 1 move
> {: .solution}
>
> $n = 2$
>
> > ## Solution
> > 3 moves
> {: .solution}
>
> $n = 3$
>
> > ## Solution
> > 7 move
> {: .solution}
>
> $n = 4$
>
> > ## Solution
> > 15 moves
> {: .solution}
>
> $n = 5$
>
> > ## Solution
> > 31 move
> {: .solution}
>
> $n = 6$
>
> > ## Solution
> > 63 moves
> {: .solution}
>
> Can you see a pattern here? Can you write the size of the solution as a function of $n$?
>
> > ## Solution
> > $2^{n}-1$ moves
> {: .solution}
{: .challenge}

So the solution almost doubles in size as we increase the number of disks by one. 
Concretely, solving a problem for $n$ disks requires $2^n - 1$ moves.

Let's see if we can prove this _hypothesis_ (our educated guess from observations) to be correct.

Suppose $n=1$, then the solution consist of $2^1 - 1=1$ moves 
which is consistent with our observation (i.e., our formula works for the base case of $n=1$).
Now, assume you can solve the puzzle for $n-1$ disks in $2^{n-1} - 1$ moves, 
then you can solve the puzzle for $n$ disks in $2^n - 1$ moves: 
you need $2^{n-1} - 1$ moves to recursively solve the first subproblem of moving 
disks 1 through $n-1$ to the spare peg. Then you need one move to move disk n to its destination. 
Finally, you need another $2^{n-1} - 1$ moves to recursively solve the 
second subproblem of moving disks 1 through $n-1$ from the spare peg to the destination.
If you add up the moves, you get $2^n - 1$:

$$
(2^{n-1} - 1) + 1 + (2^{n-1} - 1) = 2\times(2^{n-1}) - 1 = 2^n - 1
$$

Do you fancy a more formal notation? We can use recurrence relation to describe the 
time (number of moves) it takes to solve the puzzle as a function of $n$ (number of disks):

$$
T(n)=\left\{\begin{matrix}
1 & n = 1\\ 
2T(n-1)+1 & n \geq 1 
\end{matrix}\right.
$$

In fact, you didn't need to run the code over different values of $n$ and count the number of moves. 
You could start by noting $T(n=1)=1$, then $T(n=2)=2\times T(n=1)+1 = 3$, moving on $T(3)=2\times T(2)+1 = 7$ and so on.

> ## Mathematical Induction
> The method of proof which I have used above is known as _Mathematical Induction_. 
> It is a very useful technique to prove recurrence relations and to analysis recursive algorithms.
{: .callout}

> ## Principle of Mathematical Induction:
>
> Let P be a property of positive integers such that:
> 1. Basis Step: P(1) is true, and
> 2. Inductive Step: if P(n) is true, then P(n+1) is true.
> Then P(n) is true for all positive integers.
>
> Mathematical Induction is beyond the scope of this lesson but you can read more about it on [Wikipedia](https://en.wikipedia.org/wiki/Mathematical_induction).
{: .callout}

Back to the monks. 
They're using $n = 64$ and so they will need to move a disk $2^{64} - 1$ times. 
We assumed they can make a move per second, so how long is $2^{64} - 1$ seconds? 
Using a rough estimate, that comes to $584$ billion years! 
The sun has only about another five to seven billion years left before it goes all 
supernova on us. 
So, yes, the world will end, but it will happen long before the monks 
can get all 64 disks to its destination.

> ## Challenge
>
> Can you implement a non-recursive function to solve the Towers of Hanoi puzzle?
>
> > ## Solution
> > ~~~
> > # It's not a challenge if I do it for you :)
> > ~~~
> > {: .python}
> {: .solution}
{: .challenge}


> ## Thinking Recursively
> The Towers of Hanoi puzzle is a classic problem that demonstrates 
> how one must _think recursively_ in order to solve a problem.
>
> Here are some guidelines:
> 1. Identify and solve the (usually simple) base case(s).
> 2. Determine how to break the problem down into smaller problems of the same kind.
> 3. Call the function recursively to solve the smaller problems.
>	* Assume it works. Do not think about how!
> 4. Use the solutions to the smaller problems to solve the original problem.
{: .callout}