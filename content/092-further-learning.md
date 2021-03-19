---
layout: default
nav_order: 9
title: Further Learning
permalink: /further-learning/
---

# Further Learning, Tools, & Projects

These are some resources for learning more about the command line as well as some ideas of CLI software to use and projects that suit themselves to the CLI.

## Resources to review & learn more

A fair amount of these materials will repeat what we've covered in this workshop, but they can still help solidify your understanding.

- [The UNIX Shell](https://librarycarpentry.org/lc-shell/) by Library Carpentry
- [Learn to Love the Command Line](https://acrl.ala.org/techconnect/post/learn-to-love-the-command-line/) on ACRL Tech Connect (yes, I wrote a blog post with this title seven years ago, then forgot about it and accidentally named this workshop the same thing)
- [Bash Scripting: automating repetitive command line tasks](https://acrl.ala.org/techconnect/post/bash-scripting-automating-repetitive-command-line-tasks/) by Sibyl Schaefer on ACRL Tech Connect
- [Intro to CLIs](https://docs.google.com/presentation/d/14oisMTEG-O-_DnHSfBRCLsuRq041VnJt9qFOAiVhdpI/edit#slide=id.p) by Coral Sheldon-Hess. The [ccac-data-analytics/learn-cli](https://github.com/ccac-data-analytics/learn-cli) repo has even more material.
- [Scripting (Bash)](https://training.ashleyblewer.com/presentations/bash-scripting.html#2) by Ashley Blewer
- [Command Line Interface](https://training.ashleyblewer.com/presentations/cli.html#2) by Ashley Blewer
- [Using The Terminal](https://help.ubuntu.com/community/UsingTheTerminal) from the Ubuntu documentation
- [Bash Guide for Beginners](https://tldp.org/LDP/Bash-Beginners-Guide/html/index.html) covers virtually all Bash syntax but, most usefully, gives examples for everything and not only definitions
- [Stack Overflow](https://stackoverflow.com/questions/tagged/bash) and the [Unix & Linux Stack Exchange](https://unix.stackexchange.com/) are great places for questions, as they are for many computing topics

We covered the surface of many topics but here are a few I didn't have enough time for:

- array variables
- [string manipulation](https://tldp.org/LDP/abs/html/string-manipulation.html) using variables like finding string length with `${#NAME}`, substrings like `${NAME:4}` and `${NAME:4:2}` (starts at index of first number and runs the length of second number), removing characters `${NAME:test}` to remove "test" from the front of "testname" for instance while `${NAME%name}` removes "name" from the back of "testname" (this can be useful to strip file extensions e.g. `${FILENAME%.jpg}`)
- parsing command-line arguments with `getopts`
- the special Internal Field Separator `$IFS` variable which changes how Bash considers the separators between words in a sentence e.g. `LIST=one:two:three; IFS=':'; for N in $LIST; do echo $N; done` prints "one", "two", "three" on separate lines because a colon `:` is used as a separate instead of a space

## GLAM Software

These are tools that may be particularly relevant for people in the <abbr title="Galleries, Libraries, Archives, and Museums">GLAM</abbr> space, though many of them are generically relevant tools that simply improve the quality of your command line life.

The number one most frustrating thing about the command line, for me, has always been installing and configuring software. Getting things running, particularly in a Windows environment but often on Mac too, can be a struggle. My advice is to 1) search the web for solutions because you are not the first with your problem, 2) ask for help on [the Code4Lib Slack](https://code4lib.org/slack), 3) check that executables are on your `$PATH`, and 4) do not let  frustration dissuade you from trying.
{: .warn}

### Improved Fundamentals

- `ack` - great regular expression search utility
- `curl` - make HTTP requests, do stuff with URLs
- `exa` - nicer version of `ls`
- `fish` - nice shell that's more user friendly than bash
- `git` - incredibly popular version control software
- `homebrew` - makes installing & updating CLI software easy on a Mac
- `rsync` - robust file synchronization locally or between machines
- `tldr` - nicer help documentation for common tools
- `todo-txt` - todo list app for the CLI
- `vim` - popular text editor
- `wget` - download web content
- `z` - quicker navigation, remembers commonly visited places
- `zsh` - another nice shell, bash alternative that is more popular than fish

### Metadata

- `catmandu` - bulk metadata/cataloging operations
- `csvkit` - Python tools for CSV processing
- `exiftool` - look at image file metadata (EXIF tags)
- `jq` - JSON data processing
- `marcgrep.pl` - search over MARC files for fields matching a pattern
- `miller` - more CSV & JSON processing tools
- `xmlstarlet` - XML metadata manipulation
- `xsltproc` - apply XSLT transformations to XML
- `yaz` - "toolkit for Z39.50/SRW/SRU clients/servers"

### Media

- `ffmpeg` - play, record, convert, and stream audiovisual media
- `ghostscript` - `gs` utility for creating & interpreting PDFs
- `imagemagick` - image conversion & editing library
- `pandoc` - "Swiss-army knife of markup format conversion"
- `youtube-dl` - download YouTube videos

### Coding, Web Development

Every programming language has a suite of command line tools to help you code, performing tasks like compiling or optimizing code, linting it for problems, quickly scaffolding out project templates. Languages also usually come with a builtin <abbr title="Read-Eval-Print Loop">REPL</abbr> which is a sort of CLI for the language itself, letting you write code one line at a time to learn or test.

When I use Python on a project, I always use `pipenv` to manage my python version and dependencies. The popular Django website framework has its own command-line interface.

Similarly, Ruby projects can use `bundler` and `rbenv` to manage dependencies and ruby versions respectively.

When I'm writing JavaScript, or honestly even web projects that do not really involve JS, I always end up using `node`, `npm`, and a suite of related tools. `npm` not only manages dependencies but also lets you define custom scripts in its package.json configuration file. `gulp` is my processing/build tool of choice but `grunt` used to be (and yes I find it funny that `node` developers are writing generations of tools named after unflattering bodily sounds).

[Static-site generators](https://www.netlify.com/blog/2020/04/14/what-is-a-static-site-generator-and-3-ways-to-find-the-best-one/) (Jekyll, Hugo, Gatsby) all typically use the CLI to run tasks like building the site or running a local testing server.

## More Example Projects

I generated several examples to show the power of the command line but couldn't include them all at the beginning of the workshop.

### Download several files at once

I needed a few logs from our Moodle LMS. These are trivial to download all at once if I know how the filenames are structured and use the `seq` (sequence) command which returns a range of integers.

```sh
scp moo:/var/log/moodle/12-2020120$(seq 5 7)-020000.csv .
```

Also handy: `seq -w` pads with zeroes so all numbers are same length, see this example: `seq -w 7 11`

You also `wget` a series of files with predictable, numeric IDs. For instance, to download a hundred images with formulaic filenames 0001.jpeg to 0100.jpeg from a website:

```sh
wget https://example.com/0$(seq -w 1 100).jpeg
```

### Working with Website Images

Each year, we change the main home page image of our website. I provide sample screenshots of what the website will look like with several different image options. To do this, I need to both take a bunch of screenshots and refresh a preview of the website, which sometimes doesn't work because of caching. To automatically move the screenshots I'm taking to a "HomePageImages" directory while repeatedly clearing the cache, I run two never-ending loops:

```sh
$ # on my laptop in my Downloads folder
$ while true; do mv "Screen Shot *" ~/Pictures/HomePageImages/; sleep 10; done
$ # on the web server running our Django site
$ while true; python manage.py clear_cache; sleep 10; done
```

This `while true` phrase is a bit of a hack; `while` loops run until their condition becomes false, but the `true` shell builtin is always going to be, well, true. It's a simple way to generate an infinite loop. I can manually cancel the loops with the Ctrl+C keyboard interrupt when I'm done.

### `csvkit` is an actual miracle

When I went to email the attendees of this workshop, I immediately encountered an annoyance: the registration list was a plain HTML table with no easy means of copying just the email address column. Fittingly, the command-line offers a convenient solution in the form of Python's brilliant `csvkit`, which is a suite of utilities. You have to first `pip install csvkit` first for these commands to work. I copy-pasted the HTML table, which inserts tabs between columns, then ran this single command:

```sh
$ csvcut -t -c 3 attendees.tsv | pbcopy
```

`csvcut` "cuts" the column specified by the `-c` parameter, which can be numeric (indexed from 1) or the name used in a header row. The `-t` flag specifies that the input file uses tabs as separators. `pbcopy` is a Mac utility that adds text to the OS clipboard.

We already saw, in one of the opening examples, an example of `csvformat` converting from comma to tab separation. But csvkit is a Swiss army knife of abilities. I use it extensively in my "[libraries_course_list](https://github.com/cca/libraries_course_lists)" project which processes course information into taxonomies (essentially hierarchical, controlled vocabularies) for our institutional repository. Here is an example passage:

```sh
#!/usr/bin/env fish
# usage: ./upload-taxos-to-vault.fish data/report.csv
# log to file named with today's date in ISO 8601
set logfile logs/(date '+%Y-%m-%d').txt
# parse institutional repository credentials from file using jq
set pw (jq -r '.password' .credentials)
set un (jq -r '.username' .credentials)

# parse all department codes after trimming header row
set departments (csvcut -c 2 $argv[1] | tail -n +2 | sort | uniq )
for department in $departments
    # full course list in EQUELLA taxonomy format
    set taxonomy_name "$department - COURSE LIST"
    set uuid (eq tax --name $taxonomy_name | jq -r '.uuid')
    if [ $uuid ]
        echo "Updating $taxonomy_name taxonomy" >> $logfile
        uptaxo --tid $uuid --pw $pw --un $un \
            --csv data/$department-course-list-taxo.csv >> $logfile
    end
end
```

The project uses the `fish` shell I've mentioned before and we can see some of its appeal in the simpler syntax of loops (no need for "do", all loop and if-conditions are closed by the same "end" statement whereas every bash construction has a different closing statement) and subshells (no `$` which conflates them with variable references).

For use in this project and elsewhere, I wrote my own CLI to our openEQUELLA repository's REST API `[eq](https://github.com/cca/equella_cli)` which has proven invaluable. `uptaxo` is a CLI to another openEQUELLA script which **up**loads **taxo**nomies. While there are certainly better approaches than using a shell script to duct-tape together homemade tools, this workflow has saved me hours of my life and proven relatively bulletproof.

### Upload and restore multiple Moodle course backups

Moodle has its own compressed backup format ".mbz". These files contain _all_ the media in a course and as such can grow quite large, surpassing the file size limits of the web interface not to mention taking a long time to process. Using `moosh` ("Moodle shell") and common file transfer software, it's easy to restore many large courses at once.

```sh
$ # transfer to server
$ rsync -avzhP *.mbz moodle.school.edu:/path/to/moodle
$ # when transfers complete, log into server & run moosh
$ ssh moodle.school.edu
$ cd /path/to/moodle
$ # restore courses into course category id=1, run as the web server user or use
$ # the moosh "-n" flag if you're running as root
$ sudo -u www-data moosh course-restore *.mbz 1
$ # delete backup files
$ rm *.mbz
```

### Run the openEQUELLA launcher with your password on your clipboard

The openEQUELLA repository software comes with a launcher script which is somewhat awkward to use, it opens a useless terminal window and then a login prompt that doesn't remember any credentials. I wrote a bash script to wrap running the launcher but also copy my password onto my clipboard:

```sh
#!/usr/bin/env bash
jq -r '.password' ~/.equellarc | tr -d '\n' | pbcopy
# Path to OpenEQUELLA Admin Console Launcher script, download from
# https://github.com/openequella/openEQUELLA-admin-console-package/releases
/Applications/admin-console/Mac-launcher.sh 2&>/dev/null &
```

The `2&>/dev/null` phrase silences all output of the command while the final `&` runs it in the background. I put this script on my path so I can simply run it in my open terminal window but then continue working.

I have an ".equellarc" file with credentials in my user's home folder because I use my own CLI tool `[equella-cli](https://www.npmjs.com/package/equella-cli)` to perform many operations on our IR.
