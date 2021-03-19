---
layout: default
nav_order: 3
title: Examples & Running a Shell
permalink: /example-and-running/
---

# An example and getting started with the command line

Before we get ahead of ourselves, I want to convince you that the command line is worth learning. Then we will see exactly where it lives on our computers and how to prepare for the exercises that come later.

## A few (hopefully compelling) examples

_Why_ would you use a textual interface to a computer? Isn't it much less intuitive than the graphical user interface, a massive innovation that is essential to the ubiquity of computers? I've culled a few examples from my work of operations that would be tedious, if not impossible, without the command line.

Before we look at too many command examples, let's talk about how we represent them. The dollar sign `$` symbol indicates _the beginning of the command prompt_. When practicing a command, do not type that part. Secondly, we will often have placeholder values in our examples. These will be represented like `$PLACEHOLDER` which is what a variable looks like in Bash.
{: .note}

### Reformat CSV & open Google Spreadsheet to append contents

I receive near-daily CSVs from our student information system about course
changes and we haven't found a better way to automatically process them yet. So
I download these CSVs manually but can then perform several operation in one
command I've written into a short script:

```sh
$ csvformat -T $CSV_FILE | sed -e '1,2d' | \
pbcopy && open $SPREADSHEET_URL && trash $CSV_FILE
```

I've combined this chain of commands into a script so I don't have to remember them all:

```sh
$ colocopy $CSV_FILE
```

This does several things:

1. Reformat the CSV to be tab-separated values (for easier pasting into Google Sheets)
1. Delete the header row and a line of useless preamble from the CSV (common problem with COUNTER reports, for example) with `sed`
1. Add this data to the system clipboard (`pbcopy` is a Mac OS X utility)
1. `open` the Google Spreadsheet in my default web browser
1. `trash` the downloaded file (`trash` is a node.js utility)

### Backup large Archive-IT WARC files

My organization wants to create a backup copy of Archive-It's WARC files for a website. These files are numerous and up to a gigabyte in size; manually downloading and transferring them all is tedious. [Their documentation](https://support.archive-it.org/hc/en-us/articles/360015225051-Find-and-download-your-WARC-files-with-WASAPI) shows how we can use the command line to retrieve a list of files for bulk download. The script below roughly outlines how we might create a list of files, download them one-by-one, remove a long temporary query string from the files' names, transfer them to a remote server, and finally remove them so they don't overwhelm our hard drive:

```sh
$ USER=username; PASS=password; COLLECTION=123456; REMOTE_SERVER=remote.storage.edu
$ curl -u $USER:$PASS "https://warcs.archive-it.org/wasapi/v1/webdata?collection=$COLLECTION" > data.json
$ jq -r .files[].locations[0] data.json > urls.txt
$ for URL in $(cat urls.txt); do \
wget --http-user=$USER --http-password=$PASS --accept txt,gz $URL; \
rename -v 's/\?.*tmp//' *.tmp; \
scp *.gz $REMOTE_SERVER; \
rm -v *.gz; \
done
```

If we can `ssh` into our remote server, it would be even more effective to simply run these commands on that server and save ourselves the `scp` transfer step. `rsync` is also a better, but more complicated, tool for file transfer which is better about picking up where we left off if progress is interrupted.

## More reasons to ❤️

**"Text is the universal interface"**: this point may seem abstract until we see more examples, but on the command line data and programs work in harmony. The command line is understandable, composable, and portable. Because everything is text it is easy to inspect processes, feed data streams through a series of transformations and to add, modify, or remove steps from the series.

[Basics of the Unix Philosophy](https://homepage.cs.uri.edu/~thenry/resources/unix_art/ch01s06.html) elaborates this point well, including this synopsis: "Write programs that do one thing and do it well. Write programs to work together. Write programs to handle text streams, because that is a universal interface."

**Software availability**: it seems incredible to say it in the year 2021 but some software still does not have a GUI and is only available on the command line. Web development build tools like `gulp` come to mind as not having a GUI equivalent. There is actually an enormous quantity of software written explicitly for the command line that we do not have access to otherwise.

Furthermore, some software like the popular `git` version control program _does_ have GUI applications but they are somewhat limited; more can be accomplished via the `git` CLI and often more quickly, too.

**Scripting**: by their nature, all command line operations can be bunched together into a script and repeated whenever needed. These scripts can be context-sensitive by accessing environmental variables or arguments that are passed to them. GUIs can use macros or tools like Mac's Automator to approximate this but not with the same depth or elegance of the command line.

## Running the command line

Let's all start a terminal and get ready for the information and exercises that follow.

**On a Mac**: run Terminal.app which lives under Applications > Utilities. You can easily find Terminal by searching for it with Spotlight (click magnifying icon in your menu bar or use the keyboard shortcut <kbd>⌘ + Spacebar</kbd>).

Mac OS X recently [changed it's default shell](https://support.apple.com/en-us/HT208050) from `bash` to `zsh`. While that's a fine choice, to simplify today's workshop I recommend we all use bash. If you are on Catalina or later, or for any reason see "zsh" as the response to the `echo $0` command below, switch to bash simply by running it as a command: type `bash` followed <kbd>Return</kbd>.
{: .warn }

**On Linux**: most distributions come with a builtin app, usually named "Terminal" or simmilar. On Ubuntu you can use Dash to search for "terminal" or the keyboard shortcut <kbd>Ctrl + Alt + T</kbd>. Ubuntu has comprehensive documentation on [how to start a terminal for various distributions](https://help.ubuntu.com/community/UsingTheTerminal#Starting_a_terminal).

**On Windows**: run the [Git Bash](https://gitforwindows.org) program we downloaded earlier.

Are we ready? Let's run this command.

Input
{: .label .label-green }
```sh
$ echo $0
```

When you have typed out "<kbd>e-c-h-o-space-dollar-zero</kbd>" press <kbd>Enter</kbd> or <kbd>Return</kbd> to execute the command. You should see "bash" as a response, the name of the shell we're using.

Output
{: .label .label-yellow }
```sh
bash
```

If you _don't_ see the text "bash", please let me know so we can figure out what went wrong. You may need to retype the command or your system may not be configured as expected.

We just ran our first command! Hurrah! In the sections that follow, I will be talking about the syntax of the command line, some fundamental commands, and various options. You can feel free to follow along on your own. If you run into a problem or are not seeing the same thing as me, feel free to stop me to ask a question.
