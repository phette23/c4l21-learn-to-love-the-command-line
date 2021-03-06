---
layout: default
nav_order: 3
title: CLI Fundamentals
permalink: /fundamentals/
has_children: true
---

# Fundamentals of the command line

This will be a lot of information all at once. Don't worry if you don't understand every word and please feel free to interrupt me with questions.

## What is the what?

First of all, the words "command line", "terminal", and "shell" are all used somewhat indiscriminately but mean slightly different things.

A command-line **shell** is a piece of software that interprets text-based instructions to interact with the operating system. Shells have their own unique scripting syntax and are closely related to programming languages. Examples of shells include: `bash`, `zsh`, `fish`, and Windows PowerShell. We will be using Bash in this workshop because it is common and comes installed by default on most UNIX-like systems, such as Ubuntu and Mac OS X.

A **terminal** or **terminal emulator** is a graphical application wrapped around a shell. It will have settings about your defaults and appearance, for instance. Examples of terminals include: Terminal, [iTerm2](https://iterm2.com/), [Git Bash](https://gitforwindows.org/), & GNOME Terminal.

A **command-line interface** (CLI) could be described as any interface that is primarily text-based. A shell's syntax is also its CLI, a program like `git` has its own particular CLI, gopher servers have their own CLI. CLI is the most broad and generic term used here because it pertains to both the shell and the syntax of various programs we will run via the shell.

A **path** is a text representation of a hierarchical system of folders. On the command line, paths are shown as a forward-slash "/" separated list starting at the very root of our file system. So "/Users/ephetteplace/My Folder" is the path to "My Folder", which is inside its parent folder "ephetteplace", which is in turn inside "Users", which is finally under the root of the entire file system.

What is a **command** exactly? Is it a program? A script? A magic spell? Command is another vague term. We can think of it as one segment, typically a single line of text, to be interpreted and executed by a shell. Commands may contain references to **shell builtins**, which are the shell's fundamental syntax, as well as references to executable programs or scripts, which are additional software installed in the operating system.

## Fundamentals: Prompt, Syntax, Files, Folders, & Tricks

We will begin by learning what the command line looks like, transition into its syntax and common file operations, then conclude with some handy tricks to make using bash easier.

### The Prompt

By default, bash will _not_ show us simply a dollar sign symbol `$` waiting for us to type text. Instead, most shells show us contextual information alongside the place where we type commands, henceforth referred to as the command prompt. Here's what I see on Git Bash on Windows:

```sh
phette23@ComputerName MINGW64 ~
$
```

`phette23` is the name of my user account while `ComputerName` is the name of my laptop, I'm honestly not sure what `MINGW64` is (I suspect it's something to do with the OS architecture), and in bash the tilde `~` is a special character representing your user's home folder. Here, it shows my current location; I am in my home folder. All commands are executed from a specific spot in your file system.

On a Mac, you might see a simpler prompt:

```sh
bash-3.2$
```

This shows the name of the shell ("bash"), the shell's version number (3.2), and the usual dollar sign.

Do you see something different from one of the above? Can you figure out what the different elements of your prompt are? If we have time, later we will cover customizing the prompt's appearance. This is what mine looks like:

```sh
ephetteplace at Erics-MacBook-Pro in ~/C/cli-workshop on main*
¿
```

This shows my username (ephetteplace) "at" the name of my laptop, "in" my current location in an abbreviated form, "on" the "main" git branch. The asterisk `*` after "main" indicates that I have unsaved changes in my git repository. Finally, I use an inverted question mark as my command prompt.

Some commands span multiple lines. When this happens, the prompt character usually changes to indicate that we're not finished writing the current command. Here's an example where our `echo $0` command is split onto two lines:

```sh
$ echo \
> $0
bash
```

The command produces the same output but is split across two lines, the first with a `$` prompt and the second with `>`. If we see a `>` while typing a command, we know that the command line is telling us it expects something additional before it can run.

### Important bash syntax

Let's cover the most important pieces of bash syntax. But first, we want to enter a special folder where we can play around, so we don't accidentally mess up our home folder. Let's run a couple commands:

Input
{: .label .label-green }
```sh
$ mkdir Code4Lib-CLI-Workshop
$ cd Code4Lib-CLI-Workshop
```

This may be unexpected but we actually don't see output from either of these commands, they are both silent. However, if your prompt shows your current location, you might notice that the second `cd` ("**c**hange **d**irectory") command changed it. We have created, and then entered, a folder named "Code4Lib-CLI-Workshop".

"Directory" is an older term for "folder". We will see that command-line tools tend to reference directories in their names and documentation. I will try to say "folder" instead but know that they are the same thing.
{: .note}

There's already a lot going on here. Why did we choose such an awkward folder name? Let's step back a moment to talk about the format of a command by looking at another example:

```sh
$ touch newfile.txt
```

`touch` is the command and `newfile.txt` is its only argument.

Another, more complicated, command:

Input
{: .label .label-green}
```sh
mv -v newfile.txt renamed.txt
```

Output
{: .label .label-yellow }
```sh
renamed 'newfile.txt' -> 'renamed.txt'
```

`mv` is the command, `-v` is an option—also known as a "flag" (because the hyphen makes it look like a little horizontal flag post, I suppose?)—which stands for "verbose" in this context, "newfile.txt" is the first argument, and "renamed.txt" is the second argument. "Move" `mv` is another command that is normally silent when everything goes well, but the "verbose" `-v` flag makes it tell us exactly what happened.

**Spaces matter**. White space separates commands and their options from each other. It is syntactically significant character. Think of a programming language like JavaScript; you execute a function like so:

```js
printThis("first", "and then this second")
```

Two strings are passed as arguments to the "printThis" function. Double quotes wrap text to create a string literal, while a comma separates the two string arguments. Unlike many programming languages, the command line _does not_ use commas to separate arguments, it uses spaces. What do you think the difference between `touch my file.txt` and `touch myfile.txt` is?

Now, perhaps, we see why we named the folder Code4Lib-CLI-Workshop. This name has no spaces in it which makes it more convenient to reference on the command line. We don't _have_ to name everything without spaces, but it helps. How can we reference names with spaces in them? There are two ways: backslash escaping spaces or wrapping arguments in quotes.

```sh
$ touch my\ file.txt
$ touch "my file.txt"
```

These two commands are equivalent. The backslash `\` character "escapes" the character that follows it, meaning that the character is interpreted as mere text with no special meaning. If we want to put a tilde ~, dollar sign $, or space in a file name on the command line, we have to escape them.

### Arguments and options vary

Some commands take no arguments, some take one, some take multiple, but most take a variable number of arguments and also have a variety of option flags that can be specified. Many commands do subtlety different things depending on the nature of the arguments they receive. Let's continue looking at the `mv` command as our example, building on some other commands we've used already.

```sh
$ touch "my file.txt" another.txt
$ mkdir Subfolder
$ mv -v "my file.txt" another.txt Subfolder
$ mv -v Subfolder/another.txt .
```

The period `.` here means "the current folder", it references the folder we're in, "Code4Lib-CLI-Workshop".

What was happening as we ran these commands? `mv` does much more than we might expect. It moves already-existing files, yes, but it can also rename files, rename folders, as well as move _multiple_ files at once if you specify a folder as a destination.

How do we know that `mv` has this syntax? We could look it up online but most command line tools are self-documenting. There is a `man` command which takes another command as its argument; `man mv` shows you the **man**ual for `mv`. We exit the manual by pressing <kbd>q</kbd> for **q**uit.

Unfortunately, `man` isn't available on Git Bash, but that's OK. Sometimes commands have a `--help` flag (often with a `-h` shortcut, though `mv` does not) that inform you about their usage. Run `mv --help` if you're on Git Bash to see help documentation.

Let's read through these together to get a sense of how to understand documentation for commands. Command documentation starts by writing out the basic structure of using command, often showing us some of the flag options. Then, beneath that, is a list of the flags and what they do.

```
NAME
     mv -- move files

SYNOPSIS
     mv [-f | -i | -n] [-v] source target
     mv [-f | -i | -n] [-v] source ... directory

DESCRIPTION
     In its first form, the mv utility renames the file named by the source
     operand to the destination path named by the target operand. This form is
     assumed when the last operand does not name an already existing directory.

     In its second form, mv moves each file named by a source operand to a
     destination file in the existing directory named by the directory operand.
     The destination path for each operand is the pathname produced by the
     concatenation of the last operand, a slash, and the final pathname
     component of the named file.

     The following options are available:

     -f      Do not prompt for confirmation before overwriting the destination.
     (The -f option overrides any previous -i or -n options.)

     -i      Cause mv to write a prompt to standard error before moving a file
     that would overwrite an existing file. If the response from the standard
     input begins with the character 'y' or 'Y', the move is attempted.
     (The -i option overrides any previous -f or -n options.)

     -n      Do not overwrite an existing file.
     (The -n option overrides any previous -f or -i options.)

     -v      Cause mv to be verbose, showing files after they are moved.
```

How do we know whether to use `man` or a `--help` flag or something else? There is no surefire way to know unless we've used the command before. It is safe to speculatively try these most common approaches. Don't be afraid! Error messages often give clues as to how to find help.

### More file system commands

Let's quickly cover several more useful commands. To start with, one of the most-used commands is `ls` for "list". We've been creating files and folders and moving them around, but doing so in the dark. `ls` shows us what files and folders are in our current directory:

Input
{: .label .label-green}
```sh
$ ls
```

Output
{: .label .label-yellow}
```sh
Subfolder   another.txt     renamed.txt
```

`ls` is a command with _a lot_ of options. Notice how the "Subfolder" directory looks the same as a regular file above? Let's look at the command's help documentation to see if we can get more informative output. As a good default, I like to use `ls -Gl` which colorizes output (the "G" flag) and lists items in "long" format which shows a lot more information than merely a file's name.

**Flags come in a few forms**. Commands can have single hyphen flags like `ls -l` and double-hyphenated flags like `bash --version`. Often, the single letter is shorthand for a longer one, but not always (these fundamental commands we are discussing do not have longform flags). The single hyphen flags can be combined such that `-G -l` is the same as `-Gl`. When I am typing on the command line I almost always use the short flags for convenience, but when I write a bash script I spell out the long flag to make it more transparent (`-v` might mean "verbose" or "version" depending on the command, for instance).

`pwd` **p**rints your current **w**orking **d**irectory i.e. where you currently are.

Input
{: .label .label-green}
```sh
$ pwd
```

Output
{: .label .label-yellow}
```sh
/Users/ephetteplace/Code4Lib-CLI-Workshop
```

`rm` **r**e**m**oves files. Let's use it to clean up some of the empty text files we've been creating.

**`rm` is probably the most dangerous command we will learn in this workshop**. The command line does not put files in a Recycling Bin purgatory, it erases them from existence. Always be _extra_ careful with `rm`. When in doubt, use the "interactive" `-i` flag which asks for permission before deleting.
{: .warn}

Input
{: .label .label-green}
```sh
$ rm "Subfolder/my file.txt" renamed.txt
$ rm -v another.txt
```

Output
{: .label .label-yellow}
```sh
removed 'another.txt'
```

`rm` looks a lot like `mv`we can pass one or multiple files to it and it removes them all, but it won't tell us anything until we ask it to be verbose with the `-v` flag.

`rmdir` is like remove but for **dir**ectories. Try `rmdir Subfolder`. Did it work? `rmdir` only works on empty folders. Honestly, I never use it, because `rm` has a **r**ecursive `-r` flag which makes it more useful. If we want to delete everything in our subfolder we can run:

```sh
$ rm -r Subfolder
```

Finally, let's introduce two new special characters: `..` and `*`. Two periods in a row refers to the _parent_ of the current directory. So `..` means Code4Lib-CLI-Workshop when we're in Subfolder, and it means our user's home folder when we're in Code4Lib-CLI-Workshop. Why is this important? It helps us navigate the command line more efficiently:

```sh
$ cd ..
```

This commands _moves us up the folder hierarchy into the parent of our current location_. It would otherwise be a pain to type out the full path /Users/ephetteplace/Code4Lib-CLI-Workshop just to move one level up from our Subfolder.

The `*` actually operates much like a wildcard in a search engine; it matches _everything_. This makes it very powerful and also a little bit dangerous. Below, we move into our Subfolder, move all the contents into the parent, then move ourselves into the parent:

```sh
$ mkdir Subfolder
$ touch Subfolder/newfile.txt
$ cd Subfolder
$ mv * ..
$ cd ..
```

Note that we could have done this without changing our location by running `mv Subfolder/* ..`. This highlights another power of `*`: we can put text _in front_ or _behind_ it, just like a wildcard in a search engine. The command below moves _only files that end in .txt_ into our subfolder:

```sh
$ touch another.txt
$ touch one.txt
$ mv -v *.txt Subfolder
```

The way this works is the shell looks for potential matches and expands the `*` into them. So `mv *.txt Subfolder` is equivalent to `mv another.txt newfile.txt one.txt Subfolder`. Most commands are written to work with `*` naturally, thus why they accept variable numbers of arguments. The way an asterisk expands into its matching arguments is called **globbing** for some reason.

| Character | Meaning
| --- | ---
| [space] | separates commands, arguments, and options
| \ | "escape" the following character
| ~ | User's home folder e.g. /Users/username
| . | The current location, the folder you're in
| .. | The parent folder of your current location
| * | wildcard

We will see a few more special characters later, like `$`, `&`, and `|`. But first let's return to `ls` for a minute to look at a new flag, `-a` for "all".

Input
{: .label .label-green}
```sh
$ mv Subfolder/*.txt .
$ ls -Gal
```

Output
{: .label .label-yellow}
```sh
drwxr-xr-x  12 ephetteplace  staff   384 Mar  4 15:12 .
drwxr-xr-x  25 ephetteplace  staff   800 Mar  1 15:07 ..
drwxr-xr-x  12 ephetteplace  staff   384 Mar  4 15:30 Subfolder
-rw-r--r--   1 ephetteplace  staff    36 Mar  4 16:09 another.txt
-rw-r--r--   1 ephetteplace  staff    36 Mar  4 16:05 newfile.txt
-rw-r--r--   1 ephetteplace  staff    36 Mar  4 16:09 one.txt
```

We can understand more now that we know the special meanings of `.` and `..`. The "all" flag also shows us _hidden files_ (filenames that begin with a period, ".gitignore" and ".DS_Store" are two common ones). What about `ls -Gal Subfolder`? `ls` isn't restricted to the current folders' contents, we can list another folder without even changing location.

One final file system command: `cp`. Any guesses are to what it does? Try to find its documentation if you cannot guess.

The `cp` syntax should be reminiscent of what we've seen elsewhere: it has a `-v` flag for "verbose", an `-r` flag for "recursive", and like `mv` it can take a variable number of files if it's last argument is a destination directory. Here is a quick demonstration of its use:

Input
{: .label .label-green}
```sh
$ touch original
$ cp -v original duplicate
$ mkdir -p ParentSubfolder/Child
$ cp -v original duplicate ParentSubfolder/Child
$ cp -rv ParentSubfolder CopyParentSubfolder
```

Output
{: .label .label-yellow}
```sh
original -> duplicate
original -> ParentSubfolder/Child/original
duplicate -> ParentSubfolder/Child/duplicate
ParentSubfolder -> CopyParentSubfolder
ParentSubfolder/Child -> CopyParentSubfolder/Child
ParentSubfolder/Child/original -> CopyParentSubfolder/Child/original
ParentSubfolder/Child/duplicate -> CopyParentSubfolder/Child/duplicate
```

Notice three things: 1) we learned a new `mkdir` flag, `-p`, which scaffolds out an entire folder hierarchy all at once, 2) `cp`'s **r**ecursive `-r` flag duplicated the entire tree of folders & files, and 3) `cp` accepted argument lists with very different compositions—two files, two files and a folder, two folders—yet they all worked!

While we need to learn many different commands to master the CLI, there are predictable patterns to how commands are structured. Commands almost always have the same kinds of flags (recursion, verbose, help) and they are structured to work well with globbing i.e. with variable numbers of arguments.

### Niceties: quicker typing & cursor movement

We have probably already noticed one of the more annoying things about the command line by now: typos. The command line, like every programming language, is extremely pedantic. Commands fail if we mistype a flag or an argument. We may have spent some time deleting text to rewrite it. Here are a few tips that help us write accurate commands with less effort.

First of all, modern shells come with a **tab-completion** feature that intelligently fills in the end of a term based on the text we've already entered. Remember our lengthy "Code4Lib-CLI-Workshop" folder name? If we are in the _parent_ folder that contains it, we can type something like "cd Co<kbd>&lt;tab&gt;</kbd>" and the complete folder name appears on our prompt.

We always have to type enough to uniquely identify the object we're referencing. Say we have two files "another.txt" and "another_one.txt" in our folder. If we type "rm a<kbd>&lt;tab&gt;</kbd>" then the tab cannot complete because it cannot know which file we're trying to remove. But it _does_ fill out as much text as it can—the phrase "rm another" appears because, up until that point, our two filenames are the same.

Another handy trick: the arrow keys ⬆️ &nbsp;and ⬇️ &nbsp;move up and down through our command history. If we mistyped our last command, it's often time-saving to up-arrow, edit, and run again.

But what if we mistyped _the very beginning_ of our command, like writing `tuch a-very-very-long-filename.txt`? Up-arrow doesn't save much time because we have to delete everything to fix the `touch` typo. Never fear! These handy keyboard shortcuts can help us jump around the line to edit specific parts:

| Shortcut | Action
| --- | ---
| Ctrl + A | Move the cursor to the start of the prompt
| Ctrl + E | Move the cursor to the end of the prompt
| Ctrl + K | Delete from the cursor to the end of the line
| Ctrl + W | Delete the word behind the cursor
| Ctrl + L | Clear the contents of the terminal window
| Ctrl + C | Cancel the current command (one way to break an infinite loop)

Those last two are simply useful to know and not related to cursor navigation. These aren't easy to remember but they're so useful that I recommend trying to use them until they become second nature.
