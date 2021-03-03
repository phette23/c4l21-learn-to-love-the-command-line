---
layout: default
nav_order: 3
title: Fundamentals - Exercises
permalink: /fundamentals/exercises
parent: Fundamentals of the command line
---

# Fundamentals Exercises

These exercises are meant to make you comfortable with the command line: typing commands, moving around, creating/renaming files and folders.

## 1. Setup

Using the command line: create a new folder, anywhere on your laptop. Then inside that folder create at least three plain text ".txt" files and a subfolder with at least one file in it. Print the full contents of your new folder in a way such that a) everything it contains is on its own line, b) the subfolder is highlighted as a folder and not merely another file, and c) the date created of the contents is visible.

Move two of your text files into your subfolder. Now, move _all_ of the contents of the subfolder out of it and back into its parent _using only a single command_. Hint: think about some of the special characters we know about it. Now delete the (empty) subfolder. Confirm that the subfolder is gone yet you still have all your files by listing the parent folder's contents again.

<details>
<summary>Open here to the solutions</summary>

<div class="language-sh highlighter-rouge">
<div class="highlight"><pre class="highlight">
<code>$ touch one.txt two.txt three.txt
$ mkdir Subfolder
$ touch Subfolder/four.txt
$ ls -Gl
$ mv one.txt two.txt Subfolder
$ mv Subfolder/* .
$ rmdir Subfolder
$ ls</code></pre></div></div>
</details>

## 2. Files abound
