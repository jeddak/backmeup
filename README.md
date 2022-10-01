# `backmeup`
## Description

A backup script, which is actually a convenience wrapper for the
[`dar`](https://dar.linux.free.fr) disk archiving tool, written in
`zsh`.

`dar` is an extremely powerful program, but has a lot of options and
different use cases that can overwhelm the casual user. `backmeup`
strives to simplify the backup process by making some decisions for
you and allowing you to specify just the most basic parameters. It
allows you to set up an automated backup process quickly and then move
on to other things, with the peace of mind of knowing that your data
is being backed up.

## Dependencies
Requires `zsh`, `dar`, `awk`, `cut`, `df`, `du`, `echo`, `grep`, 
`mkdir`, `realpath`, `tail`, `uname`

## Compatibility
`backmeup` was written on Debian Linux, although it should work fine
on other GNU-based distros, and could easily be adapted to MacOS X and
BSD-based Unices.

## Usage

`backmeup`

   `-f` *configuration file*
   
   `-d` *backup directory*
   
   `-t` *backup type*

### Arguments

*backup type* is `full` | `diff`
  
`full` does a complete backup of everything specified in the
configuration file

`diff` does an incremental backup of everything specified in the
configuration file based on the changes made since the last `full`
backup

### Example

   backmeup -f ~/.backmeup.cfg -d /media/tsawyer/BACKDUMP/backup/phobos -t diff

## What It Does

The script assumes you are backing up to a single volume.

It first checks to make sure the volume has enough free space (this is
based on a hardcoded minimum of 25% free space - see the variable named
`MIN_FREE_PERCENT` which is set to `.25`).

If the drive is more than 75% full, the script begins deleting prior
backups starting with the oldest, until there is enough free space.

Once it's satisified that there is enough free space, it invokes `dar`
according to the configuration (see below).

The script is hardcoded to use an archive size of 512 megabytes. This
can be changed by modifying the `MAX_SIZE` variable in the script.

## Configuration

The backup directories and the filename patterns to ignore are
specified in a separate configuration file.

Where the configuration file lives and what it's named is up to you,
but you need to tell the script where it's located using the `-f`
option.

The contents of the file are actually `dar` options  and are passed
straight through.

In short, the `-g` flag includes specific directories, and the `-P`
flag omits specific directories. (See the 
[`dar` documentation](https://dar.linux.free.fr) for more options that
may be included in this file.)

Example configuration file contents:
    
    # include
    -g usr/local/etc 
    -g usr/local/sbin 
    -g etc
    -g home/tsawyer
    
    # ignore
    -P home/tsawyer/.VirtualBox
    -P home/tsawyer/.adobe
    -P home/tsawyer/.cabal
    -P home/tsawyer/.cache
    -P home/tsawyer/.claws-mail
    -P home/tsawyer/.config
    -P home/tsawyer/.cpan
    -P home/tsawyer/.cpan
    -P home/tsawyer/.dvdcss
    -P home/tsawyer/.eclipse
    -P home/tsawyer/.emacs.d/.cache
    -P home/tsawyer/.googleearth
    -P home/tsawyer/.gradle
    -P home/tsawyer/.ivy2
    -P home/tsawyer/.java
    -P home/tsawyer/.kde
    -P home/tsawyer/.kube/cache
    -P home/tsawyer/.kube/http-cache
    -P home/tsawyer/.local 
    -P home/tsawyer/.local/share/Trash
    -P home/tsawyer/.m2
    -P home/tsawyer/.m2/repository
    -P home/tsawyer/.m2/wrapper
    -P home/tsawyer/.minikube
    -P home/tsawyer/.mozilla
    -P home/tsawyer/.mutt
    -P home/tsawyer/.npm
    -P home/tsawyer/.p2
    -P home/tsawyer/.pia_manager
    -P home/tsawyer/.ps
    -P home/tsawyer/.sbt
    -P home/tsawyer/.scalaide
    -P home/tsawyer/.secondlife/cache
    -P home/tsawyer/.ssb
    -P home/tsawyer/.ssh
    -P home/tsawyer/.stack
    -P home/tsawyer/.steam
    -P home/tsawyer/.thumbnails
    -P home/tsawyer/.var
    -P home/tsawyer/.wine
    -P home/tsawyer/.xfce4-session.verbose-log
    -P home/tsawyer/.zoom
    

Use `backmeup` with a scheduler such as `cron` to ensure that your
data is always being replicated.
