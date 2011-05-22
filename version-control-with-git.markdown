title: Version Control with Git

# Version Control with Git

# Who am I?
* Jeroen Budts
* PHP & Drupal Developer
* At Inuits - A Belgian Open Source Consultancy Company
* http://budts.be
* @teranex
* will become a father in November

![git-00](img/git-00.png)

# Overview
* Basic Git usage
* How Git stores it's stuff
* More advanced Git usage
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




# Thanks!
This presentation is licensed under the [Creative Commons Attribution-Non Commercial-Share Alike 3.0 license](http://creativecommons.org/licenses/by-nc-sa/3.0/us/)

Some of these images where taken from the [Pro Git](http://progit.org/) book
