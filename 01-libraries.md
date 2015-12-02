---
layout: page
title: Programming with Python
subtitle: Analyzing Sequencing Data
minutes: ??
---
> ## Learning Objectives {.objectives}
>
> *   Explain what a library is, and what libraries are used for.
> *   Load a Python library and use the things it contains.
> *   Read data from a file into a program.
> *   Assign values to variables.
> *   Select individual values and subsections from data.
> *   Perform operations on arrays of data.
> *   Display simple graphs.

Words are useful,
but what's more useful are the sentences and stories we build with them.
Similarly,
while a lot of powerful tools are built into languages like Python,
even more live in the [libraries](reference.html#library) they are used to build.

In order to work on our sequence data,
we need to [import](reference.html#import) a library called fq (for Fastq).
We can load fq using:

~~~ {.python}
import fq
~~~

Importing a library is like getting a piece of lab equipment out of a storage locker
and setting it up on the bench. Libraries provide additional functionality to the basic Python package, much like a new piece of equipment adds functionality to a lab space.
Once you've loaded the library,
we can ask the library to read our data file for us:

~~~ {.python}
fq.fq2list('small.fq')
~~~
~~~ {.output}
[('@HWI-ST863:222:D22L1ACXX:8:1101:1095:55853/1',
  'ATTGCATGCACACCAAGTGTTNGGCAAAATGTTAGAGTGAGACGACAGTTGGGGAGGTTTGACGATTATCTTTCATTTGTAGAGTAGATGAATCGAAGA',
  '+',
  '@@@DDDDDHHHHFD;AG:CBF#3AFGHIEBGEHGG?F1?CBBDHGGFF8BFHID;;B(=?B>CBCC=C@CCCCC@>CDC>@A@:@CCCDCACDCBBC<5'),
 ('@HWI-ST863:222:D22L1ACXX:8:1101:1095:58549/1',
  'AGCTTTTCGATGACACTGCGGNAAGTAGATAAAGTATGCTTGAAACCAACCTCTTTTCTCATCGAATTGAACATTTCTAGAGCTTTCATAGGATCCTTC',
  '+',
  '@CCFFFFFHHHHHIJJIJJIJ#2AFGEGHGGIDIGGIIJIIJJJJIIJGIIGFHIIJIIIIJJIGIJIAHHHGHFDDFFFFECEEEEEEEDDDDDDDD>'),
 ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:10563/1',
  'TTTGAACCTCCGCGACGTCTANACAGTGAGTGAGGTCCGTTCCATACGGAATTGTGGTTGTTGTCATTAGGGAAGAGAGTTTCACAGAGACTTAGAAGC',
  '+',
  'CCCFFFFFHHHGHJEHIGIII#08DDFGHHGIBFGHIJIFHIGGIHHHHFFFDE>@CCBBDDDCCCFEEDDDDBDBBCB@@CDDDDCDDDCCCDDDDDD'),
 ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:24331/1',
  'AGCTCGTCAGCTCACCCGTCCGAAGTATCTCGCCTAATGTAACCGTCTTGTCTGGACCAATTAACTCATTCCAGTAGCTCTTCTTGCCATCATCATAAA',
  '+',
  'CCCFFFFFHHHGHJJJJJHIJIIJJDHCFIIGJJGHIJJBHIJJJIHIIIEECGGHHHHFFFFFEECECEDEBCA>CCDDDDDDDDDDDDDDCCCDCDD'),
 ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:44576/1',
  'TGGTTTTTGGTCCAAATGAATCTCTGACACAAGAGGCATGGTGAGTAGAGAACAAGGAAGGTTTCCAGAAAAGTGATTATTGGTTAGATTGTATAAAGG',
  '+',
  'BBCBDFFFHHHHHJJJJJJJJJJJIJIJJJJJJJJJJJJIIBFEH@FHHHIJJJJHIIJIJHIJIJIHHHHHF7@BDFFEEEDCDDDDDEDAADDEEDD'),
 ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:93615/1',
  'CTAGATTCTCGTCAAAAAAATTTTCCCTCCTTCGATCTCTCCTTTTTCCTTATTCAGCATTTCCCCATTTTTTCACGCAAGATCCGTGATTTTCCAGGG',
  '+',
  'CCCFFFFFHHHHHJJJJIJGIJJJIIJJJJJJJJJJIGIJJIJJJJJIIIJCHIEHIJIIJJJHHHHHFFFFDEDCDDDDDDDDDDDDDDEEDEDDDDD'),
 ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:99982/1',
  'ATTTACTATGTCTCAATCGGGTCTTGTGAGCAATGTCCAAATTCCAAGGAGGAAGATCAAGATTTCTCACTTTGATAACTCGGCCTTGATTGCGGGTTA',
  '+',
  'CCCFFFFFHHHHGGGIJJJJB3CFGIIIGIGGIJIDHIIJIIGHIIGJJHHIIIJJIIJFIJJJIJJIIJJIHHHHHFCEDDFBCDDDDDDDDDDB>@A'),
 ('@HWI-ST863:222:D22L1ACXX:8:1101:1097:2610/1',
  'ACAAGAGGATGAAAGACGATACCGTGTGGAAATAGATTCTGATGTTCCAATTGTTTCTGAGCTTTCTTAGGTAGCAAGAAAGTAACCCTATGACCTTTC',
  '+',
  '???BD;B?=:C3AEEE;E<A1CFA:B:?D9CD>D>@DEC49??998/9?<BD=)8=B@;CE=ACDC@A@C;ACD<77;?@>;6>>AA?>?;AA>A:>AA'),
 ('@HWI-ST863:222:D22L1ACXX:8:1101:1097:3713/1',
  'GTAGAACATATGTTACTTAAAGGATCTATATATCTATCCAATCATAATTCGTATATATAGGGAAACCTTCACATTTACGTTTACTTATCAGCCGTTCGT',
  '+',
  'CCCFFFFFHHHHHIJJJJJJJJJHIJIJJJJJJJJJJJJJJJJJIJIJJJJJJIGIJJJJJJJJJJJJJJGJJJJHIJIHIHHHHHFFFFFFEDDDDD?'),
 ('@HWI-ST863:222:D22L1ACXX:8:1101:1097:8358/1',
  'GAATGGCTATATTTGTTCATAAATTTATAACAAAAATGGTAGCTTTTCTTTTCATGAATTCAAGCTTTTCTTCTGAATATGTGGTGGAATTTTCTGTAC',
  '+',
  'B@BFFFFFHHHHHJJGHJIJJJJJJJJJJJJJJJJJJJJEHIIJJJJGIJJJJIJJJIJJJJJJJJJJJJJJIJJIJJJJJJJJGHHHFFFFFDFEEDA')]
~~~

The expression `fq.fq2list(...)` is a [function call](reference.html#function-call)
that asks Python to run the function `fq2list` that belongs to the `fq` library.
This [dotted notation](reference.html#dotted-notation) is used everywhere in Python
to refer to the parts of things as `thing.component`.

`fq.fq2list` has a [parameter](reference.html#parameter):
the name of the file we want to read.
This parameter needs to be a character string (or [strings](reference.html#string) for short),
so we put it in quotes.

When we are finished typing and press Shift+Enter,
the notebook runs our command.
Since we haven't told it to do anything else with the function's output,
the notebook displays it.
In this case,
that output is the data we just loaded.

Our call to `fq.fq2list` read our file,
but didn't save the data in memory.
To do that,
we need to [assign](reference.html#assignment) the array to a [variable](reference.html#variable).
A variable is just a name for a value,
such as `x`, `current_temperature`, or `subject_id`.
Python's variables must begin with a letter and are [case sensitive](reference.html#case-sensitive).
We can create a new variable by assigning a value to it using `=`.
As an illustration,
let's step back and instead of considering a table of data,
consider the simplest "collection" of data,
a single value.
The line below assigns the value `55` to a variable `weight_kg`:

~~~ {.python}
weight_kg = 55
~~~

Once a variable has a value, we can print it to the screen:

~~~ {.python}
print(weight_kg)
~~~
~~~ {.output}
55
~~~

and do arithmetic with it:

~~~ {.python}
print('weight in pounds:', 2.2 * weight_kg)
~~~
~~~ {.output}
weight in pounds: 121.0
~~~

We can also change a variable's value by assigning it a new one:

~~~ {.python}
weight_kg = 57.5
print('weight in kilograms is now:', weight_kg)
~~~
~~~ {.output}
weight in kilograms is now: 57.5
~~~

As the example above shows,
we can print several things at once by separating them with commas.

If we imagine the variable as a sticky note with a name written on it,
assignment is like putting the sticky note on a particular value:

![Variables as Sticky Notes](fig/python-sticky-note-variables-01.svg)

This means that assigning a value to one variable does *not* change the values of other variables.
For example,
let's store the subject's weight in pounds in a variable:

~~~ {.python}
weight_lb = 2.2 * weight_kg
print('weight in kilograms:', weight_kg, 'and in pounds:', weight_lb)
~~~
~~~ {.output}
weight in kilograms: 57.5 and in pounds: 126.5
~~~

![Creating Another Variable](fig/python-sticky-note-variables-02.svg)

and then change `weight_kg`:

~~~ {.python}
weight_kg = 100.0
print('weight in kilograms is now:', weight_kg, 'and weight in pounds is still:', weight_lb)
~~~
~~~ {.output}
weight in kilograms is now: 100.0 and weight in pounds is still: 126.5
~~~

![Updating a Variable](fig/python-sticky-note-variables-03.svg)

Since `weight_lb` doesn't "remember" where its value came from,
it isn't automatically updated when `weight_kg` changes.
This is different from the way spreadsheets work.

Just as we can assign a single value to a variable, we can also assign an array of values
to a variable using the same syntax.  Let's re-run `fq.fq2list` and save its result:

~~~ {.python}
data = fq.fq2list('small.fq')
~~~

This statement doesn't produce any output because assignment doesn't display anything.
If we want to check that our data has been loaded,
we can print the variable's value:

~~~ {.python}
print data
~~~
~~~ {.output}
[('@HWI-ST863:222:D22L1ACXX:8:1101:1095:55853/1', 'ATTGCATGCACACCAAGTGTTNGGCAAAATGTTAGAGTGAGACGACAGTTGGGGAGGTTTGACGATTATCTTTCATTTGTAGAGTAGATGAATCGAAGA', '+', '@@@DDDDDHHHHFD;AG:CBF#3AFGHIEBGEHGG?F1?CBBDHGGFF8BFHID;;B(=?B>CBCC=C@CCCCC@>CDC>@A@:@CCCDCACDCBBC<5'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1095:58549/1', 'AGCTTTTCGATGACACTGCGGNAAGTAGATAAAGTATGCTTGAAACCAACCTCTTTTCTCATCGAATTGAACATTTCTAGAGCTTTCATAGGATCCTTC', '+', '@CCFFFFFHHHHHIJJIJJIJ#2AFGEGHGGIDIGGIIJIIJJJJIIJGIIGFHIIJIIIIJJIGIJIAHHHGHFDDFFFFECEEEEEEEDDDDDDDD>'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:10563/1', 'TTTGAACCTCCGCGACGTCTANACAGTGAGTGAGGTCCGTTCCATACGGAATTGTGGTTGTTGTCATTAGGGAAGAGAGTTTCACAGAGACTTAGAAGC', '+', 'CCCFFFFFHHHGHJEHIGIII#08DDFGHHGIBFGHIJIFHIGGIHHHHFFFDE>@CCBBDDDCCCFEEDDDDBDBBCB@@CDDDDCDDDCCCDDDDDD'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:24331/1', 'AGCTCGTCAGCTCACCCGTCCGAAGTATCTCGCCTAATGTAACCGTCTTGTCTGGACCAATTAACTCATTCCAGTAGCTCTTCTTGCCATCATCATAAA', '+', 'CCCFFFFFHHHGHJJJJJHIJIIJJDHCFIIGJJGHIJJBHIJJJIHIIIEECGGHHHHFFFFFEECECEDEBCA>CCDDDDDDDDDDDDDDCCCDCDD'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:44576/1', 'TGGTTTTTGGTCCAAATGAATCTCTGACACAAGAGGCATGGTGAGTAGAGAACAAGGAAGGTTTCCAGAAAAGTGATTATTGGTTAGATTGTATAAAGG', '+', 'BBCBDFFFHHHHHJJJJJJJJJJJIJIJJJJJJJJJJJJIIBFEH@FHHHIJJJJHIIJIJHIJIJIHHHHHF7@BDFFEEEDCDDDDDEDAADDEEDD'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:93615/1', 'CTAGATTCTCGTCAAAAAAATTTTCCCTCCTTCGATCTCTCCTTTTTCCTTATTCAGCATTTCCCCATTTTTTCACGCAAGATCCGTGATTTTCCAGGG', '+', 'CCCFFFFFHHHHHJJJJIJGIJJJIIJJJJJJJJJJIGIJJIJJJJJIIIJCHIEHIJIIJJJHHHHHFFFFDEDCDDDDDDDDDDDDDDEEDEDDDDD'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:99982/1', 'ATTTACTATGTCTCAATCGGGTCTTGTGAGCAATGTCCAAATTCCAAGGAGGAAGATCAAGATTTCTCACTTTGATAACTCGGCCTTGATTGCGGGTTA', '+', 'CCCFFFFFHHHHGGGIJJJJB3CFGIIIGIGGIJIDHIIJIIGHIIGJJHHIIIJJIIJFIJJJIJJIIJJIHHHHHFCEDDFBCDDDDDDDDDDB>@A'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1097:2610/1', 'ACAAGAGGATGAAAGACGATACCGTGTGGAAATAGATTCTGATGTTCCAATTGTTTCTGAGCTTTCTTAGGTAGCAAGAAAGTAACCCTATGACCTTTC', '+', '???BD;B?=:C3AEEE;E<A1CFA:B:?D9CD>D>@DEC49??998/9?<BD=)8=B@;CE=ACDC@A@C;ACD<77;?@>;6>>AA?>?;AA>A:>AA'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1097:3713/1', 'GTAGAACATATGTTACTTAAAGGATCTATATATCTATCCAATCATAATTCGTATATATAGGGAAACCTTCACATTTACGTTTACTTATCAGCCGTTCGT', '+', 'CCCFFFFFHHHHHIJJJJJJJJJHIJIJJJJJJJJJJJJJJJJJIJIJJJJJJIGIJJJJJJJJJJJJJJGJJJJHIJIHIHHHHHFFFFFFEDDDDD?'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1097:8358/1', 'GAATGGCTATATTTGTTCATAAATTTATAACAAAAATGGTAGCTTTTCTTTTCATGAATTCAAGCTTTTCTTCTGAATATGTGGTGGAATTTTCTGTAC', '+', 'B@BFFFFFHHHHHJJGHJIJJJJJJJJJJJJJJJJJJJJEHIIJJJJGIJJJJIJJJIJJJJJJJJJJJJJJIJJIJJJJJJJJGHHHFFFFFDFEEDA')]
~~~

Now that our data is in memory,
we can start doing things with it.
First,
let's ask what [type](reference.html#type) of thing `data` refers to:

~~~ {.python}
print type(data)
~~~
~~~ {.output}
<type 'list'>
~~~

The output tells us that `data` currently refers to a list. These data correspond Illumina sequencing reads. Each element in the list is an individual read, consisting of 4 different parts (identifier, sequence, separator, base qualities)

If we want to get an individual read from the list,
we must provide an [index](reference.html#index) in square brackets,
just as we do in math:

~~~ {.python}
print 'first read:', data[0]
~~~
~~~ {.output}
first read: ('@HWI-ST863:222:D22L1ACXX:8:1101:1095:55853/1', 'ATTGCATGCACACCAAGTGTTNGGCAAAATGTTAGAGTGAGACGACAGTTGGGGAGGTTTGACGATTATCTTTCATTTGTAGAGTAGATGAATCGAAGA', '+', '@@@DDDDDHHHHFD;AG:CBF#3AFGHIEBGEHGG?F1?CBBDHGGFF8BFHID;;B(=?B>CBCC=C@CCCCC@>CDC>@A@:@CCCDCACDCBBC<5')
~~~

~~~ {.python}
print 'fourth read:', data[3]
~~~
~~~ {.output}
fourth read: ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:24331/1', 'AGCTCGTCAGCTCACCCGTCCGAAGTATCTCGCCTAATGTAACCGTCTTGTCTGGACCAATTAACTCATTCCAGTAGCTCTTCTTGCCATCATCATAAA', '+', 'CCCFFFFFHHHGHJJJJJHIJIIJJDHCFIIGJJGHIJJBHIJJJIHIIIEECGGHHHHFFFFFEECECEDEBCA>CCDDDDDDDDDDDDDDCCCDCDD')
~~~

The expression `data[3]` may not surprise you,
but `data[0]` might.
Programming languages like Fortran and MATLAB start counting at 1,
because that's what human beings have done for thousands of years.
Languages in the C family (including C++, Java, Perl, and Python) count from 0
because that's simpler for computers to do.
As a result,
if we have a list of length M in Python,
its indices go from 0 to M-1.
It takes a bit of getting used to,
but one way to remember the rule is that
the index is how many steps we have to take from the start to get the item we want.

An index like `[3]` selects a single element of an array,
but we can select whole sections as well.
For example,
we can select the first five reads like this:

~~~ {.python}
print data[0:5]
~~~
~~~ {.output}
[('@HWI-ST863:222:D22L1ACXX:8:1101:1095:55853/1', 'ATTGCATGCACACCAAGTGTTNGGCAAAATGTTAGAGTGAGACGACAGTTGGGGAGGTTTGACGATTATCTTTCATTTGTAGAGTAGATGAATCGAAGA', '+', '@@@DDDDDHHHHFD;AG:CBF#3AFGHIEBGEHGG?F1?CBBDHGGFF8BFHID;;B(=?B>CBCC=C@CCCCC@>CDC>@A@:@CCCDCACDCBBC<5'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1095:58549/1', 'AGCTTTTCGATGACACTGCGGNAAGTAGATAAAGTATGCTTGAAACCAACCTCTTTTCTCATCGAATTGAACATTTCTAGAGCTTTCATAGGATCCTTC', '+', '@CCFFFFFHHHHHIJJIJJIJ#2AFGEGHGGIDIGGIIJIIJJJJIIJGIIGFHIIJIIIIJJIGIJIAHHHGHFDDFFFFECEEEEEEEDDDDDDDD>'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:10563/1', 'TTTGAACCTCCGCGACGTCTANACAGTGAGTGAGGTCCGTTCCATACGGAATTGTGGTTGTTGTCATTAGGGAAGAGAGTTTCACAGAGACTTAGAAGC', '+', 'CCCFFFFFHHHGHJEHIGIII#08DDFGHHGIBFGHIJIFHIGGIHHHHFFFDE>@CCBBDDDCCCFEEDDDDBDBBCB@@CDDDDCDDDCCCDDDDDD'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:24331/1', 'AGCTCGTCAGCTCACCCGTCCGAAGTATCTCGCCTAATGTAACCGTCTTGTCTGGACCAATTAACTCATTCCAGTAGCTCTTCTTGCCATCATCATAAA', '+', 'CCCFFFFFHHHGHJJJJJHIJIIJJDHCFIIGJJGHIJJBHIJJJIHIIIEECGGHHHHFFFFFEECECEDEBCA>CCDDDDDDDDDDDDDDCCCDCDD'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:44576/1', 'TGGTTTTTGGTCCAAATGAATCTCTGACACAAGAGGCATGGTGAGTAGAGAACAAGGAAGGTTTCCAGAAAAGTGATTATTGGTTAGATTGTATAAAGG', '+', 'BBCBDFFFHHHHHJJJJJJJJJJJIJIJJJJJJJJJJJJIIBFEH@FHHHIJJJJHIIJIJHIJIJIHHHHHF7@BDFFEEEDCDDDDDEDAADDEEDD')]
~~~

The [slice](reference.html#slice) `0:5` means,
"Start at index 0 and go up to, but not including, index 5."
Again,
the up-to-but-not-including takes a bit of getting used to,
but the rule is that the difference between the upper and lower bounds is the number of values in the slice.

We don't have to start slices at 0:

~~~ {.python}
print data[3:10]
~~~
~~~ {.output}
[('@HWI-ST863:222:D22L1ACXX:8:1101:1096:24331/1', 'AGCTCGTCAGCTCACCCGTCCGAAGTATCTCGCCTAATGTAACCGTCTTGTCTGGACCAATTAACTCATTCCAGTAGCTCTTCTTGCCATCATCATAAA', '+', 'CCCFFFFFHHHGHJJJJJHIJIIJJDHCFIIGJJGHIJJBHIJJJIHIIIEECGGHHHHFFFFFEECECEDEBCA>CCDDDDDDDDDDDDDDCCCDCDD'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:44576/1', 'TGGTTTTTGGTCCAAATGAATCTCTGACACAAGAGGCATGGTGAGTAGAGAACAAGGAAGGTTTCCAGAAAAGTGATTATTGGTTAGATTGTATAAAGG', '+', 'BBCBDFFFHHHHHJJJJJJJJJJJIJIJJJJJJJJJJJJIIBFEH@FHHHIJJJJHIIJIJHIJIJIHHHHHF7@BDFFEEEDCDDDDDEDAADDEEDD'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:93615/1', 'CTAGATTCTCGTCAAAAAAATTTTCCCTCCTTCGATCTCTCCTTTTTCCTTATTCAGCATTTCCCCATTTTTTCACGCAAGATCCGTGATTTTCCAGGG', '+', 'CCCFFFFFHHHHHJJJJIJGIJJJIIJJJJJJJJJJIGIJJIJJJJJIIIJCHIEHIJIIJJJHHHHHFFFFDEDCDDDDDDDDDDDDDDEEDEDDDDD'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1096:99982/1', 'ATTTACTATGTCTCAATCGGGTCTTGTGAGCAATGTCCAAATTCCAAGGAGGAAGATCAAGATTTCTCACTTTGATAACTCGGCCTTGATTGCGGGTTA', '+', 'CCCFFFFFHHHHGGGIJJJJB3CFGIIIGIGGIJIDHIIJIIGHIIGJJHHIIIJJIIJFIJJJIJJIIJJIHHHHHFCEDDFBCDDDDDDDDDDB>@A'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1097:2610/1', 'ACAAGAGGATGAAAGACGATACCGTGTGGAAATAGATTCTGATGTTCCAATTGTTTCTGAGCTTTCTTAGGTAGCAAGAAAGTAACCCTATGACCTTTC', '+', '???BD;B?=:C3AEEE;E<A1CFA:B:?D9CD>D>@DEC49??998/9?<BD=)8=B@;CE=ACDC@A@C;ACD<77;?@>;6>>AA?>?;AA>A:>AA'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1097:3713/1', 'GTAGAACATATGTTACTTAAAGGATCTATATATCTATCCAATCATAATTCGTATATATAGGGAAACCTTCACATTTACGTTTACTTATCAGCCGTTCGT', '+', 'CCCFFFFFHHHHHIJJJJJJJJJHIJIJJJJJJJJJJJJJJJJJIJIJJJJJJIGIJJJJJJJJJJJJJJGJJJJHIJIHIHHHHHFFFFFFEDDDDD?'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1097:8358/1', 'GAATGGCTATATTTGTTCATAAATTTATAACAAAAATGGTAGCTTTTCTTTTCATGAATTCAAGCTTTTCTTCTGAATATGTGGTGGAATTTTCTGTAC', '+', 'B@BFFFFFHHHHHJJGHJIJJJJJJJJJJJJJJJJJJJJEHIIJJJJGIJJJJIJJJIJJJJJJJJJJJJJJIJJIJJJJJJJJGHHHFFFFFDFEEDA')]
~~~

We also don't have to include the upper and lower bound on the slice.
If we don't include the lower bound,
Python uses 0 by default;
if we don't include the upper,
the slice runs to the end of the axis,
and if we don't include either
(i.e., if we just use ':' on its own),
the slice includes everything:

~~~ {.python}
small = data[:2]
print 'small is:'
print small
~~~
~~~ {.output}
small is:
[('@HWI-ST863:222:D22L1ACXX:8:1101:1095:55853/1', 'ATTGCATGCACACCAAGTGTTNGGCAAAATGTTAGAGTGAGACGACAGTTGGGGAGGTTTGACGATTATCTTTCATTTGTAGAGTAGATGAATCGAAGA', '+', '@@@DDDDDHHHHFD;AG:CBF#3AFGHIEBGEHGG?F1?CBBDHGGFF8BFHID;;B(=?B>CBCC=C@CCCCC@>CDC>@A@:@CCCDCACDCBBC<5'), ('@HWI-ST863:222:D22L1ACXX:8:1101:1095:58549/1', 'AGCTTTTCGATGACACTGCGGNAAGTAGATAAAGTATGCTTGAAACCAACCTCTTTTCTCATCGAATTGAACATTTCTAGAGCTTTCATAGGATCCTTC', '+', '@CCFFFFFHHHHHIJJIJJIJ#2AFGEGHGGIDIGGIIJIIJJJJIIJGIIGFHIIJIIIIJJIGIJIAHHHGHFDDFFFFECEEEEEEEDDDDDDDD>')]
~~~

We can access the individual parts of each read using the index notation as well.

~~~ {.python}
print 'read sequence', data[0][1]
print 'base qualities', data[0][3]
~~~
~~~ {.output}
read sequence ATTGCATGCACACCAAGTGTTNGGCAAAATGTTAGAGTGAGACGACAGTTGGGGAGGTTTGACGATTATCTTTCATTTGTAGAGTAGATGAATCGAAGA
base qualities @@@DDDDDHHHHFD;AG:CBF#3AFGHIEBGEHGG?F1?CBBDHGGFF8BFHID;;B(=?B>CBCC=C@CCCCC@>CDC>@A@:@CCCDCACDCBBC<5
~~~

The individual parts of each read are character strings. We can use  functions to get some information about them.

The function `len` allows us to obtain the length of a character string, in this case the read length.

~~~ {.python}
print 'sequence length:', len(data[0][1])
~~~
~~~ {.output}
sequence length: 99
~~~

The function `string.count(character)` counts the occurrences of a character in a string. This way we can count the number of known miscalled bases in a read:

~~~ {.python}
seq = data[0][1]
print '#N in sequence:', seq.count('N')
~~~
~~~ {.output}
#N in sequence: 1
~~~

Or, we can count the occurrences of the bases guanine and cytosine:

~~~ {.python}
print '#GC in sequence:', seq.count('G') + seq.count('C')
~~~
~~~ {.output}
#GC in sequence: 40
~~~

And with a bit of simple math, we can calculate the GC content of a read:

~~~ {.python}
nGC = seq.count('G') + seq.count('C')
print 'GC content (#G + #C) / length =', nGC / len(seq) * 100
~~~
~~~ {.output}
#GC in sequence: 0
~~~

This is of course a bit odd, if we have a read of length 99bp and count 40 Gs and Cs, we do not expect a GC content of 0. The reason for this is how Python 2.x treats mathematical operations on integer numbers. In order to get the correct value, we have to explicitly convert ('cast') the numbers to float.

> ## Integer division {.callout}
>
> We are using Python 3, where division always returns a floating point number:
>
> ~~~ {.python}
> $ python3 -c "print 5/9"
> ~~~
> ~~~ {.output}
> 0.5555555555555556
> ~~~
>
> Unfortunately, this wasn't the case in Python 2:
>
> ~~~ {.python}
> 5/9
> ~~~
> ~~~ {.output}
> 0
> ~~~
>
> If you are using Python 2 and want to keep the fractional part of division
> you need to convert one or the other number to floating point:
>
> ~~~ {.python}
> float(5)/9
> ~~~
> ~~~ {.output}
> 0.555555555556
> ~~~
> ~~~ {.python}
> 5/float(9)
> ~~~
> ~~~ {.output}
> 0.555555555556
> ~~~
> ~~~ {.python}
> 5.0/9
> ~~~
> ~~~ {.output}
> 0.555555555556
> ~~~
> ~~~ {.python}
> 5/9.0
> ~~~
> ~~~ {.output}
> 0.555555555556
> ~~~
>
> And if you want an integer result from division in Python 3,
> use a double-slash:
> ~~~ {.python}
> 4//2
> ~~~
> ~~~ {.output}
> 2
> ~~~
> ~~~ {.python}
> 3//2
> ~~~
> ~~~ {.output}
> 1
> ~~~


~~~ {.python}
print 'GC content (#G + #C) / length =', float(nGC) / len(seq) * 100
~~~
~~~ {.output}
#GC in sequence: 40.404040404
~~~

Next, we have a look at the base qualities.

~~~ {.python}
qual = data[0][3]
print qual
~~~
~~~ {.output}
@@@DDDDDHHHHFD;AG:CBF#3AFGHIEBGEHGG?F1?CBBDHGGFF8BFHID;;B(=?B>CBCC=C@CCCCC@>CDC>@A@:@CCCDCACDCBBC<5
~~~

The base qualities represent the probabilities of a base to be correctly called. The individual characters represent the scores and are nowadays encoded with the phred33 scheme. This means we take the numerical representation of a character using the `ord(character)` function.

~~~ {.python}
print qual[0], ord(qual[0])
~~~
~~~ {.output}
@ 64
~~~

The phred33 scoring scheme means that we have to subtract 33 from the ord-value of a character to obtain the actual quality score:

~~~ {.python}
print qual[0], ord(qual[0]) - 33
~~~
~~~ {.output}
@ 31
~~~

We can use this to have a closer look at the quality of the known miscalled base, which is represented by 'N', using the `string.find(string)` function.

~~~ {.python}
pos_miscalled = seq.find('N')
print qual[pos_miscalled], ord(qual[pos_miscalled])
~~~
~~~ {.output}
# 2
~~~

The mathematician Richard Hamming once said,
"The purpose of computing is insight, not numbers,"
and the best way to develop insight is often to visualize data.
Visualization deserves an entire lecture (or course) of its own,
but we can explore a few features of Python's `matplotlib` library here.
While there is no "official" plotting library,
this package is the de facto standard.
First,
we will import the `pyplot` module from `matplotlib`
and use its `plot` function to create and display a quality plot of our data:

~~~ {.python}
import matplotlib.pyplot
% matplotlib inline

x, y = fq.get_qualplot(qual)
matplotlib.pyplot.plot(x,y)
~~~

> ## Some IPython magic {.callout}
>
> If you're using an IPython / Jupyter notebook,
> you'll need to execute the following command
> in order for your matplotlib images to appear
> in the notebook when `show()` is called:
>
> ~~~ {.python}
> % matplotlib inline
> ~~~
>  
> The `%` indicates an IPython magic function -
> a function that is only valid within the notebook environment.
> Note that you only have to execute this function once per notebook.


You can group similar plots in a single figure using subplots.
This script below uses a number of new commands. The function `matplotlib.pyplot.figure()`
creates a space into which we will place all of our plots. The parameter `figsize`
tells Python how big to make this space. Each subplot is placed into the figure using
the `subplot` command. The `subplot` command takes 3 parameters. The first denotes
how many total rows of subplots there are, the second parameter refers to the
total number of subplot columns, and the final parameters denotes which subplot
your variable is referencing. Each subplot is stored in a different variable (axes1, axes2,
axes3). Once a subplot is created, the axes are can be titled using the
`set_xlabel()` command (or `set_ylabel()`).
Here are base composition plots side by side:

~~~ {.python}
import numpy
import matplotlib.pyplot

seqs = fq.fq2list('small.fq')
x, bases_y = fq.get_baseplot(seqs)

fig = matplotlib.pyplot.figure(figsize=(20.0,3.0))
figA = fig.add_subplot(1,4,1)
figA.set_ylabel('A content')
figA.set_xlabel('Base position')
figC = fig.add_subplot(1,4,2)
figC.set_ylabel('C content')
figC.set_xlabel('Base position')
figG = fig.add_subplot(1,4,3)
figG.set_ylabel('G content')
figG.set_xlabel('Base position')
figT = fig.add_subplot(1,4,4)
figT.set_ylabel('T content')
figT.set_xlabel('Base position')

figA.plot(x, bases_y[0], label='A')
figC.plot(x, bases_y[1], label='C')
figG.plot(x, bases_y[2], label='G')
figT.plot(x, bases_y[3], label='T')

fig.tight_layout()
matplotlib.pyplot.show(fig)
~~~

The [call](reference.html#function-call) to `fq.fq2list` reads our data,
`fq.get_baseplots` computes the base composition for the sequences,
and the rest of the program tells the plotting library
how large we want the figure to be,
that we're creating three sub-plots,
what to draw for each one,
and that we want a tight layout.
(Perversely,
if we leave out that call to `fig.tight_layout()`,
the graphs will actually be squeezed together more closely.)

> ## Scientists dislike typing {.callout}
>
> We will always use the syntax `import numpy` to import NumPy.
> However, in order to save typing, it is
> [often suggested](http://www.scipy.org/getting-started.html#an-example-script)
> to make a shortcut like so: `import numpy as np`.
> If you ever see Python code online using a NumPy function with `np`
> (for example, `np.loadtxt(...)`), it's because they've used this shortcut.

> ## Check your understanding {.challenge}
>
> Draw diagrams showing what variables refer to what values after each statement in the following program:
>
> ~~~ {.python}
> mass = 47.5
> age = 122
> mass = mass * 2.0
> age = age - 20
> ~~~

> ## Sorting out references {.challenge}
>
> What does the following program print out?
>
> ~~~ {.python}
> first, second = 'Grace', 'Hopper'
> third, fourth = second, first
> print(third, fourth)
> ~~~

> ## Slicing strings {.challenge}
>
> A section of an array is called a [slice](reference.html#slice).
> We can take slices of character strings as well:
>
> ~~~ {.python}
> element = 'oxygen'
> print('first three characters:', element[0:3])
> print('last three characters:', element[3:6])
> ~~~
>
> ~~~ {.output}
> first three characters: oxy
> last three characters: gen
> ~~~
>
> What is the value of `element[:4]`?
> What about `element[4:]`?
> Or `element[:]`?
>
> What is `element[-1]`?
> What is `element[-2]`?
> Given those answers,
> explain what `element[1:-1]` does.

> ## Thin slices {.challenge}
>
> The expression `element[3:3]` produces an [empty string](reference.html#empty-string),
> i.e., a string that contains no characters.
> If `data` holds our array of patient data,
> what does `data[3:3, 4:4]` produce?
> What about `data[3:3, :]`?

> ## Check your understanding: plot scaling {.challenge}
>
> Why do all of our plots stop just short of the upper end of our graph?

> ## Check your understanding: drawing straight lines {.challenge}
>
> Why are the vertical lines in our plot of the minimum inflammation per day not perfectly vertical?

> ## Make your own plot {.challenge}
>
> Create a plot showing the standard deviation (`numpy.std`) of the inflammation data for each day across all patients.

> ## Moving plots around {.challenge}
>
> Modify the program to display the three plots on top of one another instead of side by side.
