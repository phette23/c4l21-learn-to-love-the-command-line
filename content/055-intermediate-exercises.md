---
layout: default
nav_order: 8
title: Intermediate exercises
permalink: /intermediate/exercises
parent: Intermediate CLI topics
---

# Intermediate command line Exercises

This expects you to have the [names.txt](https://phette23.github.io/c4l21-learn-to-love-the-command-line/names.txt) file we used earlier.

## 1. Writing a script

Write a script to iterate over a text file of URLs and download any images among them, skipping the others.

<details>
<summary><b>Open here for hints</b></summary>
A regular expression you could use to identify most image URLs would be <code>'\.(jpe?g|png|webp)$'</code>. You will need to use <code>grep</code>'s <code>-E</code> flag. If using <code>curl</code> is proving too tricky try assuming that <code>wget $URL</code> will download a file even if your system does not have <code>wget</code> installed.
</details>
<br>

<details>
<summary><b>Open here to see solutions</b></summary>

<pre><code>#!/usr/bin/env bash
for URL in $(cat urls.txt); do
# grep's "-q" flag silences output, you could also write output to /dev/null
# like grep -E $REGEX >/dev/null (writing to /dev/null is a common pattern)
echo $URL | grep -E -q '\.(jpe?g|png|webp)$' && wget $URL
done</code></pre>
</details>
<br>

## 2. Using `seq` for iteration

`seq` is another simple but useful command that produces **seq**uences of integers. Try running `seq 0 3` and `seq 5 2` to get a sense of its usage. Without using a command we haven't learned yet, how can you _reverse_ the order of the lines in the names.txt file?

Hint: `seq`, `tail`, `head`, and a `for` loop are the tools you need.

<details>
<summary><b>Open here to see solutions</b></summary>

<pre><code>$ LENGTH=$(cat names.txt | wc -l)
$ for NUMBER in $(seq 1 $LENGTH)
$ do
$ tail -n $NUMBER names.txt | head -n1 >> reversed-names.txt
$ done
$ cat reversed-names.txt</code></pre>

<br>The actual, easiest way to do this is to use the <code>tac</code> command which is, in function and name, "reverse cat". It prints a file to stdout starting with the last line.
</details>
<br>

Aside: `seq -w` will ensure all integer strings are the same length by padding them with zeroes, e.g. `seq -w 1 100` goes from 001 to 100, and can be incredibly useful in dealing with filename conventions.

## 3. Questions

**What could most easily be called the fundamental unit of data on the command line?**

{: .lower-alpha-list }
- binary
- a line of text
- a file
- a folder

<details>
<summary><b>Open here to the answer</b></summary>

This is a philosophical question and thus debatable but <b>a line of text</b> is the best answer for reasons we've repeatedly seen during the intermediate topics: data passed through a command pipeline is processed one line a time; when you iterate over a file it is done one line at a time; and we even write out of our commands one line a a time.

"A file" might seem like the answer but it is actually possible to perform many complex operations without ever creating or referencing a file, for instance streaming data from a website, through a series of text manipulations, and back out to the web. Most of the time when we were writing to or accessing files it was more a matter of convenience than a necessity.
</details>
<br>

**Any command can be incorporated into the prompt. What might be some useful information you would want displayed when you use the command line?**

Consider that commands that rely on network requests or very expensive operations make a poor choice, since they will be executed every time a fresh command prompt appears.

**Are loops more suitable for scripts or ad hoc commands typed one-by-one? Same question for piping data between commands: is this more suitable to scripts or ad hoc commands?**

Yes, it's another somewhat subjective question.

<details>
<summary><b>Open here to the answer</b></summary>

Pipelines are elegant and useful while running ad hoc commands one at a time; loops are more powerful (they can more easily use "if" conditions, for instance) and better suited to scripts. Pipelines are more compact than loops so it's easier to type them out. Loops are more verbose and can be tricky to type sometimes because of how they break across multiple lines. Also, in a script, you can store data in a variable while performing repeated operations, making pipes less necessary. Most scripts will use both pipes and loops but overly relying on pipes can be problematic. See <a href='/c4l21-learn-to-love-the-command-line/further-learning#csvkit-is-an-actual-miracle'>my csvkit script</a> example, for instance.
</details>
