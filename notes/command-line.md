## Command Line Basics

### Terminology
* directory (linux terminology) = folder (windows / mac terminology)
* path = directions to a specific directory
  * chain together the different directory names by separating them with a `/`
* argument = what things a command takes (some commands take one argument, some take two, some more still)

### Basic Commands

* `ls`:  list contents of director
  * default:  current directory
  * can give it a path to show contents of other directories / subdirectories

* `cd`:  change directory
  * give it path of directory you wish to change into
  * path can be absolute or relative

* `pwd`:  print working directory
  * displays full absolute path to current (e.g. "working") directory
  * helpful for figuring out where you currently are in the filesystem

* `mkdir`:  make directory
  * can give a path or simply a directory name
    (which is just the simplest relative path)

* `rm`:  remove
  * removes a file or directory
  * need `-r` option to remove a directory
    (`-r` means recurisve and says remove this and everything inside of it)
  * can give it a filename / directory name (which is just the simples relative path)
    or can give it an absolute or relative path to a different directory file
  * be careful -- there is no recycle / trash bin (e.g. when you remove it, it's gone)

* `mv`:  move / rename
  * moves a directory / file to either a different name (e.g. rename)
    or a different location
  * both arguments can be absolute or relative paths

* `cp`:  copy
  * takes 2 arguments, e.g. `cp source destination`
  * copies a file or directory
  * need `-r` to copy a directory (again, recursive)
  * both `source` and `destination` can be absolute or relative paths

### Special Locations 

* `.`:  current directory
* `~`:  *your* home directory
* `..`:  parent directory
* `/`:  root directory (the very top of the hierarchical filesystem)

### Absolute vs Relative Paths
* absolute path
  * path that begins at the root directory (i.e. `/`)
  * means / references the same location regardless of where you are in the filesystem
  * like giving directions from a single, fixed, agreed-upon location that everyone knows
  * `~` is not technically an absolute path, but instead gets expanded into an absolute path

* relative path
  * path relative to your current location
  * path means something different depending on where you are
  * like giving directions from your current location
  
### Options
Options modify the effect of a command.
Most commands have options.
Examples:
* `ls`
  `-l`:  long, provide additional information on size, timestamp, owner, etc.
  `-a`:  shows all files including hidden files (filenames which begin with a `.`)
* `cp`
  `-r`:  recursive

Some options are 1 letter (those begin with a single `-`).
Some options are a long-form (those begin with `--`).
Many options have both forms.
Single letter options can generally be combined unless they have arguments with them.

How to know options?
* most commands have a `--help` option which will provide high-level help info
  for the most commonly used options
* man pages:  `man cmdname`
  * "man" stands for "manual"
  * full manual information for each command

### Globbing / Wildcards
Sometimes you care about all files or directories that match a certain pattern.
We can use wildcards to do this.  The wildcard character is `*`, which
will simply represent anything at that level.
For instance, running
```
ls *.csv
```
will list all files with the extension `.csv` in the current directory.
Wildcards can be used at any level within a path as well,
so for instance
```
ls */*.csv
```
would instead list any files ending with the extension `.csv` in any immediate
subdirectory of the current directory.
This process of using wildcards is known as globbing.

### Helpful Notes
* Avoid spaces in file / directory names.  Since spaces are used to separate out
  the arguments and options, spaces in filenames or directory names will often be interpreted as multiple
