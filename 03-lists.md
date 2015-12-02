---
layout: page
title: Programming with Python
subtitle: Storing Multiple Values in Lists
minutes: ??
---
> ## Learning Objectives {.objectives}
>
> *   Explain what a list is.
> *   Create and index lists of simple values.

Just as a `for` loop is a way to do operations many times,
a list is a way to store many values.
We create a list by putting values inside square brackets:

~~~ {.python}
bases = ['A', 'C', 'G', 'T']
print 'DNA contains', bases
~~~

~~~ {.output}
DNA contains ['A', 'C', 'G', 'T']
~~~

We select individual elements from lists by indexing them:

~~~ {.python}
print 'first and last:', bases[0], bases[-1]
~~~

~~~ {.output}
first and last: A T
~~~

and if we loop over a list,
the loop variable is assigned elements one at a time:

~~~ {.python}
for base in bases:
    print base
~~~

~~~ {.output}
A
C
G
T
~~~

There is one important difference between lists and strings:
we can change the values in a list,
but we cannot change the characters in a string.
For example:

~~~ {.python}
names = ['Newton', 'Darwing', 'Turing'] # typo in Darwin's name
print('names is originally:', names)
names[1] = 'Darwin' # correct the name
print('final value of names:', names)

dna_rna = ['ACGT', 'ACGT'] # uracil in RNA!
print 'DNA contains', dna_rna[0]
print 'RNA contains', dna_rna[1]
dna_rna[1] = 'ACGU'
print 'RNA contains', dna_rna[1]
~~~

~~~ {.output}
DNA contains ACGT
RNA contains ACGT
RNA contains ACGU
~~~

works, but:

~~~ {.python}
dna_rna[1][3] = 'U'
~~~

~~~ {.error}
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-73-ef3c4f1d5d8e> in <module>()
----> 1 dna_rna[1][3] = 'U'

TypeError: 'str' object does not support item assignment
~~~

does not.

> ## Ch-Ch-Ch-Changes {.callout}
>
> Data which can be modified in place is called [mutable](reference.html#mutable),
> while data which cannot be modified is called [immutable](reference.html#immutable).
> Strings and numbers are immutable. This does not mean that variables with string or number values are constants,
> but when we want to change the value of a string or number variable, we can only replace the old value
> with a completely new value.
>
> Lists and arrays, on the other hand, are mutable: we can modify them after they have been created. We can
> change individual elements, append new elements, or reorder the whole list.  For some operations, like
> sorting, we can choose whether to use a function that modifies the data in place or a function that returns a
> modified copy and leaves the original unchanged.
>
> Be careful when modifying data in place.  If two variables refer to the same list, and you modify the list
> value, it will change for both variables! If you want variables with mutable values to be independent, you
> must make a copy of the value when you assign it.
>
> Because of pitfalls like this, code which modifies data in place can be more difficult to understand. However,
> it is often far more efficient to modify a large data structure in place than to create a modified copy for
> every small change. You should consider both of these aspects when writing your code.

There are many ways to change the contents of lists besides assigning new values to
individual elements:

~~~ {.python}
bases.append('U')
print 'DNA/RNA alphabet:', bases
~~~
~~~ {.output}
DNA/RNA alphabet: ['A', 'C', 'G', 'T', 'U']
~~~

~~~ {.python}
del bases[4]
print 'DNA alphabet:', bases
~~~
~~~ {.output}
DNA alphabet: ['A', 'C', 'G', 'T']
~~~

~~~ {.python}
bases.reverse()
print 'Complementary strand', bases
~~~
~~~ {.output}
Complementary strand ['T', 'G', 'C', 'A']
~~~

While modifying in place, it is useful to remember that python treats lists in a slightly counterintuitive way.

If we make a list and (attempt to) copy it then modify in place, we can cause all sorts of trouble:

~~~ {.python}
dna = ['A', 'C', 'G', 'T']
rna = dna
rna[3] = 'U'
print 'RNA', rna
print 'DNA', dna
~~~
~~~ {.output}
RNA ['A', 'C', 'G', 'U']
DNA ['A', 'C', 'G', 'U']
~~~

This is because python stores a list in memory, and then can use multiple names to refer to the same list.
If all we want to do is copy a (simple) list, we can use the list() command, so we do not modify a list we did not mean to:

~~~ {.python}
dna = ['A', 'C', 'G', 'T']
rna = list(dna)
rna[3] = 'U'
print 'RNA', rna
print 'DNA', dna
~~~
~~~ {.output}
RNA ['A', 'C', 'G', 'U']
DNA ['A', 'C', 'G', 'T']
~~~

We can use the `enumerate` function to get the position (or index) of the elements in a list.

~~~ {.python}
for i, base in enumerate(dna):
    print i, base
~~~
~~~ {.output}
0 A
1 C
2 G
3 T
~~~

Knowing lists allows us to have a closer look at the base qualities at each position. If we have M N-bp reads, it means we have N lists of M quality values:

~~~ {.python}
for read in fq.read_fq('small.fq'):
    qual = read[3]
    for q in qual[:10]:
        print ord(q) - 33,
    print
~~~
~~~ {.output}
31 31 31 35 35 35 35 35 39 39
31 34 34 37 37 37 37 37 39 39
34 34 34 37 37 37 37 37 39 39
34 34 34 37 37 37 37 37 39 39
33 33 34 33 35 37 37 37 39 39
34 34 34 37 37 37 37 37 39 39
34 34 34 37 37 37 37 37 39 39
30 30 30 33 35 26 33 30 28 25
34 34 34 37 37 37 37 37 39 39
33 31 33 37 37 37 37 37 39 39
~~~

To store these values, we have to build a list of lists.

~~~ {.python}
qualities = []
for q in qual:
    qualities.append([])
print 'empty qualities per position:' qualities[:10]
~~~
~~~ {.output}
empty qualities per position: [[], [], [], [], [], [], [], [], [], []]
~~~

~~~ {.python}
qualities = []
for q in qual:
    qualities.append([])
for read in fq.read_fq('small.fq'):
    qual = read[3]
    for i, q in enumerate(qual):
        qualities[i].append(ord(q) - 33)

print qualities[:10]
~~~
~~~ {.output}
[[31, 31, 34, 34, 33, 34, 34, 30, 34, 33], [31, 34, 34, 34, 33, 34, 34, 30, 34, 31], [31, 34, 34, 34, 34, 34, 34, 30, 34, 33], [35, 37, 37, 37, 33, 37, 37, 33, 37, 37], [35, 37, 37, 37, 35, 37, 37, 35, 37, 37], [35, 37, 37, 37, 37, 37, 37, 26, 37, 37], [35, 37, 37, 37, 37, 37, 37, 33, 37, 37], [35, 37, 37, 37, 37, 37, 37, 30, 37, 37], [39, 39, 39, 39, 39, 39, 39, 28, 39, 39], [39, 39, 39, 39, 39, 39, 39, 25, 39, 39]]
~~~

We can make this look nicer:

~~~ {.python}
for i, col in enumerate(qualities[:10]):
    for j, q in enumerate(qualities[i][:10]):
        print qualities[j][i],
    print
~~~
~~~ {.output}
31 31 31 35 35 35 35 35 39 39
31 34 34 37 37 37 37 37 39 39
34 34 34 37 37 37 37 37 39 39
34 34 34 37 37 37 37 37 39 39
33 33 34 33 35 37 37 37 39 39
34 34 34 37 37 37 37 37 39 39
34 34 34 37 37 37 37 37 39 39
30 30 30 33 35 26 33 30 28 25
34 34 34 37 37 37 37 37 39 39
33 31 33 37 37 37 37 37 39 39
~~~

And to calculate the average qualities for each position:

~~~ {.python}
for col in qualities:
    avg_q = 0
    for q in col:
        avg_q = avg_q + q
    avg_qualities.append(avg_q / float(n_sequences))

print 'Average qualities per position:'
for q in avg_qualities[:10]:
    print q,
print
~~~
~~~ {.output}
Average qualities per position:
32.8 32.9 33.2 36.0 36.4 35.7 36.4 36.1 37.9 37.6
~~~

We can now visualise this:

~~~ {.python}
%matplotlib inline
import numpy as np
import matplotlib.pyplot as plt

x = []
y = []
for i, pos_quals in enumerate(qualities):
    for q in pos_quals:
        x.append(i)
        y.append(q)

pl = plt.plot(x, y, 'k.', linewidth=0.5)
bp = plt.boxplot(qualities, 0, '')
avg_pl = plt.plot(range(len(avg_qualities)), avg_qualities, 'y', linewidth=3)
~~~



## Turn a string into a list {.challenge}

Use a for-loop to convert the string "hello" into a list of letters:

~~~ {.python}
["h", "e", "l", "l", "o"]
~~~
Hint: You can create an empty list like this:

~~~ {.python}
my_list = []
~~~
