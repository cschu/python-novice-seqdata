---
layout: page
title: Programming with Python
subtitle: Analyzing Data from Multiple Files
minutes: ??
---
> ## Learning Objectives {.objectives}
>
> *   Use a library function to get a list of filenames that match a simple wildcard pattern.
> *   Use a for loop to process multiple files.

We now have almost everything we need to process all our data files.
The only thing that's missing is a library with a rather unpleasant name:

~~~ {.python}
import glob
~~~

The `glob` library contains a single function, also called `glob`,
that finds files whose names match a pattern.
We provide those patterns as strings:
the character `*` matches zero or more characters,
while `?` matches any one character.
We can use this to get the names of all the HTML files in the current directory:

~~~ {.python}
print(glob.glob('*.html'))
~~~

~~~ {.output}
['01-numpy.html', '02-loop.html', '03-lists.html', '04-files.html', '05-cond.html', '06-func.html', '07-errors.html', '08-defensive.html', '09-debugging.html', '10-cmdline.html', 'index.html', 'LICENSE.html', 'instructors.html', 'README.html', 'discussion.html', 'reference.html']
~~~

As these examples show,
`glob.glob`'s result is a list of strings,
which means we can loop over it
to do something with each filename in turn.
In our case,
the "something" we want to do is generate a set of plots for each file in our sequencing dataset.
Let's test it by calculating and plotting the GC content per read for some larger input files.

~~~ {.python}
import glob
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.mlab as mlab

filenames = glob.glob('*.100k.fq')

for fn in filenames:
    print fn
    gc_per_read = []

    for read in fq.read_fq(fn):
        nGC = read[1].count('G') + read[1].count('C')
        gc_per_read.append(float(nGC) / len(read[1]))

    n, bins, patches = plt.hist(gc_per_read, 20, normed=1, facecolor='green', alpha=0.2)
    y = mlab.normpdf(bins, np.mean(gc_per_read), np.std(gc_per_read))
    l = plt.plot(bins, y, 'r--', linewidth=2)
    plt.show()
~~~
