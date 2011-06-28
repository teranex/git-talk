title: Version Control with Git

%# Version Control with Git
!SLIDE
![main](img/main-slide.png)

# Who am I?
* Jeroen Budts
* PHP & Drupal Developer
* At [Inuits](http://inuits.eu) - An Open Source Consultancy Company
* [http://budts.be](http://budts.be)
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
First initialize the git repository:
    git init

Then add files
    git add -A
    # or
    git add .
    # or
    git add myfile.txt
    git add myotherfile.rb

And commit the files
    git commit

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

If you modify files, you need to '_stage_' them again, by running `git add`.  

If you only modified files (not added new ones) you can skip staging with:
    git commit -a

You can also add only parts of a modified file:
    git add -p myfile.txt

# The Index (Staging Area)
    git status # show the status of your repo

![git-18](img/git-18.png)

# Viewing the history
    git log

![git-13](img/git-13.png)

# Viewing the history
`git log` has many options. The following:

    git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset \
    %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative

Will give you this, which gives **a lot** more information

![git-11](img/git-11.png)

Tip: add this in your global gitconfig as an alias: ~/.gitconfig

    [alias]
        l = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s 
        %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative    

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
* a blob never changes
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

When you have been working on a (feature) branch for a while you will probably want to merge those branches back together.

    # to merge the origin branch back into your mywork branch (to bring it up to date)
    # checkout the target branch
    git checkout mywork
    # merge the branch into the current branch
    git merge origin

Now your repository looks like this: (Notice that commit C7 has two parents)
![git-06](img/git-06.png)

# Merging

When merging two or more branches there are two possibilities:

* Merge commit
* Fast Forward  


## Merge commit
* When both branches have new commits a merge commit is created.
* Git automatically proposes a commit message:  

    `Merge branch 'mywork' into master`
* The commit has two or more parents

# Merging
##Fast Forward
* When the target branch does not have new commits
* No merge commit is created
* In fact nothing much happens
* Except: The reference for the target branch is simply changed to point the same commit as the source branch
* This is an ideal situation and can never go wrong
* Obviously you are not always this lucky (→ rebasing)

# Merging 
## Fast Forward
![git-07-before](img/git-07-before.png) Merge the 'mywork' branch into origin
![git-07-after](img/git-07-after.png) The origin branch is simply fast-forwarded

# Rebasing
Instead of merging (with merge commits) you can also rebase (so you can then fast forward)

Some people will tell you that this is very harmful, it can break your repository and destroy the universe.  
This is **NOT TRUE**. (At least if you know what you are doing)

## So what is 'rebasing'?
By rebasing your commits you can actually rewrite your history:

* Edit a commit message
* Add missing files to a commit
* Reorder your commits
* Modify the parent of a commit
* Merge a few commits together into a single commit
* Delete commits from the history
* ...
* And break your repository if you want :)

# Rebasing
## How not to break your repo?

Do not rebase commits which have already been pushed to other people  

Do not rebase commits which have already been pushed to other people  

Do not rebase commits which have already been pushed to other people  

(_I'l explain how to push later_)

Each commit which is rebased will get a new, different, hash. People (and Git) which pull this new hash will get confused.

If you do rebase a commit which was already pushed, Git will refuse the new commit, unless you use the `--force` option.


# Rebasing
## Amending changes

* The easiest and 'safest' kind of rebase 
* Only possible for the most recent commit
* Let's you add missing files and modify the commit message  

After modifying your index again:

    git commit --amend

# Rebasing
## ... on another branch
This is an alternative approach to merging, with a merge commit.

Let's reuse the example:

![git-05](img/git-05.png)

Now you  want to merge 'mywork' into 'origin' without creating a merge commit

# Rebasing
## ... on another branch
What we did before:

    git checkout origin
    git merge mywork

What we will do know

    # on the mywork branch
    git rebase origin
    # fix any merge conflicts
    git checkout origin
    git merge mywork

# Rebasing
## ... on another branch

    # on the mywork branch
    git rebase origin

![git-08](img/git-08.png)

# Rebasing
## ... on another branch

![git-09](img/git-09.png)

# Rebasing
## ... on another branch

![git-10](img/git-10.png)

# Rebasing
## Interactively

With interactive rebasing you can really rewrite history the way you want it to be. ...And break your repository.

Rebase the commits since <commit-hash>

    git rebase --interactive <commit-hash>

Suppose we have the following commits

![git-11](img/git-11.png)

# Rebasing
## Interactively

To rebase the most recent 3 commits:

    git rebase --interactive 4efd195

![git-12](img/git-12.png)

# Working with remotes

To share your work with other people you have a few options. One of these is to work with remotes

Easiest method to set this up is by cloning an existing repository instead of initializing your repo.

    git clone http://git.drupal.org/project/drupal.git

![git-14](img/git-14.png)

This will set up everything for you

# Working with remotes
## Manually adding remotes
Sometimes you will want to add a remote

* Because you had already created the repository
* Or maybe because you want to add one or more additional remotes

This is done with the `git remote` command

    git remote add github git@github.com:teranex/git-talk.git

This will add my Github repository for this presentation as a remote with the name _github_

# Working with remotes
## Pulling and pushing

When you have cloned the repository:

    git pull

This will pull in the changes from the current branch on the origin

    git push

Will push your changes to the origin

**However**: This will only work for '_tracking_' branches.

# Working with remotes
## Remote Branches

It is important to think about remote branches as just _branches_

By default, Git does not know, nor care, about relationships between branches!

* local branch: **master**
* remote ('github') branch: **github/master**
* **Git just sees two branches**  

    `git pull github master`  
    `git push github master`

* By default, no local branches are created for remote branches

# Working with remotes
## Remote Branches

You can get a good overview of all your local and remote branches and how they are trakcing with:
    git branch -avv

![img-19](img/git-19.png)

# Working with remotes
## Tracking branches

To create a local branch based on a remote branch:

    git checkout --track -b mywork github/mywork

To link an existing local branch to a remote branch:

    git branch --setup-stream github/mywork

You can verify this in the git config file (.git/config) in your repository:

![git-15](img/git-15.png)

# Working with remotes
## Merging while pulling

To better understand pulling, let's see what actually happens. Instead of using `git pull`, you also do (while on the master branch):

    # pull in the new objects
    git fetch github

    # merge the remote branch with the local branch
    git merge github/master

This works exactly the same as merging two local branches!  

# Working with remotes
## Avoiding useless merge commits
Merging a remote branch in your local branch can create a useless merge commit:

![git-16](img/git-16.png)

You can avoid this by rebasing instead of merging:

    git pull --rebase

or, if you want to do it manually:

    git fetch github
    git rebase github/master

# Some really cool tools

* Gitk: Part of the official Git distro, but it's UGLY UGLY UGLY
* Giggle & GitX: better looking, for Gnome & Mac
* tig: commandline. Also very useful to stage/unstage files
* Fugitive and gitv plugins for Vim
* meld: mergetool for linux, has support for git

![git-17](img/git-17.png)

# Git and Subversion (and friends)

Git has plugins available to migrate from and/or integrate into other versioning systems as well. One such plugin is **git-svn**.

With git-svn you can:

* migrate an existing subversion repository to a git repository, including all the history.
* use Git locally to do your work, but push to a central Subversion server.

# Git and Subversion
To locally use Git and push to a central Subversion server:

First 'clone' the Subversion repository into a local Git repo

    git svn clone -s http://svn.example.com/myproject
    # the -s means the subversion repo has a standard layout (trunk/ etc)

Now you can work as usual with your Git repository. Except... instead of running ``git pull`` to get the changes from other people, you now do:

    git svn rebase

And you don't do ``git push``, but:

    git svn dcommit

# Git Flow

![git-20](img/git-20.png)

# Git Flow

To use the '[Git Flow](https://github.com/nvie/gitflow)' branching strategy a Git plugin is available: git-flow. This plugin makes it really easy to follow Git Flow:

First initialize it to configure the names. (I recommend to use the default)
    git flow init

To start a feature branch:

    git flow feature start my-exiting-feature

To publish the feature (push it to the remote)

    git flow feature publish my-exiting-feature

To finalize the branch:
    git flow feature finish my-exiting-feature

# Git and Drupal

For the Drupal infrastructure (Update status, testing bot, etc) it is important to following the following conventions:

## Branch names
* 7.x
* 7.x-1.x
* 7.x-2.x

development releases will be created twice a day (7.x-1.x-dev)

## Tags
* 7.x-1.3-beta6: beta 6 release
* 7.x-3.1: final release
* valid: unstable, alpha, beta, rc

You can create other branches for features etc  

If you don't have commit access on drupal.org, you can contribute by creating git patches (with `git diff` or `git format-patch`)

# Suggested reading and resources

* Drupal Git documentation: [http://drupal.org/documentation/git](http://drupal.org/documentation/git)
* the git man-pages
* the .git-folder of a repository (just take a look, it's interesting)
* Pro Git: [http://progit.org/](http://progit.org/)
* The Git Community Book: [http://book.git-scm.com/](http://book.git-scm.com/)
* StackOverflow: [http://stackoverflow.com](http://stackoverflow.com/questions/tagged/git) has a lot of valuable questions and answers related to Git.

# Thanks!
This presentation is licensed under the [Creative Commons Attribution-Non Commercial-Share Alike 3.0 license](http://creativecommons.org/licenses/by-nc-sa/3.0/us/)

This presentation is available on github: [https://github.com/teranex/git-talk](https://github.com/teranex/git-talk)

Sources of images and inspiration:

* Pro Git: [http://progit.org/](http://progit.org/)
* The Git Community Book: [http://book.git-scm.com/](http://book.git-scm.com/)
* Webspecies (image of Git kid): [http://blog.webspecies.co.uk](http://blog.webspecies.co.uk/2011-05-23/the-new-era-of-php-frameworks.html)
