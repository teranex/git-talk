title: Version Control with Git

# Version Control with Git

# Who am I?
* Jeroen Budts
* PHP & Drupal Developer
* At [Inuits](http://inuits.eu) - An Open Source Consultancy Company
* http://budts.be
* @teranex
* will become a father in November

![git-00](img/git-00_2.png)

# Overview
* Basic Git usage
* How Git stores it's stuff
* More advanced Git usage
  (branching, merging, rebasing, remotes, ...)
* Some cool Git tools
* Git and Drupal

# What is Git?
> "Git is a free & open source, distributed version control system designed to 
> handle everything from small to very large projects with speed and efficiency."  

* Distributed
* Open Source
* Fast
* Created by Linus Torvalds (Linux)

# Basic Git Usage
* Initialize a repo: `git init`
* Add files to the index: `git add -A` or `git add .`
* Commit files: `git commit`

![git-01](img/git-01.png)

# The Index (Staging Area)
The index contains the changes to will be added to your next commit. Your commit will /not/ contain the changes in your working directory. Only the changes that were added to the index!

![git-02](img/git-02.png)

# The Index (Staging Area)
If you add a file, or a part of a file, to the index, a copy is made of that file. When you commit, it is that copy which ends up in the commit.

**Important**: if you make changes to the file after adding it to the index, those changes will 
not end up in your commit, unless you add the file again to the index!

    `git diff`  
    # shows the diff of your working copy

    `git diff --cached`
    # show the diff of your index

Let's make some more changes and add some of these changes to the index.

# Undoing changes
## Checkout
Checkout a single file. Notice the dashes: `git checkout` is also used in other cases, this makes it clear to Git that you are pointing to a single file.
    git checkout -- /path/to/file    # restore version from index
    git checkout HEAD /path/to/file  # restore to latest committed version


## Reset
Reset all the changes in your working copy
    git reset [--hard]

## Revert
Write a reversed version of an existing commit. This is very useful if you have already pushed the commit
    git revert [commit-sha]

## Rebase
We'll discuss this later

# How Git stores your data
Knowing how Git stores all your data will give you a better understanding of the fundamentals and Git will become a lot more predictable

The most important point to remember:  
   
** Basically a Git repository is a database of objects **

# How Git stores your data
The most important types of objects:

* blobs
* trees
* commits

Let's study those in a bit more detail

# Blobs

Git stores the _contents_ of a file in a 'blob'.

* does not contain any meta data
* a hash is calculated as the blob name
* the hash will always be the same for the same contents

some examples:

    $ git hash-object hello.txt
    ce6c1fd146f65c899e6b10e46c89097c644e3229
    
    $ git hash-object say-hi.rb
    a8784b043f12b4b0c9114c55ebf33f5c9b44ce8f

# Trees
A tree is a like a directory

* Can contain references to 0, 1 or more blobs
* Can contain references to 0, 1 or more other trees
* Contains the meta data about the files
* Itself also identified by a hash

![git-03](img/git-03_2.png)

# Commits
If you think about Git, think about commits!

* Contains a reference to one tree object
* Contains references to one or more other commits
   * one exception: the very first commit in the repo
* A commit usually points to it's parent commit
* In case of a merge, it can point to two or more commits (one commit for each branch which you merged together)

# Commits

![git-04](img/git-04.png)

Did I mention that you should think in terms of commits, when working with Git?  

If you understand commits, you basically understand Git.

# Git References or "refs"

Because a commit hash is very difficult to remember and not really useful to work with, Git uses references to point to specific commits.

## HEAD
One such reference is **HEAD**

* It points to the commit which is currently checked out into your working directory

## master
Another important reference is **master**

* master is the 'default' branch in Git
* when working on the master branch, the master reference and the HEAD reference point to the same commit

And that brings us to...

# Branches

Branches in Git are basically just references to other commits

* very easy to create
* once you get the hang of it, you will probably create branches for almost every feature you want to implement
* a branch has a name
* this name is also the reference to the most recent commit for that branch
* a commit can be shared by branches
* The point where a branch is split of from another branch, can be found by following the parents of all the commits until you find a common commit

# Branches
Creating a branch and switching to it is easy:

    git branch mywork
    
    git checkout mywork

The two previous commands can be combined:

    git checkout -b mywork

This will create a branch and immediately check it out.

Once you have merged your work on a specific feature back into your master branch you can delete your feature branch:

    git branch -d mywork


# Branches

* We are working on a branch named 'origin'
* At commit C2 we decide to split off a new branch named 'mywork'.
* Both branches originate from commit C2; commit C3 and C5 have the same parent

![git-05](img/git-05.png)

# Merging

When you have been working on a (feature) branch for a while you will probably want to merge it back into your master or develop branch (whatever name you use for it)

    # checkout the target branch
    git checkout master
    # merge our mywork branch into master
    git merge mywork

Now your repository looks like this: (Notice that commit C7 has two parents)
![git-06](img/git-06.png)

# Merging
fast forward
merge commits

# Rebasing

# Working with remotes

# History


# Suggested reading and resources

# Thanks!
This presentation is licensed under the [Creative Commons Attribution-Non Commercial-Share Alike 3.0 license](http://creativecommons.org/licenses/by-nc-sa/3.0/us/)

Sources of images and inspiration:

* Pro Git: [http://progit.org/](http://progit.org/)
* The Git Community Book: [http://book.git-scm.com/](http://book.git-scm.com/)
