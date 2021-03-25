---
layout: default
nav_order: 4
title: Fundamentals exercises
permalink: /fundamentals/exercises
parent: CLI Fundamentals
---

# Fundamentals Exercises

These exercises are meant to make you comfortable with the command line: typing commands, moving around, and manipulating files and folders.

## 1. Setup

Using the command line: create a new folder, anywhere on your laptop. Then inside that folder create at least three plain text ".txt" files and a subfolder with at least one file in it. Print the full contents of your new folder in a way such that a) everything it contains is on its own line, b) the subfolder is highlighted as a folder and not merely another file, and c) the date created of the contents is visible.

Move two of your text files into your subfolder. Then move _all_ of the contents of the subfolder out of it and back into its parent _using only a single command_. Hint: think about some of the special characters we know about it. Now delete the (empty) subfolder. Confirm that the subfolder is gone yet you still have all your files by listing the parent folder's contents again.

<details>
<summary><b>Open here to see solutions</b></summary>

<pre><code>$ touch one.txt two.txt three.txt
$ mkdir Subfolder
$ touch Subfolder/four.txt
$ ls -Gl
$ mv one.txt two.txt Subfolder
$ mv Subfolder/* .
$ rmdir Subfolder
$ ls</code></pre>
</details>

## 2. Files abound

We now have several files all sitting in the root of a folder. Copy two of the files, indicating in their name that they are copies. Create a subfolder named "Originals" with another folder named "Copies" within it (hint: you should be able to do this with one command), then put both of the copied files within "Copies".

Now, move all the remaining files left in the root folder to the subfolder we just made. Confirm that all your files are in the right place. Finish by removing _all_ the files we've just created, but make the command ask you if you're sure before each deletion.

<details>
<summary><b>Open here to see solutions</b></summary>

<pre><code>$ cp one.txt one-copy.txt
$ cp two.txt two-copy.txt
$ mkdir -p Originals/Copies
$ mv *-copy.txt Originals/Copies
$ mv *.txt Originals
$ ls Originals/Copies
$ ls Originals
$ rm -ir *</code></pre>
</details>

## 3. Questions

No typing commands here, but a few questions for thought.

**Which of the terms below best applies to `bash`?**

{: .lower-alpha-list }
- shell
- terminal
- GUI
- command

<details>
<summary><b>Open here to the answer</b></summary>

<b>shell</b> is the most suitable term. Bash is technically a command itself—try running <code>bash</code> in your terminal? What happens? Are you in <i>Inception</i>? If you're having fun with this, trying running `echo $SHLVL`, then `exit`, and finally `ECHO $SHLVL` again. What do you think happened?
</details>
<br>

**Why do `mv` and `cp` accept arguments in the order that they do, performing a slightly different sort of operation if a folder is the final argument?**

Consider the difference between these two commands:

```sh
$ mv source.file destination.file
$ mv source1.file source2.file Destination.folder
```

<details>
<summary><b>Open here to the answer</b></summary>

Commands operate differently depending on the order and quantity of their arguments so that they work as well with globbing (more than two arguments) as they do normally. `mv` wants to be support the syntax `mv *.file Destination.folder` thus it must supports a variable number of arguments. Since only a folder can accept multiple moved or copied files, it makes sense that the final argument must be a folder when there are multiple files referenced. The syntax also better mimics common English language structures: "_move_ the files source1 and source2 into the Destination folder" is more readable than "into folder Destination, _move_ the files source1 and source2".
</details>
<br>

**Can you think of one task you've done recently, whether it's a one-time thing or a recurring one, that might have been made easier by using the command line?**

We have learned relatively little so far, but maybe you can already see some hints of how bulk file system management can be more effective in some ways on the command line.
