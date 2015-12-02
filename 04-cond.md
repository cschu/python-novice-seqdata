---
layout: page
title: Programming with Python
subtitle: Making Choices
minutes: ??
---
> ## Learning Objectives {.objectives}
>
> *   Explain the similarities and differences between tuples and lists.
> *   Write conditional statements including `if`, `elif`, and `else` branches.
> *   Correctly evaluate expressions containing `and` and `or`.

One of the typical quality control checks for sequencing data is the base content analysis and especially the GC content analysis. How can we use Python to count this?
In this lesson, we'll learn how to write code that
runs only when certain conditions are true.

## Conditionals

We can ask Python to take different actions, depending on a condition, with an if statement:

~~~ {.python}
number = 37
if number > 100:
    print number, 'greater 100'
else:
    print number, 'not greater 100'
print 'done'
~~~
~~~ {.output}
37 not greater 100
done
~~~

The second line of this code uses the keyword `if` to tell Python that we want to make a choice.
If the test that follows the `if` statement is true,
the body of the `if`
(i.e., the lines indented underneath it) are executed.
If the test is false,
the body of the `else` is executed instead.
Only one or the other is ever executed:

![Executing a Conditional](fig/python-flowchart-conditional.svg)\

Conditional statements don't have to include an `else`.
If there isn't one,
Python simply does nothing if the test is false:

~~~ {.python}
num = 53
print 'before conditional...'
if num > 100:
    print '53 is greater than 100'
print '...after conditional'
~~~
~~~ {.output}
before conditional...
...after conditional
~~~

We can also chain several tests together using `elif`,
which is short for "else if".
The following Python code uses `elif` to print the sign of a number.

~~~ {.python}
number = -3
if number > 0:
    print number, 'is positive'
elif number == 0:
    print number, 'is zero'
else:
    print number, 'is negative'
~~~
~~~ {.output}
-3 is negative
~~~

One important thing to notice in the code above is that we use a double equals sign `==` to test for equality
rather than a single equals sign
because the latter is used to mean assignment.

We can also combine tests using `and` and `or`.
`and` is only true if both parts are true:

~~~ {.python}
if (1 > 0) and (-1 > 0):
    print 'both true'
else:
    print 'at least one part is not true'
~~~
~~~ {.output}
at least one part is not true
~~~

while `or` is true if at least one part is true:

~~~ {.python}
if (1 > 0) or (-1 > 0):
    print 'at least one part is true'
else:
    print 'both false'  
~~~
~~~ {.output}
at least one part is true
~~~


~~~ {.python}
xna = 'ACCGTA'
if 'U' in xna:
    print xna, 'is an RNA sequence'
else:
    print xna, 'is a DNA sequence'
~~~
~~~ {.output}
ACCGTA is a DNA sequence
~~~


## Checking our Data

Now that we've seen how conditionals work,
we can use them to count the individual bases in our reads.

~~~ {.python}
seqs = fq.fq2list('small.fq')
base_composition = []
n_sequences = 0
for base in seqs[0][1]:
    base_composition.append([0,0,0,0,0])

for read in seqs:
    n_sequences = n_sequences + 1
    seq = read[1]
    for i, base in enumerate(seq):
        if base == 'A':
            base_composition[i][0] += 1
        elif base == 'C':
            base_composition[i][1] += 1
        elif base == 'G':
            base_composition[i][2] += 1
        elif base == 'U' or base == 'T':
            base_composition[i][3] += 1
        else:
            base_composition[i][4] += 1

for i, base in enumerate('ACGTN'):
    print base,
    for pos in base_composition[:10]:
        print pos[i],
    print
~~~
~~~ {.output}
A 5 1 4 1 4 4 0 2 3 3
C 1 1 2 0 2 1 3 4 1 2
G 2 3 1 4 2 2 1 2 2 3
T 2 5 3 5 2 3 6 2 4 2
N 0 0 0 0 0 0 0 0 0 0
~~~

In this way,
we have asked Python to do something different depending on the condition of our data.

Now we can calculate the frequencies from the absolute counts:

~~~ {.python}
for i, base in enumerate('ACGTN'):
    print base,
    for pos in base_composition[:10]:
        print pos[i] / float(n_sequences),
    print
~~~

~~~ {.output}
A 0.5 0.1 0.4 0.1 0.4 0.4 0.0 0.2 0.3 0.3
C 0.1 0.1 0.2 0.0 0.2 0.1 0.3 0.4 0.1 0.2
G 0.2 0.3 0.1 0.4 0.2 0.2 0.1 0.2 0.2 0.3
T 0.2 0.5 0.3 0.5 0.2 0.3 0.6 0.2 0.4 0.2
N 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0
~~~

And with this we can plot the base composition per position:

~~~ {.python}
y = []
for base in 'ACGTN':
    y.append([])

for i, base in enumerate('ACGTN'):
    for j, pos in enumerate(base_composition):
        y[i].append(pos[i] / float(n_sequences))

x = range(len(seq))
for i, base in enumerate('ACGTN'):
    plt.plot(x, y[i], linewidth=0.5)
~~~

Finally, we plot the GC content per read. Here, we could use the data from before, but will use the in-built `string.count(character)` function that we already used in lesson 1.

~~~ {.python}
import matplotlib.mlab as mlab
gc_per_read = []

for read in fq.fq2list('small.fq'):
    nGC = read[1].count('G') + read[1].count('C')
    gc_per_read.append(float(nGC) / len(read[1]))

for i, gc in enumerate(gc_per_read[:10]):
    print i, gc


n, bins, patches = plt.hist(gc_per_read, 20, normed=1, facecolor='green', alpha=0.2)
y = mlab.normpdf(bins, np.mean(gc_per_read), np.std(gc_per_read))
l = plt.plot(bins, y, 'r--', linewidth=2)
~~~
~~~ {.output}
0 0.40404040404
1 0.383838383838
2 0.464646464646
3 0.464646464646
4 0.383838383838
5 0.40404040404
6 0.414141414141
7 0.393939393939
8 0.313131313131
9 0.282828282828
~~~


> ## How many paths? {.challenge}
>
> Which of the following would be printed if you were to run this code? Why did you pick this answer?
>
> 1.  A
> 2.  B
> 3.  C
> 4.  B and C
>
> ~~~ {.python}
> if 4 > 5:
>     print('A')
> elif 4 == 5:
>     print('B')
> elif 4 < 5:
>     print('C')
> ~~~

> ## What is truth? {.challenge}
>
> `True` and `False` are special words in Python called `booleans` which represent true
and false statements. However, they aren't the only values in Python that are true and false.
> In fact, *any* value can be used in an `if` or `elif`.
> After reading and running the code below,
> explain what the rule is for which values are considered true and which are considered false.
>
> ~~~ {.python}
> if '':
>     print('empty string is true')
> if 'word':
>     print('word is true')
> if []:
>     print('empty list is true')
> if [1, 2, 3]:
>     print('non-empty list is true')
> if 0:
>     print('zero is true')
> if 1:
>     print('one is true')
> ~~~

> ## Close enough {.challenge}
>
> Write some conditions that print `True` if the variable `a` is within 10% of the variable `b`
> and `False` otherwise.
> Compare your implementation with your partner's:
> do you get the same answer for all possible pairs of numbers?


> ## In-place operators {.challenge}
>
> Python (and most other languages in the C family) provides [in-place operators](reference.html#in-place-operator)
> that work like this:
>
> ~~~ {.python}
> x = 1  # original value
> x += 1 # add one to x, assigning result back to x
> x *= 3 # multiply x by 3
> print(x)
> ~~~
> ~~~ {.output}
> 6
> ~~~
>
> Write some code that sums the positive and negative numbers in a list separately,
> using in-place operators.
> Do you think the result is more or less readable than writing the same without in-place operators?

> ## Tuples and exchanges {.challenge}
>
> Explain what the overall effect of this code is:
>
> ~~~ {.python}
> left = 'L'
> right = 'R'
>
> temp = left
> left = right
> right = temp
> ~~~
>
> Compare it to:
>
> ~~~ {.python}
> left, right = right, left
> ~~~
>
> Do they always do the same thing?
> Which do you find easier to read?
