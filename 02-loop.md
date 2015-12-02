---
layout: page
title: Programming with Python
subtitle: Repeating Actions with Loops
minutes: ??
---
> ## Learning Objectives {.objectives}
>
> *   Explain what a for loop does.
> *   Correctly write for loops to repeat simple calculations.
> *   Trace changes to a loop variable as the loop runs.
> *   Trace changes to other variables as they are updated by a for loop.

In the last lesson,
we wrote some code that plots some values of interest from a small sequencing dataset.

We have a some more sets right now, though, and more on the way.
We want to create plots for all of our data sets with a single statement.
To do that, we'll have to teach the computer how to repeat things.

An example task that we might want to repeat is printing each character in a
word on a line of its own. One way to do this would be to use a series of `print` statements:

~~~ {.python}
word = 'GATTACA'
print word[0]
print word[1]
print word[2]
print word[3]
print word[4]
print word[5]
print word[6]
~~~
~~~ {.output}
G
A
T
T
A
C
A
~~~

This is a bad approach for two reasons:

1.  It doesn't scale:
    if we want to print the characters in a string that's hundreds of letters long,
    we'd be better off just typing them in.

1.  It's fragile:
    if we give it a longer string,
    it only prints part of the data,
    and if we give it a shorter one,
    it produces an error because we're asking for characters that don't exist.

~~~ {.python}
word = 'CAT'
print(word[0])
print(word[1])
print(word[2])
print(word[3])
~~~
~~~ {.output}
t
i
n
~~~
~~~ {.error}
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-27-1443d092a724> in <module>()
      3 print word[1]
      4 print word[2]
----> 5 print word[3]

IndexError: string index out of range

C
A
T
~~~


Here's a better approach:

~~~ {.python}
word = 'GATTACA'
for char in word:
    print char

~~~

~~~ {.output}
G
A
T
T
A
C
A
~~~

This is shorter---certainly shorter than something that prints every character in a hundred-letter string---and
more robust as well:

~~~ {.python}
word = 'CAT'
for char in word:
    print char
~~~

~~~ {.output}
C
A
T
~~~

The improved version of `print_characters` uses a [for loop](reference.html#for-loop)
to repeat an operation---in this case, printing---once for each thing in a collection.
The general form of a loop is:

~~~ {.python}
for variable in collection:
    do things with variable
~~~

We can call the [loop variable](reference.html#loop-variable) anything we like,
but there must be a colon at the end of the line starting the loop,
and we must indent anything we want to run inside the loop. Unlike many other languages, there is no
command to end a loop (e.g. end for); what is indented after the for statement belongs to the loop.

Here's another loop that repeatedly updates a variable. It determines the length of the quality string and the calculates the average quality.

~~~ {.python}
length = 0
qualsum = 0
qual = fq.fq2list('small.fq')[0][3]
for q in qual:
    length = length + 1
    qualsum = qualsum + ord(q) - 33
print 'There are', length, 'bases with average quality', qualsum/float(length)
~~~

~~~ {.output}
There are 99 bases with average quality 32.6666666667
~~~

It's worth tracing the execution of this little program step by step.
Since there are 99 quality scores,
the statement on line 3 will be executed 99 times.
The first time around,
`length` and `qualsum` are zero (the value assigned to it on line 1)
and `q` is `'@'`.
The statement adds `ord('@') - 33 = 31` to the old value of `qualsum`,
producing 31,
and updates `qualsum` to refer to that new value.
The next time around,
`q` is `'@'` again and `length` is 1,
so `length` is updated to be 2.
After 97 more updates,
`length` is 99;
since there is nothing left in `qual` for Python to process,
the loop finishes
and the `print` statement on the last line tells us our final answer.

Note that a loop variable is just a variable that's being used to record progress in a loop.
It still exists after the loop is over,
and we can re-use variables previously defined as loop variables as well:

~~~ {.python}
base = 'U'
print 'Before loop, base is', base
for base in 'ACGT':
    print base
print 'After loop, base is', base
~~~

~~~ {.output}
Before loop, base is U
A
C
G
T
After loop, base is T
~~~

Note that `len`, which we have encountered in lesson 1, is much faster than any function we could write ourselves,
and much easier to read than a multi-line loop;
it will also give us the length of many other things that we haven't met yet,
so we should always use it when we can.


> ## From 1 to N {.challenge}
>
> Python has a built-in function called `range` that creates a sequence of numbers. Range can
> accept 1-3 parameters. If one parameter is input, range creates an array of that length,
> starting at zero and incrementing by 1. If 2 parameters are input, range starts at
> the first and ends at the second, incrementing by one. If range is passed 3 parameters,
> it stars at the first one, ends at the second one, and increments by the third one. For
> example,
> `range(3)` produces the numbers 0, 1, 2, while `range(2, 5)` produces 2, 3, 4,
> and `range(3, 10, 3)` produces 3, 6, 9.
> Using `range`,
> write a loop that uses `range` to print the first 3 natural numbers:
>
> ~~~ {.python}
> 1
> 2
> 3
> ~~~

> ## Computing powers with loops {.challenge}
>
> Exponentiation is built into Python:
>
> ~~~ {.python}
> print(5**3)
> 125
> ~~~
>
> Write a loop that calculates the same result as `5 ** 3` using
> multiplication (and without exponentiation).

> ## Reverse a string {.challenge}
>
> Write a loop that takes a string,
> and produces a new string with the characters in reverse order,
> so `'Newton'` becomes `'notweN'`.
