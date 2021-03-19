---
layout: default
nav_order: 5
title: Intermediate CLI topics
permalink: /intermediate/
has_children: true
---

@TODO

- text tools that work with output redirection: `sed`
- other assorted tools: `grep`, `less`?

---

# Intermediate command line topics

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

You should be able to copy-paste that URL onto your prompt. Note that, on Windows, the <kbd>Control + C</kbd> and <kbd>Control + V</kbd> keyboard shortcuts conflict with special terminal functions. You may need to right-click with your mouse to paste. If `curl` isn't working for you, you can always [download the file](/names.txt) into our workshop folder.

## Text processing greatest hits

We have a lot to cover, so let's quickly learn the basics of several commands before diving into making them work together.

The `cat` 😺 command stands for con**cat**enate. It can be used to combine several files but it's often used  to print the contents of a text file to your terminal:

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

Wait a minute, why didn't that work? Can you see the difference between passing names.txt to `uniq` and `cat`? Any guesses as to why this did not remove all the duplicates?

I find it counterintuitive but what `uniq` actually does is eliminate duplicate _adjacent_ lines. So to find a unique list of values we want to first `sort` and then `uniq` our names file. How do we do this? Using **output redirection** to "pipe" the output of `sort` to the input of `uniq`. Output redirection is one of the most powerful features of the command line:

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

You can also pass `head` or `tail` a number prefixed with a plus `+` sign to print everything _but_ N minus 1 lines. Why N minus 1 and not simply N? Again, I cannot explain, that's just how it works 🤷🏻. So, combining a few things, here is a deduplicated set of names with no header row:

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

One last handy tool before we look at files: `tr` stands for **tr**anslate. It converts a given character to another but we need to know output redirection because it operates _only_ on standard input, not on files.

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

All this manipulating text output is well and good but what if we want to write results to a file? It's relatively rare that we want merely to view transformed text, we usually want to save the changes in a file. Luckily, much like the vertical bar `|` pipes one command's output to another's input, we can use the greater than symbol `>` to write output to a file:

```sh
$ cat names.txt | sort | uniq > sorted-names.txt
```

If we write to a file that does not yet exist, it is created. If we write to a pre-existing file, its contents are overwritten.

It's often useful to not overwrite but to _append_ to a file. For instance, if we are continually monitoring the output of a repeated process, we use two greater thans `>>` to append output to a log file:

```sh
$ echo "first name" > more-names.txt
$ cat names.txt | tail -n +2 >> more-names.txt
$ echo "last name" >> more-names.txt
```

`echo` is a new command to us—it prints the input string(s) we provide to stdout. Why would that be useful? Mostly so Bash scripts can print information as they execute. Here, we used `echo` to create a new file "more-names.txt" with a single entry, then added all but the first line of our names.txt file to it, and then added one final entry.

Two important warnings: `>` _erases the file's contents_ before our chain of commands even begins. So often if we write to the same file we're using as input, it simply wipes out the file's contents. Notice how `sort more-names.txt > more-names.txt` empties the file.<br>
Secondly, an analogous problem occurs when appending output to the input file: it creates an infinite loop that builds an enormous text file until we out of disk space. Try to think of data as being processed one line at a time through the entire command pipeline, as opposed to the whole input file passed through step-by-step. If we keep adding to the input before processing is complete, the cycle never ends. Therefore, if we want to modify an existing file, we **write to a new file and then `mv` the new one to overwrite the input file** e.g. `sort names.txt > sorted-names.txt; mv sorted-names.txt names.txt`.
{: .warn }

There are some commands that have flags which let us edit files in place but know that this is not typically the way things work in the shell, but has to be specially supported.

## Control flow: variables, conditions, subshells

As we probably anticipated, Bash also has features typical of all programming languages that let us substitute "variable" names for literal data, conditions that execute commands only based on certain criteria, and loops that repeat operations a configurable number of times.

Bash variable assignment is straightforward: write a name, then equals `=`, then its value. To obtain the variables value later, prefix the name with a dollar sign `$`.

Input
{: .label .label-green}
```sh
$ NAME=Eric
$ echo $NAME
$ LASTNAME=Phetteplace
$ echo $LASTNAME
$ NAME="Adriadne Vegas"
$ echo $NAME
```

Output
{: .label .label-yellow}
```sh
Eric
Phetteplace
Adriadne Vegas
```

We see several notable things in this small example: _because spaces are meaningful characters_ we have to be cognizant of their usage. What happens if you write `LASTNAME = Phetteplace` with spaces? Also, we can overwrite existing variables by writing to them again.

What if we wanted to store the value of a command in a variable? The syntax also involves a `$` though the two operations are not similar:

Input
{: .label .label-green}
```sh
$ NAME=$(tail -n 1 names.txt)
$ ODDNAME=$(tail -n 1 names.txt | tr 'n' 'N' | tr 'e' '%')
$ echo $NAME
$ echo $ODDNAME
```

Output
{: .label .label-yellow}
```sh
testname
t%stNam%
```

The `$(...)` construction executes a **subshell** i.e. think of it as automating the process of us opening a new terminal window running the Bash shell, executing the command elided here as `...`, and then copy-pasting the stdout (if any) of that command back into our variable. Subshells are immensely useful in conjunction with the two other topics of this section: variables and loops.

@TODO

- `if` `else` `fi`
- `&&` `||`

Let's revisit our table of special characters now. The meanings of hashmark `#` and single ampersand `&` are new to us. We won't have time to go into background jobs but we will see `#` when we look at Bash scripting.

| Character | Meaning
| --- | ---
| [space] | separates commands, arguments, and options
| \ | "escape" the following character
| ~ | User's home folder e.g. /Users/username
| . | The current location, the folder you're in
| .. | The parent folder of your current location
| * | wildcard
| \| | instead of printing to stdout, pipe output to the next command
| > | instead of printing to stdout, write output to a file
| \>\> | instead of printing to stdout, append output to a file
| $ | used to reference variables, open subshells
| && | logical "and", run the following command if the preceding one was successful
| \|\| | logical "or", run the following command if the preceding was unsuccessful
| & | run the preceding command as a job in the background
| # | comment (shell ignores the rest of the line that follows it)

## Bash scripting

Other than a few extra bits, Bash scripting is wonderful because of its simplicity: if we put a series of commands into a file, we can then execute them all at once by feeding the file to the Bash shell.

Let's write a short script that sorts the contents of all the text files in the current directory.

You can edit scripts using whatever text editor—think NotePad++, Sublime Text, Atom, VS Code, etc. and not a rich text editor like MS Word or Pages—we want and I recommend you use whatever you feel most comfortable with. There are also command-line editors like `vim`, `nano`, and `emacs`. These are nice in that they keep you in the context of the CLI but are hard to learn. What's most handy is if your GUI editor has an associated command that opens files for you. Atom, for instance, let's you run `atom script.sh` to create or open a script in your current location.
{: .note }

```sh
echo $(date) "sorting all text files"

for TEXTFILE in $(ls *.txt); do
    echo "Sorting $TEXTFILE..."
    # remember we want to write to a temporary file, not to our input file
    sort $TEXTFILE > tmp
    mv tmp $TEXTFILE
    echo "Done."
done

echo "Done sorting all files."
```

This script concisely combines almost everything we've learned so far: subshells, variables, globbing, writing to a file, and comments. Let's explain the few pieces that we don't understand yet. We can name it "script.sh" and run it by passing it to the Bash shell itself: `bash script.sh`.

What if we don't want to specify the `bash` interpreter every time we run the script? To turn a script into something more like a real command, one indistinguishable from tools like `cat` and `echo`, we need to do two things: make the script tell the shell how to interpret its code and make it executable.

For the first step, we add `#!/usr/bin/env bash` as the very fisrt line of the script. What does this enigmatic phrase mean? If a script starts with the `#!` then the shell assumes what follows it is the interpreter to use. It cannot assume, for instance, that our code is Bash and not Python.

We could write `#!/bin/bash` here, directly referencing the path to our Bash shell. What might be the problem with that? Depending on our machine, Bash might be installed in a different location and our script is now less portable. The `env` command helps to execute commands with a consistent **env**ironment. In this instance, it will find a Bash interpreter for us—whether it's installed at /bin/bash, /usr/local/bin/bash, or elsewhere—and use it. Almost every script you intend to run on the command line, whether it's a shell script or not, should begin with `#!/usr/bin/env $INTERPRETER` e.g. `#!/usr/bin/env python` for a Python script.

For the second step, we need to understand more about permissions. Let's review the "long" version of `ls` output:

Input
{: .label .label-green}
```sh
$ ls -l
```

Output
{: .label .label-yellow}
```sh
total 4232
-rw-r--r--  1 ephetteplace  staff  2152700 Mar 16 12:56 more-names.txt
-rw-r--r--  1 ephetteplace  staff      137 Mar 16 16:11 names.txt
-rw-r--r--  1 ephetteplace  staff      223 Mar 16 12:56 script.sh
-rw-r--r--  1 ephetteplace  staff      106 Mar 16 12:56 sorted-names.txt
```

The very first segment of the listing is a representation of the files' permissions. We don't have time to fully understand it but suffice to say that "r" stands for **r**ead permissions, the ability to view the file, "w" stands for **w**rite permissions, the ability to modify the file, and what none of our files evince is an "x" permission for the ability to e**x**ecute the file as a program. These permissions are referred to as a file's "mode" and the `chmod` command **ch**anges the **mod**e of the file:

Input
{: .label .label-green}
```sh
$ chmod +x script.sh
$ ls -l
```

Output
{: .label .label-yellow}
```sh
total 4232
-rw-r--r--  1 ephetteplace  staff  2152700 Mar 16 12:56 more-names.txt
-rw-r--r--  1 ephetteplace  staff      137 Mar 16 16:11 names.txt
-rwxr-xr-x  1 ephetteplace  staff      223 Mar 16 12:56 script.sh
-rw-r--r--  1 ephetteplace  staff      106 Mar 16 12:56 sorted-names.txt
```

The `+x` flag essentially means "give everyone executable permissions for this file". Now we can reference our script simply by its path, not specifying an interpreter, to run it: `./script.sh` (recall that period `.` references our current location).

Whew, we've finally covered the hashbang and making something executable. But what about the `for...do...done` phrase in our script? That was a `for` loop which iterates over the output of the `$(ls *.txt)` subshell. For-loops can be used to A) iterate over all the words in a sentence, or B) iterate over all the lines in a file.

```sh
#!/usr/bin/env bash
# These two loops are equivalent
$ NAMES="Eric Adriadne Katharina Dixon"
$ for NAME in $NAMES
$ do
$ echo $NAME
$ done
$ # translate space-separated text into newline-separated text, "\n" is a newline
$ echo $NAMES | tr ' ' '\n' > namesfile.txt
$ for NAME in $(cat namesfile.txt)
$ do
$ echo $NAME
$ done
```

Note that Bash is rather loose about how you arrange these statements, you can write "do" on its own line, or alongside the first line of the looped operations e.g. `do echo $NAME`. What is nice is that Bash lets you type out loops over multiple lines: normally, when we insert a newline character by pressing <kbd>Enter</kbd> or <kbd>Return</kbd> the command line executes whatever text is the on prompt. But because Bash sees the "for" statement it knows to wait until it sees a corresponding "done" before executing our code.

Bash also has [`case`](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_03.html) conditions, where you check a series of conditions and can specify a fallback if not are met, and [`while`](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_09_02.html) loops that execute as long as a condition is true. These are less frequently used (I use them a tiny fraction of the time that I use `if` and `for`) and we won't cover them.

What about passing parameters to scripts? Let's write a script named "listnames.sh" that is like our demonstrative loops above but it accepts the names as parameter.

```sh
#!/usr/bin/env bash
NAMES=$1
for NAME in $NAMES
do echo $NAME
done
```

There are several special uses of `$` and `$1` refers to the _first_ argument passed to our script. Analogously, `$2` will be the second argument, etc. So to finish our example by making our script executable and running it we can do:

```sh
$ chmod +x listnames.sh
$ ./listnames.sh "John Jacob Jingleheimer Schmidt"
```

A final note about executing programs: we will frequently run into commands that our user is not allowed to execute, or which attempt to modify portions of the file or operating system we are not allowed to modify. Common examples include updating software with `softwareupdate` on Mac or `apt-get update` on Linux.

In these instances, we can use the `sudo` command as a prefix, which stands for **su**peruser **do**. So if we run `sudo softwareupdate` and then type in our password when prompted, `sudo` runs `softwareupdate` as the root, administrative user. `sudo` is one of the first _interactive_ commands we've seen, where the prompt pauses and awaits our input. `sudo` also lets you run commands as other, non-root, users. It is common to need to run commands on a web server as the same user as the web server software e.g. `sudo -u apache ./clear_cache.php` might run a "clear_cache.php" PHP script as the "apache" user.

It should go without saying that allowing programs to run as the root user is inherently dangerous, as the elevated permissions allow almost anything to happen. Only run trusted programs with `sudo`.
{: .warn }

## Customizing _our_ shell

We've written a script, specific its interpreter, and made it executable. But it's still not really like builtin commands like `ls` or `sort` because we have to reference it's full path every time we use it, whether by typing `./script.sh` or `/Users/username/Code4Lib-CLI-Workshop/script.sh`. How come we don't need to use the full path to other commands? Let's use this problem as an impetus to learn how to customize the shell and truly make it our own.

The `$PATH` variable is a nuanced feature of most shells and the cause of a great deal of errors. `PATH` is a list of folders where the shell looks for executable programs. Since it knows about these programs, it doesn't require you to type out their full path and they are available for tab-completion. Very often, when we install software like node.js, python, or ruby programs we will need to modify our PATH to search in the folders where these languages install executables.

What are the contents of your `$PATH`? Try running `echo $PATH` to find out. Note that the folder paths are separated by colons `:`. Because our "Code4Lib-CLI-Workshop" folder is not listed, executables in it cannot be referenced directly by their name. To modify our PATH we append a new folder's path _to itself_ like so:

Input
{: .label .label-green}
```sh
$ PATH="$PATH:/Users/ephetteplace/Code4Lib-CLI-Workshop"
$ echo $PATH
```

Output
{: .label .label-yellow}
```sh
/usr/local/bin:/usr/local/sbin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/ephetteplace/Code4Lib-CLI-Workshop
```

(A neater way to do this is to use a subshell: `PATH="$PATH:"$(pwd)` appends our current location to the `PATH`).

Isn't appending something to itself what caused an infinite loop that crashed our hard drive earlier? Yeah, it is, but this problem doesn't apply to variables, go figure.

Your `PATH` may look different but should contain a bunch of similar-looking paths ending in "bin" for **bin**ary, which I think is an artifact of the days when the only programs were compiled binaries. Now what happens if we run `script.sh` without a full path? What will happen if we move to a different location and run it?

Modifying the `PATH` has been helpful, but now every time we open our terminal, if we have a folder of scripts we want to use, we have to add them to the `PATH` again. It would be better if Bash knew that we wanted to perform certain `PATH` modifications every time we ran the shell. Luckily, every time we run Bash it looks for a special ".bash_profile" file in our user's home folder and executes it like a script. In fact, we may already have a ".bash_profile" file without even knowing it.

Let's do more than just modify our `PATH`, let's set up some convenient shortcuts. I run some commands so frequently that reducing them from four characters to just one is actually a meaningful improvement. The `alias` command lets us map longer commands to short aliases which, when entered into the CLI, are expanded as if Bash ran a search-and-replace just before execution. So let's open .bash_profile and add a few lines to it:

```sh
# .bash_profile
export PATH="$PATH:/Users/ephetteplace/Code4Lib-CLI-Workshop"
# a better, more-informative list
alias l="ls -al"
# a "count lines" command
alias cl="wc -l"
```

`export` is another new command, it means that the environmental variable we're changing (in this instance, `PATH`) will be exported to any subshells. In general, anytime we change a variable in .bash_profile, we will want to `export` it or else the scripts we write may not work as expected.

@TODO prompt, PS1?

## A Conclusion of sorts

It makes sense that many people, even those in information professions, do not use the command line. It is archaic, pedantic, error-prone, and full of weird conventions dating back to a bygone era of computing. But I hope that these final sections pointed out two pieces of its immense power: interoperability and customizability. No doubt, as you have been working through these materials, certain things have seemed senseless or frustrating. But setting up a few thoughtful aliases can alleviate much of the CLI's problems. Writing out a long script to perform a series of data munging operations can save _hours_ of labor each time you use it. The command line is from an older era of computing, yes, but also an era largely devoid of opacity, of black boxes. There is an incredible freedom in being able to specify the most subtle nuances of the environment you work within.

To become familiar with the command line, I forced myself to perform some basic tasks—managing files, editing simple text, renaming things—there even though it was less convenient than using a GUI file system. Eventually, I got to the point where I could identify tasks suitable for automation, so I applied the same tenant: even if it meant taking up much more time initially, I wrote scripts to perform repeated tasks. Now, I spend a lot of time on the CLI not because it's efficient but because it's enjoyable. It welcomes you to find creative solutions that involve piecing together complex operations from the tiny building blocks of focused but deceptively powerful tools like  `sort` and `tail`. Hopefully. the tools and examples listed on [the further learning page](../further-learning) inspire you to find similar solutions in your own life.
