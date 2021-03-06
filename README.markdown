### git-number ###

git-number is a shell script that increases my command-line git productivity
(with some help from two perl scripts).

## Usage Examples ##

Here's how it increase my productivity (it might increase yours too):

    $ alias gn='git number'
    $ alias ga='git number add'

    $ gn
    # On branch master
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #1      .README.swp
    #2      README
    $

Does the output look familiar? Notice the numbers before the filenames? Those
are their ids.

Now look at this:

    $ ga 2
    git add  README  # <- It does this in the background

    $ gn
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #1      new file:   README
    #
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #2      .README.swp

When run without arguments, 'git number' runs 'git status' and attach a unique
number for each line of filename printed by 'git status', and it will 'remember'
this number-to-filename association. When run with arguments, like this:

    $ git number <any git command> [one or more numbers or git options/args]

'git number' will run that &lt;any git command&gt; and subtitute all the numbers
to their equivalent filenames. Non-numeric argument are passed intact to git.

It accepts multiple args and ranges too:

    $ ga 2-4 6 10

Which is the same as writing

    $ ga 2 3 4 6 10

## What's included ##

1. git-number: Show or operate on files by their 'ids'
2. git-list: List filenames from given ids
3. git-id: Generate and show the file ids

    for example to show the second file run:

        $ git list 2

    or to show the first three files, and the  9th and 13th:

        $ git list 1-3 9 13

## What's not included ##

Batteries.

## How it works ##

'git-id' is a perl script that does two things:

1. Runs "git status" and inserts a number before each file reported by "git
   status"
2. Show and save a copy of the output to a file (.git/gitids.txt)

(If you're pedantic then it does four things)

'git-list' is a perl script that converts numbers and ranges to their
equivalent filenames from the previous run of 'git-id'.

'git-number' uses 'git-list' to convert all its numbers and ranges arguments to
filenames and passes them down to git.

## Caveat ##

1. <strike>For a file that is marked as conflicting, the ansi closing color escape
   sequence printed by git comes after the final newline, which breaks this
   script a little</strike>. This seems to be fixed in latest git.

2. git-number depends on the output of git-status, which is a porcelain. Caveat emptor.

3. It does not work for renames:

<pre>
    $ git mv a.txt b.txt
    $ gn
    # On branch b
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #1      renamed:    a.txt -> b.txt
    #
    $ gn reset 1  # <- this will NOT do what you want it to do!
</pre>

I'm sure there are a few more. Send me a patch :)

## Installation ##

Copy (or make a symbolic link to) 'git-number', 'git-list', 'git-id' into your
$HOME/bin directory, or wherever you prefer to put them.
