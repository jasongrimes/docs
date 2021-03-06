Git cheatsheet
==============

Quick overview of basics: http://rogerdudler.github.com/git-guide/

Checking out a repository
-------------------------

Create a working copy of a repository in the current directory.

    git clone username@host:/path/to/repository

ex.

    git clone git@github.com:jasongrimes/silex-simpleuser.git

Forking a repo
--------------

* Click the fork button for the repo on github.com
* Clone it to local machine (creates dir foo in cwd, you can rename later): `git clone git@github.com:jasongrimes/foo.git`
* Add upstream remote (since "origin" points to your fork): `cd foo; git remote add upstream https://github.com/originaluser/foo.git`
* Pull in changes from upstream (without modifying local files): `git pull upstream master` 

Add and commit
--------------

* Add to the index: `git add <file>`
* Commit: `git commit -m "Commit message"`

Now the file is in the HEAD of your local working copy, but not yet pushed to the remote repository.

Pushing changes
---------------

* Push changes to remote repository, branch "master": `git push origin master`
* Newly connect to a remote server (if local repo wasn't cloned from an existing repo): `git remote add origin <server>` 

Branching
---------

* Create a new branch named `feature_x` and switch to it: `git checkout -b feature_x`
* Create a new branch using your current sources and switch to it: `git branch feature-x; git checkout feature-x`
* Switch back to master: `git checkout master`
* Delete a branch: `git branch -d feature_x`
* Push a branch to remote repo: `git push origin <branch>`
  A branch is not available to others unless it has been pushed to the remote repository.

Merging a branch back into master:

    git checkout master
    git pull origin master
    git merge my_branch
    git push origin master

Update and merge
----------------

* Fetch remote changes, see what changed, and then merge them into local copy:

    git fetch origin
    git diff master origin/master
    git merge origin/master 

* Update local repository to newest commit: `git pull` (automatically does fetch and merge)
* Merge another branch into active branch: `git merge <branch>`
* Mark files as merged after manually resolving conflicts: `git add <file>`

Getting info
------------

* Show the working tree status: `git status`
* Show changes that have not yet been added to the index: `git diff`
* Show changes that are staged for commit: `git diff --cached`
* Show commit logs: `git log`
* Show details about an "object": `git show <object>`
* Show information about remotes: `git remote -v`

Creating a new github repo
--------------------------

Create a new repository on github. Then in local sources, run:

    git init
    git add .
    git commit -m 'Initial commit.'
    git remote add origin git@github.com:jasongrimes/<reponame>.git # Point the "origin" remote to the github repo
    git pull origin master  # Get README and .gitignore files from Github, if any
    git push origin master  # Send commits to Github

Rollbacks
---------

    git diff <commid id> <file>
    git reset --hard <commit id> <file>
    git push -f


Remove file from repo without deleting working copy
---------------------------------------------------

    git rm --cached <file>

Tagging
-------

See http://git-scm.com/book/en/Git-Basics-Tagging

* Creating a tag (annotated), in this case "1.4": `git tag -a 1.4 -m 'Release version 1.4'`
* Pushing tags to remotes (they are not pushed by default): `git push origin [tagname]`
* Push all tags to a the remote that are not already there: `git push origin --tags`
* List available tags: `git tag`
* List tags matching the given pattern: `git tag -l "1.0.*"`

Creating a global .gitignore
----------------------------

* Create a `~/.gitignore_global` file.
* `git config --global core.excludesfile ~/.gitignore_global`

Changing the URL of a remote repo
---------------------------------

    git remote set-url origin git://new.url.here


How to contribute to an open source project on Github
=====================================================

See http://www.hanselman.com/blog/GetInvolvedInOpenSourceTodayHowToContributeAPatchToAGitHubHostedOpenSourceProjectLikeCode52.aspx 

## If fixing a bug, first create an issue and claim it

This step is optional. Create a new issue on github. Claim it. Remember the number.

## Fork the project on Github

See https://help.github.com/articles/fork-a-repo

* Click the fork button on the project's Github page.
* Clone it to local machine: `git clone https://github.com/jasongrimes/oauth2-php.git`
* Configure the upstream remote:

    cd oauth2-php
    git remote add upstream https://github.com/FriendsOfSymfony/oauth2-php.git
    git pull upstream

## Make a branch for your issue

    git checkout -b update-phpdoc

## Commit the change and push to your remote

Include your issue number in the commit message, so it can be automatically associated.

    git add .
    git commit -m 'Update PHPDoc documentation of interfaces.'
    git push origin update-phpdoc

## Submit a pull request

See https://help.github.com/articles/using-pull-requests

