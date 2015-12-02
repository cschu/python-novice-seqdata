---
layout: page
title: Programming with Python
subtitle: Creating Functions
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Define a function that takes parameters.
> *   Return a value from a function.
> *   Test and debug a function.
> *   Set default values for function parameters.
> *   Explain why we should divide programs into small, single-purpose functions.

At this point,
we've written code to calculate and draw some interesting features in our sequencing data and
loop over all our data files to quickly draw these plots for each of them.
But, our code is getting pretty long and complicated;
what if we had thousands of datasets,
and didn't want to generate a figure for every single one?
Commenting out the figure-drawing code is a nuisance.
Also, what if we want to use that code again,
on a different dataset or at a different point in our program?
Cutting and pasting it is going to make our code get very long and very repetative,
very quickly.
We'd like a way to package our code so that it is easier to reuse,
and Python provides for this by letting us define things called 'functions' -
a shorthand way of re-executing longer pieces of code.

Let's start by defining a function `fahr_to_kelvin` that converts temperatures from Fahrenheit to Kelvin:

~~~ {.python}
def fahr_to_kelvin(temp):
    return ((temp - 32) * (5/9)) + 273.15
~~~

The function definition opens with the word `def`,
which is followed by the name of the function
and a parenthesized list of parameter names.
The [body](reference.html#function-body) of the function --- the
statements that are executed when it runs --- is indented below the definition line,
typically by four spaces.

When we call the function,
the values we pass to it are assigned to those variables
so that we can use them inside the function.
Inside the function,
we use a [return statement](reference.html#return-statement) to send a result back to whoever asked for it.

Let's try running our function.
Calling our own function is no different from calling any other function:

~~~ {.python}
print 'freezing point of water:', fahr_to_kelvin(32)
print 'boiling point of water:', fahr_to_kelvin(212)
~~~
~~~ {.output}
freezing point of water: 273.15
boiling point of water: 373.15
~~~

We've successfully called the function that we defined,
and we have access to the value that we returned.


## Composing Functions

Now that we've seen how to turn Fahrenheit into Kelvin,
it's easy to turn Kelvin into Celsius:

~~~ {.python}
def kelvin_to_celsius(temp):
    return temp - 273.15

print 'absolute zero in Celsius:', kelvin_to_celsius(0.0)
~~~
~~~ {.output}
absolute zero in Celsius: -273.15
~~~

What about converting Fahrenheit to Celsius?
We could write out the formula,
but we don't need to.
Instead,
we can [compose](reference.html#function-composition) the two functions we have already created:

~~~ {.python}
def fahr_to_celsius(temp):
    temp_k = fahr_to_kelvin(temp)
    result = kelvin_to_celsius(temp_k)
    return result

print 'freezing point of water in Celsius:', fahr_to_celsius(32.0)
~~~
~~~ {.output}
freezing point of water in Celsius: 0.0
~~~

Let's put the quality decoding into a function.

~~~ {.python}
def decodeQuality(char):
    return ord(char) - 33

qual = '@@@IIIII'
for c in qual[:10]:
    print c, decodeQuality(c)
~~~
~~~ {.output}
@ 31
@ 31
@ 31
I 40
I 40
I 40
I 40
I 40
~~~

This is our first taste of how larger programs are built:
we define basic operations,
then combine them in ever-large chunks to get the effect we want.
Real-life functions will usually be larger than the ones shown here --- typically half a dozen to a few dozen lines --- but
they shouldn't ever be much longer than that,
or the next person who reads it won't be able to understand what's going on.

## Tidying up

Now that we know how to wrap bits of code up in functions,
we can make our analysis easier to read and easier to reuse.

For each of the 3 tests, we will build functions for calculating and plotting. We start with the GC content analysis.

~~~ {.python}
def computeGCContentPerRead(filename):
    gc_content = []
    for read in fq.read_fq(filename):
        nGC = read[1].count('G') + read[1].count('C')
        gc_content.append(float(nGC) / len(read[1]))
    return gc_content

def plotGCContentPerRead(subplot, gc_content):
    n, bins, patches = subplot.hist(gc_per_read, 20, normed=1, facecolor='green', alpha=0.2)
    y = mlab.normpdf(bins, np.mean(gc_per_read), np.std(gc_per_read))
    l = subplot.plot(bins, y, 'r--', linewidth=2)

gc_per_read = computeGCContentPerRead('large.fq')
for i, gc in enumerate(gc_per_read[:10]):
    print i, gc


fig = plt.figure(figsize=(30.0, 6.0))
sp1 = fig.add_subplot(1, 3, 1)
plotGCContentPerRead(sp1, gc_per_read)
~~~

We do the same with the base content per position:

~~~ {.python}
def computeBaseComposition(filename):
    base_composition = []
    n_sequences = 0
    for read in fq.read_fq(filename):
        n_sequences += 1
        seq = read[1]
        if not base_composition:
            for base in seq:
                base_composition.append([0, 0, 0, 0, 0])
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

    for i, pos in enumerate(base_composition):
        for j, base in enumerate('ACGTN'):
            base_composition[i][j] = base_composition[i][j] / float(n_sequences)

    return base_composition


def plotBaseComposition(subplot, base_composition):
    y = []
    x = range(len(base_composition))
    for base in 'ACGTN':
        y.append([])
    for i, base in enumerate('ACGTN'):
        for j, pos in enumerate(base_composition):
            y[i].append(pos[i])
        subplot.plot(x, y[i], linewidth=0.5)

base_composition = computeBaseComposition('large.fq')
for i, base in enumerate('ACGTN'):
    print base,
    for j, pos in enumerate(base_composition):
        if j < 10:
            pass
            #print pos[i] / float(n_sequences),
    print



fig = plt.figure(figsize=(30.0, 6.0))
sp2 = fig.add_subplot(1, 3, 2)
plotBaseComposition(sp2,base_composition)
~~~

And finally with the quality plots.

~~~ {.python}
def checkQualityPerBase(filename):
    qualities = []
    for read in fq.read_fq(filename):
        qual = read[3]
        if not qualities:
            for q in qual:
                qualities.append([])
        for i, q in enumerate(qual):
            qualities[i].append(decodeQuality(q))
    return qualities

def plotQualityPerBase(subplot, qualities):
    x = []
    y = []
    avg_qualities = []
    for i, col in enumerate(qualities):
        avg_qualities.append(0)
        for q in col:
            x.append(i)
            y.append(q)
            avg_qualities[i] = avg_qualities[i] + q
        avg_qualities[i] = avg_qualities[i] / len(col)

    #subplot.plot(x, y, 'k.', linewidth=0.5)
    bp = subplot.boxplot(qualities, 0, '')
    avg_pl = subplot.plot(range(len(avg_qualities)), avg_qualities, 'y', linewidth=3)


qualities = checkQualityPerBase('large.fq')

for i, col in enumerate(qualities[:10]):
    for j, q in enumerate(qualities[i][:10]):
        print qualities[j][i],
    print

fig = plt.figure(figsize=(30.0, 6.0))
sp3 = fig.add_subplot(1, 3, 3)
plotQualityPerBase(sp3,qualities)
~~~


Notice that rather than jumbling this code together in one giant `for` loop,
we can now read and reuse both ideas separately.
We can reproduce the previous analysis with a much simpler `for` loop:

~~~ {.python}
fig = plt.figure(figsize=(30.0, 24.0))


subplots = 0
for filename in sorted(glob.glob('*genomic*.100k.fq')):

    qualities = checkQualityPerBase(filename)
    base_composition = computeBaseComposition(filename)
    gc_per_read = computeGCContentPerRead(filename)

    subplot = fig.add_subplot(4, 3, subplots+1)
    plotQualityPerBase(subplot, qualities)
    subplot = fig.add_subplot(4, 3, subplots+2)
    plotBaseComposition(subplot,base_composition)
    subplot = fig.add_subplot(4, 3, subplots+3)
    plotGCContentPerRead(subplot, gc_per_read)
    subplots += 3
~~~

By giving our functions human-readable names,
we can more easily read and understand what is happening in the `for` loop.
Even better, if at some later date we want to use either of those pieces of code again,
we can do so in a single line.


> ## Combining strings {.challenge}
>
> "Adding" two strings produces their concatenation:
> `'a' + 'b'` is `'ab'`.
> Write a function called `fence` that takes two parameters called `original` and `wrapper`
> and returns a new string that has the wrapper character at the beginning and end of the original.
> A call to your function should look like this:
>
> ~~~ {.python}
> print(fence('name', '*'))
> ~~~
> ~~~ {.output}
> *name*
> ~~~

> ## Selecting characters from strings {.challenge}
>
> If the variable `s` refers to a string,
> then `s[0]` is the string's first character
> and `s[-1]` is its last.
> Write a function called `outer`
> that returns a string made up of just the first and last characters of its input.
> A call to your function should look like this:
>
> ~~~ {.python}
> print(outer('helium'))
> ~~~
> ~~~ {.output}
> hm
> ~~~

> ## Rescaling an array {.challenge}
>
> Write a function `rescale` that takes an array as input
> and returns a corresponding array of values scaled to lie in the range 0.0 to 1.0.
> (Hint: If $L$ and $H$ are the lowest and highest values in the original array,
> then the replacement for a value $v$ should be $(v-L) / (H-L)$.)

> ## Testing and documenting your function {.challenge}
>
> Run the commands `help(numpy.arange)` and `help(numpy.linspace)`
> to see how to use these functions to generate regularly-spaced values,
> then use those values to test your `rescale` function.
> Once you've successfully tested your function,
> add a docstring that explains what it does.

> ## Defining defaults {.challenge}
>
> Rewrite the `rescale` function so that it scales data to lie between 0.0 and 1.0 by default,
> but will allow the caller to specify lower and upper bounds if they want.
> Compare your implementation to your neighbor's:
> do the two functions always behave the same way?

> ## Variables inside and outside functions {.challenge}
>
> What does the following piece of code display when run - and why?
>
> ~~~ {.python}
> f = 0
> k = 0
>
> def f2k(f):
>   k = ((f-32)*(5.0/9.0)) + 273.15
>   return k
>
> f2k(8)
> f2k(41)
> f2k(32)
>
> print(k)
> ~~~
