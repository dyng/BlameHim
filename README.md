# BlameHim

Still feel shame while writting pieces of shit, and is afraid of being disclosed someday by `git blame`? Then it is for you.

## Usage

*blamehim* is a simple shell script that can change the author of lines to someone you specified. Thanks to git's loose permission management, you can disguise as anybody if you know his username and email address, and making it even easier, *blamehim* will do it for you. When someone blame this file, they will see the victim's name rather than yours.

## How it works

There is two mode in blamehim: *hard-mode* and *soft-mode*.

### soft mode
It is simple. *blamehim* apends a space to lines you specified, temporarily changes `user.name` and `use.email` to your victim's, makes commit, restores your own username and email address, done.
##### how to undo
In soft-mode, *blamehim* just append an extra fake commit and won't hurt existing history, so it is safe. You can simply undo it by `git reset --hard HEAD~`.

### hard mode
Unlike soft-mode, hard-mode use `git filter-branch` to rewrite almost entire history if necessary. *blamehim* run in soft-mode by default, use `-f` to enable hard-mode, like

    blamehim -f -a badguy 1-$ file

Remember that hard-mode may cost a pretty long time to complete.

##### how to undo
You can find your original branch header under `.git/refs/original`, or refer to `git reflog`. If you found it, `git reset` will do the job.

## Example

First I have a file and blamed like this:

    f8d5ce00 (dyng 2013-08-01 23:55:22 +0900 1) foo
    f8d5ce00 (dyng 2013-08-01 23:55:22 +0900 2) bar
    f8d5ce00 (dyng 2013-08-01 23:55:22 +0900 3) foo

Then I run script like this:

    blamehim -a badguy -e badguy@badguys.com 1-$ file

Here is the blame result after things done:

    d009b43c (badguy 2013-08-02 00:02:41 +0900 1) foo
    d009b43c (badguy 2013-08-02 00:02:41 +0900 2) bar
    d009b43c (badguy 2013-08-02 00:02:41 +0900 3) foo
