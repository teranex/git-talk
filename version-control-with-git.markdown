title: Version Control with Git

# Version Control with Git
* Jeroen Budts
* PHP & Drupal Developer
* At Inuits - A Belgian Open Source Consultancy Company
* http://budts.be
* @teranex

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

# The Index (Staging Area)
The index contains the changes to will be added to your next commit. Your commit will /not/ contain the changes in your working directory. Only the changes that were added to the index!

![git-01](img/git-01.png)
