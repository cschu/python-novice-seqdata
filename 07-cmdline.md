---
layout: page
title: Programming with Python
subtitle: Command-Line Programs
minutes: ??
---
> ## Learning Objectives {.objectives}
>
> *   Use the values of command-line arguments in a program.
> *   Handle flags and files separately in a command-line program.
> *   Read data from standard input in a program so that it can be used in a pipeline.

The IPython Notebook and other interactive tools are great for prototyping code and exploring data,
but sooner or later we will want to use our program in a pipeline
or run it in a shell script to process thousands of data files.
In order to do that,
we need to make our programs work like other Unix command-line tools.
For example,
we may want a program that reads a dataset
and prints the average, minimum, and maximum qualities per read.

> ## Switching to Shell Commands {.callout}
> In this lesson we are switching from typing commands in a Python interpreter to typing
> commands in a shell terminal window (such as bash). When you see a `$` in front of a
> command that tells you to run that command in the shell rather than the Python interpreter.


~~~
$ python qualstats.py --mean small.fq
small.fq
@HWI-ST863:222:D22L1ACXX:8:1101:1095:55853/1 32.6666666667
@HWI-ST863:222:D22L1ACXX:8:1101:1095:58549/1 37.3939393939
@HWI-ST863:222:D22L1ACXX:8:1101:1096:10563/1 35.4747474747
@HWI-ST863:222:D22L1ACXX:8:1101:1096:24331/1 37.1616161616
@HWI-ST863:222:D22L1ACXX:8:1101:1096:44576/1 37.9797979798
@HWI-ST863:222:D22L1ACXX:8:1101:1096:93615/1 38.2525252525
@HWI-ST863:222:D22L1ACXX:8:1101:1096:99982/1 37.8181818182
@HWI-ST863:222:D22L1ACXX:8:1101:1097:2610/1 29.4747474747
@HWI-ST863:222:D22L1ACXX:8:1101:1097:3713/1 39.4545454545
@HWI-ST863:222:D22L1ACXX:8:1101:1097:8358/1 39.5555555556
~~~

We might also want to look at the minimum qualities of the first four reads

~~~
$ head -n 16 small.fq | python qualstats.py --min
@HWI-ST863:222:D22L1ACXX:8:1101:1095:55853/1 2
@HWI-ST863:222:D22L1ACXX:8:1101:1095:58549/1 2
@HWI-ST863:222:D22L1ACXX:8:1101:1096:10563/1 2
@HWI-ST863:222:D22L1ACXX:8:1101:1096:24331/1 29
~~~

or the maximum qualities in several files one after another:

~~~
$ python qualstats.py --max small*.csv
../small2.fq
@HWI-ST863:222:D22L1ACXX:8:1101:1389:85332/1 40
@HWI-ST863:222:D22L1ACXX:8:1101:1389:86478/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1390:38061/1 40
@HWI-ST863:222:D22L1ACXX:8:1101:1391:7920/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1391:37561/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1391:73341/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1391:100638/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1392:10541/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1392:29060/1 40
@HWI-ST863:222:D22L1ACXX:8:1101:1392:54056/1 41
../small.fq
@HWI-ST863:222:D22L1ACXX:8:1101:1095:55853/1 40
@HWI-ST863:222:D22L1ACXX:8:1101:1095:58549/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1096:10563/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1096:24331/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1096:44576/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1096:93615/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1096:99982/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1097:2610/1 37
@HWI-ST863:222:D22L1ACXX:8:1101:1097:3713/1 41
@HWI-ST863:222:D22L1ACXX:8:1101:1097:8358/1 41
~~~

Our scripts should do the following:

1. If no filename is given on the command line, read data from [standard input](reference.html#standard-input).
2. If one or more filenames are given, read data from them and report statistics for each file separately.
3. Use the `--min`, `--mean`, or `--max` flag to determine what statistic to print.

To make this work,
we need to know how to handle command-line arguments in a program,
and how to get at standard input.
We'll tackle these questions in turn below.

## Command-Line Arguments

Using the text editor of your choice,
save the following in a text file called `sys-version.py`:

~~~ {.python}
import sys
print 'version is', sys.version
~~~

The first line imports a library called `sys`,
which is short for "system".
It defines values such as `sys.version`,
which describes which version of Python we are running.
We can run this script from the command line like this:

~~~ {.input}
$ python sys-version.py
~~~

~~~ {.output}
version is 3.4.3+ (default, Jul 28 2015, 13:17:50)
[GCC 4.9.3]
~~~

Create another file called `argv-list.py` and save the following text to it.

~~~ {.python}
import sys
print 'sys.argv is', sys.argv
~~~

The strange name `argv` stands for "argument values".
Whenever Python runs a program,
it takes all of the values given on the command line
and puts them in the list `sys.argv`
so that the program can determine what they were.
If we run this program with no arguments:

~~~ {.input}
$ python argv-list.py
~~~

~~~ {.output}
sys.argv is ['argv-list.py']
~~~

the only thing in the list is the path to our script,
which is always `sys.argv[0]`.
If we run it with a few arguments, however:

~~~ {.input}
$ python argv-list.py first second third
~~~
~~~ {.output}
sys.argv is ['argv-list.py', 'first', 'second', 'third']
~~~

then Python adds each of those arguments to that magic list.

With this in hand,
let's build a version of `qualstats.py` that always prints the per-read mean quality of a single data file.
The first step is to write a function that outlines our implementation,
and a placeholder for the function that does the actual work.
By convention this function is usually called `main`,
though we can call it whatever we want:

~~~ {.input}
$ cat qualstats-01.py
~~~

~~~ {.python}
import sys

def decodeQuality(char):
    return ord(char) - 33

def main():
    filename = sys.argv[1]
    with open(filename) as fi:
        for line in fi:
            id_, seq, sep, qual = line, fi.next(), fi.next(), fi.next()
            qualities = []
            for q in qual.strip():
                qualities.append(decodeQuality(q))
            print sum(qualities) / float(len(qualities))
~~~

This function gets the name of the script from `sys.argv[0]`,
because that's where it's always put,
and the name of the file to process from `sys.argv[1]`.
Here's a simple test:

~~~ {.input}
$ python qualstats.py small.fq
~~~

There is no output because we have defined a function,
but haven't actually called it.
Let's add a call to `main`:

~~~ {.input}
$ cat qualstats-02.py
~~~

~~~ {.python}
import sys

def decodeQuality(char):
    return ord(char) - 33

def main():
    filename = sys.argv[1]
    with open(filename) as fi:
        for line in fi:
            id_, seq, sep, qual = line, fi.next(), fi.next(), fi.next()
            qualities = []
            for q in qual.strip():
                qualities.append(decodeQuality(q))
            print sum(qualities) / float(len(qualities))

main()
~~~

and run that:

~~~ {.input}
$ python qualstats-02.py small.fq
~~~

~~~ {.output}
32.6666666667
37.3939393939
35.4747474747
37.1616161616
37.9797979798
38.2525252525
37.8181818182
29.4747474747
39.4545454545
39.5555555556
~~~

> ## The Right Way to Do It {.callout}
>
> If our programs can take complex parameters or multiple filenames,
> we shouldn't handle `sys.argv` directly.
> Instead,
> we should use Python's `argparse` library,
> which handles common cases in a systematic way,
> and also makes it easy for us to provide sensible error messages for our users.

## Handling Multiple Files

The next step is to teach our program how to handle multiple files.

~~~ {.input}
$ ls small-*.fq
~~~
~~~ {.output}
small-1.fq small-2.fq small-3.fq
~~~

~~~ {.input}
$ cat small-1.fq
~~~

~~~ {.input}
$ python qualstats-02.py small-1.fq
~~~

Using small data files as input also allows us to check our results more easily:
here,
for example,
we can see that our program is calculating the mean correctly for each line,
whereas we were really taking it on faith before.
This is yet another rule of programming:
*test the simple things first*.

We want our program to process each file separately,
so we need a loop that executes once for each filename.
If we specify the files on the command line,
the filenames will be in `sys.argv`,
but we need to be careful:
`sys.argv[0]` will always be the name of our script,
rather than the name of a file.
We also need to handle an unknown number of filenames,
since our program could be run for any number of files.

The solution to both problems is to loop over the contents of `sys.argv[1:]`.
The '1' tells Python to start the slice at location 1,
so the program's name isn't included;
since we've left off the upper bound,
the slice runs to the end of the list,
and includes all the filenames.
Here's our changed program
`readings-03.py`:

~~~ {.input}
$ cat qualstats-03.py
~~~

~~~ {.python}
import sys

def decodeQuality(char):
    return ord(char) - 33

def main():
    for filename in sys.argv[1:]:
        with open(filename) as fi:
            for line in fi:
                id_, seq, sep, qual = line, fi.next(), fi.next(), fi.next()
                qualities = []
                for q in qual.strip():
                    qualities.append(decodeQuality(q))
                print sum(qualities) / float(len(qualities))

main()
~~~

and here it is in action:

~~~ {.input}
$ python qualstats-03.py small-1.fq small-2.fq
~~~

~~~ {.output}
32.6666666667
37.3939393939
35.4747474747
37.1616161616
37.9797979798
38.2525252525
37.8181818182
29.4747474747
39.4545454545
39.5555555556
36.1111111111
35.1414141414
35.5656565657
13.6868686869
37.4949494949
39.0101010101
39.6666666667
38.101010101
34.5656565657
37.5454545455
~~~

> ## The Right Way to Do It {.callout}
>
> At this point,
> we have created three versions of our script called `qualstats-01.py`,
> `qualstats-02.py`, and `qualstats-03.py`.
> We wouldn't do this in real life:
> instead,
> we would have one file called `qualstats.py` that we committed to version control
> every time we got an enhancement working.
> For teaching,
> though,
> we need all the successive versions side by side.

## Handling Command-Line Flags

The next step is to teach our program to pay attention to the `--min`, `--mean`, and `--max` flags.
These always appear before the names of the files,
so we could just do this:

~~~ {.input}
$ cat qualstats-04.py
~~~

~~~ {.python}
import sys

def decodeQuality(char):
    return ord(char) - 33

def main():
    action = sys.argv[1]
    for filename in sys.argv[2:]:
        with open(filename) as fi:
            for line in fi:
                id_, seq, sep, qual = line, fi.next(), fi.next(), fi.next()
                qualities = []
                for q in qual.strip():
                    qualities.append(decodeQuality(q))
                if action == '--mean':
                    print sum(qualities) / float(len(qualities))
                elif action == '--max':
                    print max(qualities)
                elif action == '--min':
                    print min(qualities)

main()
~~~

This works:

~~~ {.input}
$ python qualstats-04.py --max small-1.fq
~~~
~~~ {.output}
40
41
41
41
41
41
41
37
41
41
~~~

but there are several things wrong with it:

1.  `main` is too large to read comfortably.

2.  If `action` isn't one of the three recognized flags,
    the program loads each file but does nothing with it
    (because none of the branches in the conditional match).
    [Silent failures](reference.html#silence-failure) like this
    are always hard to debug.

This version pulls the processing of each file out of the loop into a function of its own.
It also checks that `action` is one of the allowed flags
before doing any processing,
so that the program fails fast:

~~~ {.input}
$ cat qualstats-05.py
~~~

~~~ {.python}
import sys

def decodeQuality(char):
    return ord(char) - 33

def process(fi, action):
    for line in fi:
        id_, seq, sep, qual = line, fi.next(), fi.next(), fi.next()
        qualities = []
        for q in qual.strip():
            qualities.append(decodeQuality(q))
        if action == '--mean':
            print sum(qualities) / float(len(qualities))
        elif action == '--max':
            print max(qualities)
        elif action == '--min':
            print min(qualities)


def main():
    action = sys.argv[1]
    assert action in ['--mean', '--max', '--min'], 'action needs to be --min, --mean, or --max'
    for filename in sys.argv[2:]:
        with open(filename) as fi:
            process(fi, action)


main()
~~~

This is a bit longer than its predecessor,
but broken into more readable and understandable chunks.

Python has a module named [argparse](http://docs.python.org/dev/library/argparse.html)
that helps handle complex command-line flags. We will not cover this module in this lesson
but you can go to Tshepang Lekhonkhobe's [Argparse tutorial](http://docs.python.org/dev/howto/argparse.html)
that is part of Python's Official Documentation.

## Handling Standard Input

The next thing our program has to do is read data from standard input if no filenames are given
so that we can put it in a pipeline,
redirect input to it,
and so on.
Let's experiment in another script called `count-stdin.py`:

~~~ {.input}
$ cat count-stdin.py
~~~

~~~ {.python}
import sys

count = 0
for line in sys.stdin:
    count += 1

print count, 'lines in standard input'
~~~

This little program reads lines from a special "file" called `sys.stdin`,
which is automatically connected to the program's standard input.
We don't have to open it --- Python and the operating system
take care of that when the program starts up ---
but we can do almost anything with it that we could do to a regular file.
Let's try running it as if it were a regular command-line program:

~~~ {.input}
$ python count-stdin.py < small-01.csv
~~~

~~~ {.output}
2 lines in standard input
~~~

A common mistake is to try to run something that reads from standard input like this:

~~~ {.input}
$ count_stdin.py small-01.fq
~~~

i.e., to forget the `<` character that redirect the file to standard input.
In this case,
there's nothing in standard input,
so the program waits at the start of the loop for someone to type something on the keyboard.
Since there's no way for us to do this,
our program is stuck,
and we have to halt it using the `Interrupt` option from the `Kernel` menu in the Notebook.

We now need to rewrite the program so that it loads data from `sys.stdin` if no filenames are provided.
Luckily,
`numpy.loadtxt` can handle either a filename or an open file as its first parameter,
so we don't actually need to change `process`.
That leaves `main`:

~~~ {.input}
$ cat qualstats-06.py
~~~

~~~ {.python}
import sys

def decodeQuality(char):
    return ord(char) - 33

def process(fi, action):
    for line in fi:
        id_, seq, sep, qual = line, fi.next(), fi.next(), fi.next()
        qualities = []
        for q in qual.strip():
            qualities.append(decodeQuality(q))
        if action == '--mean':
            print sum(qualities) / float(len(qualities))
        elif action == '--max':
            print max(qualities)
        elif action == '--min':
            print min(qualities)


def main():
    action = sys.argv[1]
    assert action in ['--mean', '--max', '--min'], 'action needs to be --min, --mean, or --max'
    filenames = sys.argv[2:]
    if len(filenames) == 0:
        process(sys.stdin, action)
    else:
        for filename in filenames:
            with open(filename) as fi:
                process(fi, action)


main()
~~~

Let's try it out:

~~~ {.input}
$ python qualstats-06.py --max small-1.fq
~~~

~~~ {.output}
40
41
41
41
41
41
41
37
41
41
~~~

And

~~~ {.input}
$ cat small-1.fq | python qualstats-06.py --max
~~~

~~~ {.output}
40
41
41
41
41
41
41
37
41
41
~~~


That's better.
In fact,
that's done:
the program now does everything we set out to do.

> ## Arithmetic on the command line {.challenge}
>
> Write a command-line program that does addition and subtraction:
>
> ~~~ {.python}
> $ python arith.py add 1 2
> ~~~
> ~~~ {.output}
> 3
> ~~~
> ~~~ {.python}
> $ python arith.py subtract 3 4
> ~~~
> ~~~ {.output}
> -1
> ~~~
>

> ## Finding particular files {.challenge}
>
> Using the `glob` module introduced [earlier](04-files.html),
> write a simple version of `ls` that shows files in the current directory with a particular suffix.
> A call to this script should look like this:
>
> ~~~ {.python}
> $ python my_ls.py py
> ~~~
> ~~~ {.output}
> left.py
> right.py
> zero.py
> ~~~

> ## Changing flags {.challenge}
>
> Rewrite `qualstats.py` so that it uses `-n`, `-m`, and `-x` instead of `--min`, `--mean`, and `--max` respectively.
> Is the code easier to read?
> Is the program easier to understand?

> ## Adding a help message {.challenge}
>
> Separately,
> modify `qualstats.py` so that if no parameters are given
> (i.e., no action is specified and no filenames are given),
> it prints a message explaining how it should be used.

> ## Adding a default action {.challenge}
>
> Separately,
> modify `readings.py` so that if no action is given
> it displays the means of the data.

> ## A file-checker {.challenge}
>
> Write a program called `check.py` that takes the names of one or more inflammation data files as arguments
> and checks that all the files have the same number of rows and columns.
> What is the best way to test your program?

> ## Counting lines {.challenge}
>
> Write a program called `line-count.py` that works like the Unix `wc` command:
>
> *   If no filenames are given, it reports the number of lines in standard input.
> *   If one or more filenames are given, it reports the number of lines in each, followed by the total number of lines.
