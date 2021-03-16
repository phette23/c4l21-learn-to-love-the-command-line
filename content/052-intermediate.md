---
layout: default
nav_order: 5
title: Intermediate CLI topics
permalink: /intermediate/
has_children: true
---

# Intermediate command line topics

@TODO

- text tools that work with output redirection: `sed`
- other assorted tools: `grep`, `du`, `curl`, `less`?

---

At the start, I mentioned that "text is the universal interface", but we've not truly seen how the command line handles text. First, let's download this example files of names and then do a bunch of operations on it:

Input
{: .label .label-green}
```sh
$ curl -o names.txt -L phette23.github.io/c4l21-learn-to-love-the-command-line/names.txt
```

Output
{: .label .label-yellow}
```sh
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                               Dload  Upload   Total   Spent    Left  Speed
100   162  100   162    0     0   3857      0 --:--:-- --:--:-- --:--:--  3951
100   137  100   137    0     0    856      0 --:--:-- --:--:-- --:--:--  1412
```

You should be able to copy-paste that URL onto your prompt. Note that, on Windows, the <kbd>Control + C</kbd> and <kbd>Control + V</kbd> keyboard shortcuts conflict with special CLI functions. You may need to right-click with your mouse to paste. If `curl` isn't working for you, you can always [download the file](/names.txt) into our workshop folder.

## Text processing greatest hits

We have a lot to cover, so let's quickly learn the basics of several commands before diving into making them work together.

The `cat` 😺 command stands for con**cat**enate but it's often used simply to print the contents of a text file to your terminal:

Input
{: .label .label-green}
```sh
$ cat names.txt
```

Output
{: .label .label-yellow}
```sh
names
ephetteplace
katharina
ariadne
phette23
ephetteplace
testname
madeupname
nameyMcNameName
names with spaces in it
testname
testname
```

`sort` sorts text lexically:

Input
{: .label .label-green}
```sh
$ sort names.txt
```

Output
{: .label .label-yellow}
```sh
ariadne
ephetteplace
ephetteplace
katharina
madeupname
names
names with spaces in it
nameyMcNameName
phette23
testname
testname
testname
```

`uniq` removes duplicate lines, leaving only unique values:

Input
{: .label .label-green}
```sh
$ uniq names.txt
```

Output
{: .label .label-yellow}
```sh
names
ephetteplace
katharina
ariadne
phette23
ephetteplace
testname
madeupname
nameyMcNameName
names with spaces in it
testname
```

Wait a minute, why didn't that work? Can you see the difference between passing names.txt to `uniq` and `cat`? Any guesses as to why this didn't work?

I find it counterintuitive but what `uniq` actually does is remove duplicate _adjacent_ lines from its text input. So to find a unique list of values we want to first `sort` and then `uniq` our names file. How do we do this? Using **output redirection** to "pipe" the output of `sort` to the input of `uniq`. This is one of the most powerful features of the command line:

Input
{: .label .label-green}
```sh
$ sort names.txt | uniq
```

Output
{: .label .label-yellow}
```sh
ariadne
ephetteplace
katharina
madeupname
names
names with spaces in it
nameyMcNameName
phette23
testname
```

The vertical bar `|` performs this piping of output to input. Notice how our `uniq` has no arguments that follow it; since piping data between commands is so powerful, most commands are designed to work with it. You might see the terms "standard output", abbreviated "stdout", and "standard input" or "stdin" mentioned in documentation for commands. Those terms refer to these streams of data, coming from and going into a command. So while our `uniq` command did not have a text file to for input it checked stdin and found the names text there.

There is no non-standard input or output or anything. So why do we use "standard output" and not simply "output"? I don't know ¯\\_(ツ)_/¯
{: .note }

To demonstrate pipes, let's look at the mirrored commands `head` and `tail` which both take a numeric `-n` argument and return either the first or last N lines:

Input
{: .label .label-green}
```sh
$ sort names.txt | head -n 4
```

Output
{: .label .label-yellow}
```sh
ariadne
ephetteplace
ephetteplace
katharina
```

To get the very last line:

Input
{: .label .label-green}
```sh
$ sort names.txt | tail -n 1
```

Output
{: .label .label-yellow}
```sh
testname
```

You can have any number of commands strung together in what's referred to as a pipeline:

Input
{: .label .label-green}
```sh
$ cat names.txt | wc -l
$ uniq names.txt | wc -l
$ sort names.txt | uniq | wc -l
```

Output
{: .label .label-yellow}
```sh
12
11
9
```

The `wc` command stands for **w**ord **c**ount and the `-l` flag tells it to count the lines of its input. So this triad of commands demonstrates the differences in length between the complete names.txt file, the file without repeated lines, and the file without duplicate non-adjacent lines.

You can also pass a number prefixed with a plus `+` sign to print everything _but_ N minus 1 lines. Why N minus 1 and not simply N? Again, I cannot explain, that's just how it works 🤷🏻. So, combining a few things, here is a deduplicated set of names with no header:

Input
{: .label .label-green}
```sh
$ tail -n +2 names.txt | sort | uniq
```

Output
{: .label .label-yellow}
```sh
ariadne
ephetteplace
katharina
madeupname
names with spaces in it
nameyMcNameName
phette23
testname
```

One last handy tool before we look at files: `tr` stands for **tr**anslate. It converts characters but we need to know piping because it operates _only_ on standard input, not files.

Input
{: .label .label-green}
```sh
$ cat names.txt | tr 'n' 'N'
```

Output
{: .label .label-yellow}
```sh
Names
ephetteplace
kathariNa
ariadNe
phette23
ephetteplace
testName
madeupName
NameyMcNameName
Names with spaces iN it
testName
testName
```

`tr` has a useful `-d` flag which, instead of translating a given character, _deletes_ it. I use this frequently to remove newline characters from input but here we can remove spaces:

Input
{: .label .label-green}
```sh
$ tail -n 5 names.txt | tr -d ' '
```

Output
{: .label .label-yellow}
```sh
madeupname
nameyMcNameName
nameswithspacesinit
testname
testname
```

## Writing output to a file

All this manipulating text output is well and good but what if we want to write results to a file? It's relatively rare that we will only want to view transformed text, we will usually want to record the changes in a file. Luckily, much like the vertical bar `|` pipes one command's output to another's input, we can use the greater than symbol `>` to write output to a file:

```sh
$ cat names.txt | sort | uniq > sorted-names.txt
```

If we write to a file that does not yet exist, it is created. If we write to a pre-existing file, its contents are overwritten. We can run `cat sorted-names.txt` to see the file's content and verify our command worked.

It's often useful to not overwrite but to _append_ to a file. For instance, if we are continually monitoring the output of a repeated process, we want to append output to a log file by using two greater thans `>>`:

```sh
$ echo "first name" > more-names.txt
$ cat names.txt | tail -n +2 >> more-names.txt
$ echo "last name" >> more-names.txt
```

`echo` is a new command to us—it simply prints the input string(s) we provide to stdout. Why would that be useful? It's mostly so that Bash scripts can print information as they execute. Here, we used `echo` to create a new file "more-names.txt" with a single entry, then added all but the first line of our names.txt file to it, and then added one final entry.

Two important warnings: `>` _erases the file's contents_ before our chain of commands even begins. So often if we write to the input file, it simply wipes out the file's contents. Notice how `sort more-names.txt > more-names.txt` erases the file's contents. Secondly, an analogous problem occurs when appending output to an input file: it creates an infinite loop that builds an enormous text file until we're out of disk space. Try to think of data as being processed one line at a time through the entire command pipeline, as opposed to the whole input file passed through step-by-step. If we keep adding to the input before processing is complete, the cycle never ends. Therefore, if we want to modify an existing file, we **write to a new file and then `mv` the new one to overwrite the input file** e.g. `cat names.txt | sort > sorted-names.txt; mv sorted-names.txt names.txt`.
{: .warn }

There are some commands that have flags which let us edit files in place but know that this is not typically the way things work in the shell, it has to be specially supported.

## Control flow: conditions, loops

should I do subshells too? `for NAME in $(cat names.txt); do echo $NAME; done` is pivotal, as is storing command output in a variable

- variables are a good prerequisite
- `if` `else` `fi`
- `&&` `||`
- `for` `do` `done` loops

Bash also has [`case`](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_03.html) conditions, where you check a series of conditions and can specify a fallback if not are met, and [`while`](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_09_02.html) loops that execute as long as a condition is true. These are less frequently used (I use them a tiny fraction of the time that I use `if` and `for`) and we won't cover them.

Let's revisit our table of special characters now. The meanings of hashmark `#` and single ampersand `&` are new to us. We won't have time to go into background jobs but we will see `#` when we look at Bash scripting.

| Character | Meaning
| --- | ---
| [space] | separates commands, arguments, and options
| \ | "escape" the following character
| ~ | User's home folder e.g. /Users/username
| . | The current location, the folder you're in
| .. | The parent folder of your current location
| * | wildcard
| | | instead of printing to stdout, pipe output to the next command
| > | instead of printing to stdout, write output to a file
| >> | instead of printing to stdout, append output to a file
| $ | used to reference variables
| && | logical "and", run the following command if the preceding one was successful
| || | logical "or", run the following command if the preceding was unsuccessful
| & | run the preceding command as a job in the background
| # | comment (shell ignores everything that follows it)

## Bash scripting

Other than a few extra bits, Bash scripting is wonderful because of its simplicity: if we put a series of commands into a file, we can then execute them all at once by feeding the file to the Bash shell.

- changing file permissions & making a file executable (`chown`, `chmod`)
- running an executable program or script, `sudo`
- specifying the interpreter of a script
- `#` comments

## Customizing _our_ shell

- .bash_profile
- `$PATH` (use example of putting executable script we made on our PATH)
- `alias`
- prompt, PS1
