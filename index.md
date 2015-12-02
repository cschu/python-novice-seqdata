---
layout: page
title: Programming with Python
---
The best way to learn how to program is to do something useful,
so this introduction to Python is built around a common scientific task:
data analysis.

Our real goal isn't to teach you Python,
but to teach you the basic concepts that all programming depends on.
We use Python in our lessons because:

1.  we have to use *something* for examples;
2.  it's free, well-documented, and runs almost everywhere;
3.  it has a large (and growing) user base among scientists; and
4.  experience shows that it's easier for novices to pick up than most other languages.

But the two most important things are
to use whatever language your colleagues are using,
so that you can share your work with them easily,
and to use that language *well*.

We are performing quality checks of Illumina sequencing data.
The data sets are stored in Fastq format. Each read in a Fastq file consists of four rows: the identifier, the sequence, a separator, and the base qualities.

~~~
@HWI-ST863:222:D22L1ACXX:8:1101:1095:55853/1
ATTGCATGCACACCAAGTGTTNGGCAAAATGTTAGAGTGAGACGACAGTTGGGGAGGTTTGACGATTATCTTTCATTTGTAGAGTAGATGAATCGAAGA
+
@@@DDDDDHHHHFD;AG:CBF#3AFGHIEBGEHGG?F1?CBBDHGGFF8BFHID;;B(=?B>CBCC=C@CCCCC@>CDC>@A@:@CCCDCACDCBBC<5
~~~

We want to:

*   load that data into memory,
*   perform some quality control tests, and
*   plot the results.

To do all that, we'll have to learn a little bit about programming.

> ## Prerequisites {.prereq}
>
> Learners need to understand the concepts of files and directories
> (including the working directory) and how to start a Python
> interpreter before tackling this lesson. This lesson references the Jupyter (IPython)
> Notebook although it can be taught through any Python interpreter.
> The commands in this lesson pertain to **Python 2**.

> ## Getting ready {.getready}
>
> You need to download some files to follow this lesson:
>
> 1. Make a new folder in your Desktop called `python-novice-fastqc`.
> 2. Download [python-novice-fastqc-data.zip](./python-novice-fastqc-data.zip) and move the file to this folder.
> 3. If it's not unzipped yet, double-click on it to unzip it. You should end up with a new folder called `data`.
> 4. You can access this folder from the Unix shell with:
>
> ~~~ {.input}
> $ cd && cd Desktop/python-novice-fastqc/data
> ~~~

## Topics

1.  [Introduction and Libraries](01-libraries.html)
2.  [Repeating Actions with Loops](02-loop.html)
3.  [Storing Multiple Values in Lists](03-lists.html)
4.  [Making Choices](04-cond.html)
5.  [Analyzing Data from Multiple Files](05-files.html)
6.  [Creating Functions](06-func.html)
7.  [Command-Line Programs](07-cmdline.html)


## Other Resources

*   [Reference](reference.html)
*   [Discussion](discussion.html)
*   [Instructor's Guide](instructors.html)
