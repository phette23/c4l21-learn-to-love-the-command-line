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
- other assorted tools: `grep`, `du`, `curl`?

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

There is no non-standard input or output or anything. Why? I don't know ¯\_(ツ)_/¯
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

You can also pass a number prefixed with a plus `+` sign to print everything _but_ N minus 1 lines. So. combining a few things, here is a deduplicated set of names with no header:

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

`>` and `>>`, can cover `less` here too?

Revisit table of special characters?

## Control flow: conditions, loops

should I do subshells too? `for NAME in $(cat names.txt); do echo $NAME; done` is pivotal, as is storing command output in a variable

- variables are a good prerequisite
- `if` `else` `fi`
- `&&` `||`
- `for` `do` `done` loops

Bash also has [`case`](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_03.html) conditions, where you check a series of conditions and can specify a fallback if not are met, and [`while`](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_09_02.html) loops that execute as long as a condition is true. These are less frequently used (I use them a tiny fraction of the time that I use `if` and `for`) and we won't cover them.

## Scripting

- changing file permissions & making a file executable (`chown`, `chmod`)
- running an executable program or script, `sudo`
- specifying the interpreter of a script
- `#` comments

## Customizing _your_ shell

- .bash_profile
- `$PATH` (use example of putting executable script we made on our PATH)
- `alias`
- prompt, PS1
