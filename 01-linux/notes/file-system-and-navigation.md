# File system and navigation

How the Linux file system is laid out, and the commands to move around it and manage files.

## What it is
The file system is one structure that starts at the root, `/`. Inside it are folders like `/home`, `/etc`, `/var/log`, `/tmp` and `/bin`, which hold users' personal files, config files, log files, temporary files and so on. To navigate it you use `pwd` (shows where you are), `cd` (moves you into a folder) and `ls` (lists what's in a folder), and to manage files you use commands like `cp`, `mv` and `rm` to copy, rename and delete them.

## Why it matters
If an app breaks on a cloud server, I'd start by navigating the file system to read the logs in `/var/log` and look for the error that explains what went wrong, then fix the config in `/etc`.

## What I did
I moved around with `cd /` and `cd ~`, and created folders and files with `mkdir -p`, `touch` and `echo "text" > file`.

Copying keeps the original, moving doesn't:

    $ cp a.txt b.txt
    $ ls
    a.txt  b.txt

    $ echo "world" > c.txt
    $ mv c.txt e.txt
    $ ls
    a.txt  b.txt  e.txt  numbers.txt

Viewing the start and end of a file:

    $ head -n 3 numbers.txt
    1
    2
    3
    $ tail -n 3 numbers.txt
    18
    19
    20

Deleting a file and an empty folder:

    $ ls
    a.txt  b.txt  emptydir  junk.txt  numbers.txt
    $ rm junk.txt
    $ rmdir emptydir
    $ ls
    a.txt  b.txt  numbers.txt

Deleting a folder with a file inside:

    $ ls full
    file1.txt
    $ rm -r full
    $ ls
    a.txt  b.txt  numbers.txt

## What went wrong and how I fixed it
`cd my folder` failed with `cd: too many arguments`. The shell treated `my folder` as two separate words, so I fixed it by putting quotation marks around the folder name: `cd "my folder"`.

`rmdir full` failed with `rmdir: failed to remove 'full': Directory not empty`. It only works on empty folders, so I used `rm -r` instead, which deletes the folder and everything inside it.
