---
layout: default
nav_order: 3
title: Fundamentals of the command line
permalink: /fundamentals/
has_children: true
---

# Fundamentals of the command line

This will be a lot of information all at once. Don't worry if you're not understanding every word and please feel free to interrupt with questions.

## What is the what?

First of all, the words "command line", "terminal", and "shell" are all used somewhat indiscriminately but mean slightly different things.

A **terminal** or **terminal emulator** is an graphical application wrapped around a shell. It will have settings about your defaults and appearance, for instance. Examples of terminals include: Terminal.app, iTerm.app, Git Bash.exe, & GNOME Terminal.

A command-line **shell** is a piece of software that receives and interprets text-based commands that interact with the operating system. They have their own unique scripting syntax and are closely related to programming languages. Examples of shells include: bash, zsh, fish, and Windows PowerShell. We will be using Bash in this workshop because it is very common and comes installed by default on most UNIX-like systems, such as Ubuntu and OS X.

A **command-line interface** (CLI) could be described as simply any interface that is primarily text-based. A shell's syntax is also its CLI, a program like `git` has its own particular CLI, gopher servers have their own CLI. CLI is the most broad and generic term used here because it pertains to both shells and the syntax of various programs we will run via the shell.

What is a **command** exactly? Is it a program? Command is also a vague term. You could think of it as one segment, typically a single line of text, to be interpreted and executed as a shell. Commands may contain references to **shell builtins** which are the shell's fundamental syntax itself as well as references to executable programs or scripts, which are software added to the OS.

## Fundamentals: Prompt, Syntax, Files & Folders, Tricks

### The Prompt

By default, bash will _not_ show us simply a dollar sign symbol `$` waiting for us to type text. Instead, almost all shells show us some contextual information alongside the place where we type commands, henceforth referred to as the command prompt. Here's what I see on a stock Git Bash install:

```sh
phette23@ComputerName MINGW64 ~
$
```

`phette23` is the name of my user account while `ComputerName` is the name of my laptop, I'm honestly not sure what `MINGW64` is (I suspect it's something to do with the OS architecture), and in bash the tilde `~` is a special character representing your user's home folder. Here, it shows my current location; I am _in_ my home folder. All commands are executed from a specific spot in your file system.

On a Mac, you might see a much simpler prompt:

```sh
bash-3.2$
```

This shows the name of the shell ("bash"), the shell's version number (3.2), and the usual dollar sign prompt.

Do you see something different from one of the above? Can you figure out what the different elements of your prompt are? If we have time, later we can cover customizing your prompt to look the way you want. This is what mine looks like:

```sh
ephetteplace at Erics-MacBook-Pro in ~/C/cli-workshop on main*
¿
```

This shows my username (ephetteplace) "at" the name of my laptop, "in" my current location in an abbreviated form (I'm actually at /Users/ephetteplace/Code/cli-workshop but `~` stands in for my home directory /Users/ephetteplace while "Code" is truncated to merely "C"), "on" the "main" git branch. The asterisk `*` after "main" indicates that I have unsaved changes in the git repository. Finally, I use an inverted question mark for my command prompt.

"Directory" is an older term for "folder". You will see that command-line tools tend to reference directories in their names and documentation. I try to say "folder" but know that they are the same thing.
{: .note}

### Important bash syntax

Let's cover the most important pieces of bash syntax. But first, we want to enter a special folder where we can play around, so we don't accidentally mess with our user's home folder. Let's run a couple commands:

Input
{: .label .label-green }
```sh
$ mkdir Code4Lib-CLI-Workshop
$ cd Code4Lib-CLI-Workshop
```

This may seem deceiving but we actually shouldn't see _any_ output from either of these commands, they are both silent. However, if your prompt shows your current location, you might notice that the second `cd` ("**c**hange **d**irectory") command changed it. We have created, and then entered, a folder named "Code4Lib-CLI-Workshop".

There's already a lot going on here. Why did we choose such an awkward folder name? Let's step back a moment to talk about the format of a command by looking at an example:

```sh
$ touch newfile.txt
```

`touch` is the command and `newfile.txt` is its only argument.

More complicated:

```sh
$ mv -v newfile.txt renamed.txt
```

`mv` is the command, `-v` is an option—also known as a "flag" (because the hyphen makes it look like a little horizontal flag post, I suppose?)—which stands for "verbose" in this context, "newfile.txt" is the first argument, and "renamed.txt" is the second argument.

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

"Move" `mv` is another command that is normally silent when everything goes well, but the "verbose" `-v` flag makes it tell you exactly what happened.

**Spaces matter**. White space separates commands and their options from each other. It is syntactically significant character. Think of a programming language like JavaScript; you execute a function like so:

```js
printThis("first", "and then this second")
```

Two strings are passed as arguments to the "printThis" function. Double quotes wrap text to create a string literal, while a comma separates the two string arguments. Unlike many programming languages, the command line _does not_ use commas to separate arguments, it uses spaces. What do you think the difference between `touch my file.txt` versus `touch myfile.txt` is?

Now, perhaps, we see why we named the folder Code4Lib-CLI-Workshop. This name has no spaces in it which makes it more convenient to reference on the command line. You don't _have_ to name everything without spaces, but it helps. How can you work around this? There are two ways: backslash escaping spaces or wrapping arguments in quotes.

```sh
$ touch my\ file.txt
$ touch "my file.txt"
```

These two commands are equivalent. The backslash `\` character "escapes" the character that follows it, meaning that the character is interpreted as mere text with no special meaning. If you want to put a tilde, dollar sign, or space in a file name on the command line, you will have to escape them.

### Arguments and options vary

Some commands take no arguments, some take one, some take multiple, but most take a variable number of arguments and also have a variety of options that can be specified. Many commands do subtlety different things depending on the nature of the arguments they receive. Let's continue looking at the `mv` command as our example, building on some other commands we've used already.

```sh
$ touch "my file.txt" another.txt
$ mkdir Subfolder
$ mv "my file.txt" another.txt Subfolder
$ mv Subfolder/another.txt .
```

The period `.` here means "the current folder", it references the folder we're in, "Code4Lib-CLI-Workshop".

What was happening as we ran these commands? `mv` does much more than we might expect. It moves already-existing files, yes, but it can also rename files, rename folders, as well as move _multiple_ files at once if you specify a folder as a destination.

How do we know that `mv` has this syntax? We could look it up online but most command line program's are self-documenting. There is a `man` command which takes another command as its argument; `man mv` shows you the **man**ual for `mv`.

Unfortunately, `man` isn't available on Git Bash, but that's OK. Sometimes commands have a `--help` flag (often with a `-h` shortcut but in this instance `mv` does not) that inform you about their usage. Run `mv --help` if you're on Git Bash to see help documentation.

Let's read through these together to get a sense of how to understand documentation for commands.

How do you know whether to use `man` or a `--help` flag or something else? There is no surefire way to know unless you've used the command before. It is safe to speculatively try these most common approaches. Don't be afraid! Error messages will often give you clues as to how to find help.

### More file system commands

Let's quickly cover several more useful and common commands. To start with, one of the most-used commands will likely be `ls` for "list". `ls` shows you what files and folders are in your current directory:

Input
{: .label .label-green}
```sh
$ ls
```

Output
{: .label .label-yellow}
```sh
Subfolder   another.txt     my file.txt     renamed.txt
```

`ls` is a command with _a lot_ of options. Notice how the "Subfolder" directory looks the same as a regular file above? Let's look at the command's help documentation to see if we can get more informative output. As a good default, I like to use `ls -Gl` which colorizes output (the "G" flag) and lists items in "long" format which shows a lot more information than merely a file's name.

Brief digression: commands can have single hyphen flags like `ls -l` and double-hyphenated flags like `bash --version`. Often, the single letter is shorthand for a longer one, but not always (many of these fundamental commands we are discussing do not have longform flags). The single hyphen flags can be combined such that `-G -l` is the same as `-Gl`. When I am typing on the command line I almost always use the short flags for convenience, but when I write a bash script I spell out the long flag to make it more transparent (`-v` might mean "verbose" or "version" depending on the command, for instance).

`pwd` **p**rints your current **w**orking **d**irectory i.e. where you currently are.

Input
{: .label .label-green}
```sh
$ pwd
```

Output
{: .label .label-yellow}
```sh
/Users/ephetteplace/Code/cli-workshop
```

`rm` **r**e**m**oves files. Let's use it to clean up some of the empty text files we've been creating.

**`rm` is probably the most dangerous command we will learn in this workshop**. The command line does not put files in a Recycling Bin purgatory, it erases them from existence. Always be _extra_ careful with `rm`. When in doubt, try using the "interactive" `-i` flag which asks you for permission first before deletion.
{: .warn}

Input
{: .label .label-green}
```sh
$ rm "my file.txt" renamed.txt
$ rm -v another.txt
```

Output
{: .label .label-yellow}
```sh
another.txt
```

`rm` looks a lot like `mv`—you can pass one or multiple files to it and it removes them all, but it won't tell you anything until you ask it to be verbose with the `-v` flag.

`rmdir` is like remove but for **dir**ectories. Try `rmdir Subfolder`. Did it work? `rmdir` only works on empty folders. Honestly, I never use it, because `rm` has a **r**ecursive `-r` flag which makes it more useful. If we want to delete everything in our subfolder we can run:

Input
{: .label .label-green}
```sh
$ rm -r Subfolder
```

Finally, let's introduce two new special characters: `..` and `*`. Two periods in a row refers to the _parent_ of the current directory. So `..` means Code4Lib-CLI-Workshop when we're in Subfolder, and it means our user's home folder when we're in Code4Lib-CLI-Workshop. Why is this important? Well, it can help us navigate the command line more efficiently:

```sh
$ cd ..
```

This commands _moves us up the folder hierarchy into the parent of our current location_. It would otherwise be quite a pain to type out a full path like /Users/ephetteplace/Code4Lib-CLI-Workshop just to move one level up from our Subfolder.

The `*` actually operates much like a wildcard in a search engine; it matches _everything_. This makes it very powerful and also a little bit dangerous. Below, we move into our Subfolder, move all the contents into the parent, then move ourselves into the parent:

```sh
$ cd Subfolder
$ mv * ..
$ cd ..
```

Note that we could have done this without changing our location by running `mv Subfolder/* ..`. This highlights another power of `*`: you can put text _in front_ or _behind_ it, just like a wildcard. The command below moves _only files that end in .txt_ into our subfolder:

```sh
$ mv *.txt Subfolder
```

The way this works is the shell looks for potential matches and expands the `*` into them. So `mv *.txt Subfolder` is equivalent to `mv another.txt renamed.txt "my file.txt" Subfolder`. Most commands are written to work with `*` naturally, thus why they accept variable numbers of arguments. The way an asterisk expands into its matching arguments is called "globbing" for some reason.

| Character | Meaning
| --- | ---
| [space] | separates commands, arguments, and options
| \ | "escape" the following character
| ~ | User's home folder e.g. /Users/username
| . | The current location, the folder you're in
| .. | The parent folder of your current location
| * | wildcard

We will see a few more special characters later, like `$`, `&`, and `|`.

### Niceties: quicker typing & cursor movement

You have probably already noticed one of the more annoying things about the command line by now: typos. The command line, like every programming language, is extremely pedantic. It will simply fail if you mistype a command or one of it's arguments. You may have spent some time deleting text to rewrite it. Here are a few tips that help us write accurate commands with less effort.

First of all, all modern shells come with a tab-completion feature that tries to intelligently fill in the end of a term based on the text you've already entered. Remember our lengthy "Code4Lib-CLI-Workshop" folder name? If we are in the _parent_ folder that contains it, we can type something like "cd Co<kbd>&lt;tab&gt;</kbd>" and the folder name appears on our prompt. You always have to type enough to uniquely identify the object you are referencing.

Say we have two files "another.txt" and "another_one.txt" in our folder. If we type "rm a<kbd>&lt;tab&gt;</kbd>" then the command cannot complete because it cannot know which file we're trying to remove. But it _does_ fill out as much text as it can—the phrase "rm another" appears because, up until that point, our two filenames are the same.

Another handy trick: the arrow keys ⬆️ and ⬇️ move up and down through your command history. If you mistyped your last command, it's often useful to up-arrow to it, edit, and run again.

But what if you mistyped _the very beginning_ of your command, like writing `tuch a-very-very-long-filename.txt`? Up-arrow doesn't save a ton of time because you have to delete everything to fix the `touch` typo. Never fear! These handy keyboard shortcuts can help you jump around the line to edit specific parts:

| Shortcut | Action
| --- | ---
| Ctrl + A | Move the cursor to the start of the prompt
| Ctrl + E | Move the cursor to the end of the prompt
| Ctrl + K | Delete from the cursor to the end of the line
| Ctrl + W | Delete the word closest to the cursor
| Ctrl + L | Clear the contents of the terminal window
| Ctrl + C | Cancel the currently-running command (great way to break an infinite loop)

Those last two are simply useful to know and not related to cursor navigation. These aren't easy to remember but they're so useful I recommend trying to use them until they become second nature.
