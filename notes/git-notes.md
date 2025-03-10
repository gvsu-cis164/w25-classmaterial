# Git Notes 

## What is git?
* a version control system
* allows tracking of changes over time
* allows going back to previous versions
* when used with a remote, allows collaboration with others while coding

## Preliminary - basic vim introduction
What is `vim`?  It is a command line editor which
allows you to directly edit files in the terminal without opening up
a graphical window.  There's a lot of reasons terminal based editors
are helpful.  They also have their disadvantages, namely
that you don't get the full ability to use the mouse and mostly
have to navigate with the arrow keys on the keyboard.

You can do a lot with `vim`, but for our purposes,
all we care about is the bare minimum working knowledge.

You can manually open a file in `vim` with:
```
vim filename
```

Once in vim, to edit a file you need to:
1. press "i" to enter "Insert" mode
2. type whatever edits you want to the file
3. press "esc"
4. type `:wq`
5. press "enter"

## Basic Git Commands

* `git init`:  turns the current directory into a git repo.
  * Only needs to be run once for a repository.
  * Typically you want to make and change into a new directory before running this.

* `git status`:  shows the status of the current git repository that you are in.
  status of different files can include:
  * untracked:  git doesn't yet know about them / isn't paying attention to them
  * modified:  the file has changed since the last recorded version
  * staged:  the file has changed and changes have been staged (think like lined up
    to take a picture of), but the staged changes haven't officially been recorded yet

* `git add filepath`:  stages the file specified.
  The `filepath` can be a specific filename in the current directory
  or a path to a file in a subdirectory within your git repository.
  It can't be a path to something outside of your git repository.

* `git commit`:  commits the staged changes.
  This can be run in two different ways:
  * `git commit -m "some helpful commit message" -- this specifies the message
    at the same time as when you commit
  * `git commit` -- this will open up a vim (mac) or nano (WSL)
    window where you type in the commit message.

* `git log`:  see a list of git commits
  * ordered with most recent commits listed first
  * shows commit message, author, date/time
  * each commit has a unique SHA256 hash (the long, seemingly random string of characters at the top),
    which is the unique identifier for that commit.  Anywhere you use it, you only need the first 5-6 characters,
    which is generally enough to uniquely identify the commit.
  * other options can show more info such as files changed, patches from the files that were changed, etc.

* `git restore`:  used to update version of file in working directory (aka, just the version in your filesystem)
  to a previous version.
  This is most often run in two different ways:
  * `git restore filename` which gets rid of the changes made to the file since the most recent commit
  * `git restore --source=SHA filename` which grabs the version of the file from the commit with the specified SHA256 hash.

* `git diff`:  shows the changes made to a file
  This can be run in a few different ways:
  * `git diff filename`:  see the difference between the version of the file in the working directory
    and the staged version of the file
  * `git diff --staged filename`:  see the difference between the staged version of the file and the last commit
  * `git diff SHA filename`:  see the difference between the version of the file in your working
    directory and the version in the specified commit.
  * `git diff SHA1 SHA2 filename`:  see the differences between the version of specified file between
    the two specified commits

### General Workflow
The general workflow with git, when using it locally is that as
you work you repeat the following steps:
* make changes to your file(s)
* add the relevant file(s) (i.e. staging them)
* commit the staged changes

## Working with Remotes
Remotes with git are helpful as they provide a backup.
Using git locally provides version control and allows
going back to previous versions, but if you were for instance
to lose your laptop, you still lose everything.

Remotes mean that there are additional copies of the repository
stored on other machiens, most often in the cloud.
This is great for backup purposes and also makes it easier
for multiple people to work together, as multiple collaborators
can have access to the same remote.
In these cases, each user works with git locally and
can grab changes from the remote and send their changes to the remote.

### Preliminary - Setting Up SSH Keys on Github
Most frequently git is used in conjunction with a remote,
with the most common remote being [Github](https://github.com/)
or within industry sometimes gitlab.  

To connect with remotes, we need to have some way for the remote
to know who you are and that you have access to that repository.
To do this, we'll set up what are known as ssh keys, a way to securely
authenticate who you are without a password.

Then, walk through the following steps:

1. Go to Github and make a username if you do not already have one.
The username can be whatever you choose, but it's generally best if it's
something semi-professional (aka, nothing so unprofessional that you wouldn't
be willing to put it on a resume).
2. Once you have your username, open up a terminal on your laptop and run:
   ```
   ssh-keygen
   ```
   It will keep asking you questions, just keep hitting "enter" without
   typing anything else until you see some ASCII art.
3. Then, run:
   ```
   cat ~/.ssh/id_rsa.pub
   ```
   If it says no such file or directory, try the following instead:
   ```
   cat ~/.ssh/id_ed25519.pub
   ```
4. One of the above should give you a mostly random string of characters.
  Copy the output.
5. Go to github.com, and do the following:
   * click the circle in the upper right with colored pixels
   * select "Settings"
   * on the left, select "SSH and GPG keys"
   * click the green button on the right saying "New SSH key"
   * add a title -- this should be something that will help you remember which computer this is
   * paste the key into the box
   * press "Add SSH key" to save the key


### Git Remote Commands

When using remotes, we add a few different commands:

* `git pull`: goes and grabs changes from a remote repository and integrates them into
  the current branch
  * note, this is actually a combination of 2 commands underneat:  `git fetch` followed by `git merge`

* `git push`: sends the commits you've made locally to the remote repository
  * note, if there are any changes on the remote that you don't yet have,
    you will have to grab those canges first and integrate them (with `git pull`)

* `git remote -v`: see the remotes associated with the current repository

* `git remote add origin URL`: add URL (which should be replaced with
  the appropriate URL for your remote repository starting with `git@github.com`)
  as the remote for this repository with the name origin (convention/standard name)

* `git clone URL`:  clones remote repository with specified URL
  (should begin with `git@github.com`)

To get a remote, you have one of two options:
1. Have an existing local repository with no remote?
   * Create a totally empty remote repository on github
     * Go to github.com
     * Click the green "New" button
     * Give your repository a name
     * Select public or private depending on if you want others to be able to see it.
     * Press "Create repository"
   * Connect the remote to your existing repository
     * Click the green "Code" button
     * Select ssh
     * Copy the url and run `git remote add origin URL` from your terminal in your existing local repository
   * Run `git push` to update the remote with your local repository commits

2. Don't yet have a local repository and want to make remote from the start?
   * Create a new remote repository on github by:
     * Go to github.com
     * Click the green "New" button
     * Give your repository a name
     * Select public or private depending on if you want others to be able to see it
     * Select the checkbox for "Add a README file" (this makes it easier as the repo won't be empty)
     * Press "Create repository"
   * Clone the remote repository by:
     * Click the green "Code" button
     * Select ssh
     * Copy the url and run `git clone URL` from your terminal

Either way, once the remote is connected, the general workflow becomes:
* pull new changes from the remote
* make changes to your file(s)
* add the relevant file(s) (i.e. staging them)
* commit the staged changes
* push committed changes to the remote
