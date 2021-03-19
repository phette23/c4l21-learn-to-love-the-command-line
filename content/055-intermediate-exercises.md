---
layout: default
nav_order: 8
title: Intermediate exercises
permalink: /intermediate/exercises
parent: Intermediate CLI topics
---

# Intermediate command line Exercises

This expects you to have the [names.txt](https://phette23.github.io/c4l21-learn-to-love-the-command-line/names.txt) file we used earlier.

## N. Writing a script

@TODO

...write a script to do something

## N. Using `seq` for iteration

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

The actual, easiest way to do this is to use the `tac` command which is, in function and name, "reverse cat". It prints a file to stdout starting at the last line and going to the first.
</details>

Aside: `seq -w` will ensure all integer strings are the same length by padding them with zeroes, e.g. `seq -w 1 100` goes from 001 to 100, and can be incredibly useful in dealing with filename conventions.
