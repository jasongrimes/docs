Git cheatsheet
==============

Quick overview of basics: http://rogerdudler.github.com/git-guide/

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

* Create a new repository on github
* In local sources, run:

    git init
    git add .
    git commit -m 'Initial commit.'
    git remote add origin https://github.com/jasongrimes/<reponame>.git # Point the "origin" remote to the github repo
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

