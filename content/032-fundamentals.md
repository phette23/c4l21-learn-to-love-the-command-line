---
layout: default
nav_order: 3
title: Fundamentals of the command line
permalink: /fundamentals/
---

# Fundamentals of the command line

This will be a lot of information all at once. Don't worry if you're not understanding every word and please feel free to interrupt with questions.

## What is the what?

First of all, the words "command line", "terminal", and "shell" are all used somewhat indiscriminately but mean slightly different things.

A **terminal** or **terminal emulator** is an graphical application wrapped around a shell. It will have settings about your defaults and appearance, for instance. Examples of terminals include: Terminal.app, iTerm.app, Git Bash.exe, & GNOME Terminal.

A command-line **shell** is a piece of software that receives and interprets text-based commands that interact with the operating system. They have their own unique scripting syntax and are closely related to programming languages. Examples of shells include: bash, zsh, fish, and Windows PowerShell. We will be using Bash in this workshop because it is very common and comes installed by default on most UNIX-like systems, such as Ubuntu and OS X.

A **command-line interface** (CLI) could be described as simply any interface that is primarily text-based. A shell's syntax is also its CLI, a program like `git` has its own particular CLI, gopher servers have their own CLI. CLI is the most broad and generic term used here because it pertains to both shells and the syntax of various programs we will run via the shell.

What is a **command** exactly? Is it a program? Command is also a vague term. You could think of it as one segment, typically a single line (e.g. text that ends in a newline character which is typically represented as `\n`), of text to be interpreted and executed as a shell. Commands may contain references to **shell builtins** which are the shell's fundamental syntax itself as well as references to executable programs or scripts.

For the most part, these overlapping terms can be used interchangeably without creating too much confusion, but it is good to know their distinctions.

## Fundamentals: Prompt, Syntax, File System Commands

### The Prompt

By default, bash will _not_ show you simply a greater than symbol `>` waiting for you to type text. Instead, almost all shells show you some contextual information alongside the place where you type commands, henceforth referred to as the command prompt. Here's what I see on a stock Git Bash install:

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
ephetteplace at Eric\'s-MacBook-Pro in ~/C/cli-workshop on main*
¿
```

This shows my username (ephetteplace) "at" the name of my laptop, "in" my current location in an abbreviated form (I'm actually at /Users/ephetteplace/Code/cli-workshop but `~` stands in for my home directory /Users/ephetteplace while "Code" is truncated to merely "C"), "on" the "main" git branch. The asterisk after "main" indicates that I have unsaved changes in the git repository and, finally, I use an inverted question mark for my command prompt.

## Important Bash Syntax

Basic command format: `touch newfile.txt`. `touch` is the command and `newfile.txt` is its only argument.

More complicated: `mv -v newfile.txt renamed.txt`. `mv` is the command

Some commands take no arguments, some take one, some take multiple, but most take a variable number of arguments and also have a variety of options that can be specified.

**Spaces matter**. White space separates commands and their options from each other. It is syntactically significant



`mkdir My Folder` vs. `mkdir MyFolder`

[ Coral's slide deck has most of the information I want to get to here https://docs.google.com/presentation/d/14oisMTEG-O-_DnHSfBRCLsuRq041VnJt9qFOAiVhdpI/edit#slide=id.p ]
