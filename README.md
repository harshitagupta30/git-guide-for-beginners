# Git Guide For Open Source Beginners

This is a guide containing tricks which can help you to overcome your git fear.  

## Table of Contents:

1. [Forking a GitHub repository](#forking-a-github-repository)
 
 * Forking a repository
 * Forking a particular branch of a repository

2. [How to connect to original project / Setting up remote](#setting-up-remote)

3. [Working with Branches](#setting-up-a-branch-and-working-with-it)

4. [Merge conflicts](#merge-conflicts)
  
  * [Resolving merge conflicts after the PR sent and other commits get merged leading to conflicts](#resolving-merge-conflicts-after-the-pr-has-been-sent-without-creating-a-new-commit)

5. [Squashing Commits](#squashing-the-commits)

6. [Submitting a Pull Request](#submitting-a-pull-request)

7. [Renaming the commit message after push](#renaming-a-commit-message-after-push)

8. [Undoing commits](#permanently-removing-commit-from-remote-branch-/-revert-a-commit-already-pushed-to-a-remote-repository)
  

## Forking a Github Repository

Forking it is basically making a copy of the repository, but with a link back to the original. 

Forking a repository is really straightforward:

1. Make sure you’re logged into GitHub with your account.
2. Find the GitHub repository with which you’d like to work.
3. Click the Fork button on the upper right-hand side of the repository’s page.

<div style="text-align:center">
<img src = "https://github.com/harshitagupta30/git-guide-for-beginners/blob/master/images/VWFCB.png"
style="height:50px;">
</div>
<br />

Now you have a copy of the original repository in your GitHub account but to make changes to your copy of the repository you need to make a local clone for it. Before running the `git clone`, you need to determine the URL for the forked repository. That’s pretty simple: just navigate to the forked repository (this is the copy of the original repository residing in your GitHub account) and look on the right-hand side of the web page. You should see an area that is labeled “HTTPS clone URL”. Simple copy the URL there, and then use it with git clone like this 

`git clone <your ssh/git url> `

On running this Git will copy down the repository, both contents and commit history, to your system. It will also add a Git remote called origin that points back to the forked repository in your GitHub account.

Now, if you want to clone some particular branch of your repository then use this instead of the above one

`git clone -b <branch> <your ssh/git url>`


## Setting up remote

Git already added a Git remote named origin to the clone of the Git repository on your system, and this will allow you to push changes back up to the forked repository in your GitHub account using git commit (to add commits locally) and git push.
To add a Git remote pointing back to the original repository (the one you forked on GitHub) , like this:

` git remote add upstream <your ssh/git url> `

or Suppose you want to point at some particular branch let's say `master`, then  use it like this: 

` git remote add --track master upstream < your ssh/git url> `

This will add the original project as a remote named 'upstream'. To get/update the code, type:

` git fetch upstream `

Then, to merge it into your own project, type:

` git merge upstream/master `

Now you'll have an up-to-date version of the upstream code in your current branch.


## Setting up a branch and working with it

**Branch** is a way to create a separate line of changes that is independent from the main line (often referred to as “master”).

Now you're getting ready to start hacking, you'll want to switch off of the `master` branch and onto a different branch for your _new feature_. It's **important** to do this because you can only have one **Pull Request per branch**, so if you want to submit more than one fix, you'll need to have multiple branches and which can be created like this: 

`git branch <newfeature>`

Then switch to it like this:

`git checkout newfeature`

Or to do this whole thing in one command you can do like this:

`git checkout -b <newfeature>`


##Pushing changes to GitHub

So let’s say you’ve made the changes necessary to implement the specific feature or enhancement (the one “logical change”), and you’ve committed the changes to your local repository. The next step is to push those changes back up to GitHub.

If you were working in a branch called new-feature, then pushing the changes you made in that branch back to GitHub would look like this:

`git push origin <newfeature>`

The generic form of this command is

`git push <remote> <branch>`


## Merge Conflicts

**Merging** is the act of integrating another branch into your current working branch. While working on a shared project sometimes it happens that two people changed the same lines in that same file, or one of the person decided to delete it while the other person decided to modify it, _Git_ simply cannot know what is correct. So it marks the file as having a conflict - which we'll have to solve before we can continue our work.

A typical merge conflict message looks like this:

`$ git checkout newfeature`

`Switched to branch 'newfeature'`

`$ git merge master`

`Auto-merging filename.java`

`CONFLICT (content): Merge conflict in filename.java`

`Automatic merge failed; fix conflicts and then commit the result.`

When faced with a merge conflict, the first step is to understand the reason behind the conflict. Git tells you that you have "unmerged paths" (which is just another way of telling you that you have one or more conflicts) via "git status" which looks like this :

`$ git status`

`On branch newfeature`

`You have unmerged paths`

`(fix conflicts and run "git commit")`


`Unmerged paths:`

`(use "git add <file>..." to mark resolution)`


      `both modified: filename.java`


`no changes added to commit (use "git add" and/or "git commit -a")`


Now it's the time to have a look at the contents of the conflicted file. Git marks the problematic area in the file by enclosing it in `<<<<<<< HEAD" and ">>>>>>> [other/branch/name]`.

The contents after the first marker originate from your current working branch. After the angle brackets, Git tells us where (from which branch) the changes came from. A line with "=======" separates the two conflicting changes. Our task is to identify and decide which piece of code is required and which is to be removed.


### Resolving merge conflicts after the PR has been sent without creating a new commit  

Many times while working, we feel the requriment of updating our code, so that we can have an up-to-date version of the upstream code in our current branch. During the process of merging the two codes, the one on which we are working and the another which has been fetched from the remote, Git creates a commit by itself which looks like this: 

`Merge master into newfeature.`

 Also, you submitted a Pull Request to the main repository but before the request could be merged, changes were made in the repository and now your PR presents merge conflicts. You could fetch the work, resolve merge conflicts and push again but that will lead to `MERGE COMMIT` which you don't want to create because Git doesn't allow rebasing for merge commits so there is no way to rebase it into the previous commit.
 
So, to avoid this you have to follow the following steps:

- Checkout your branch on which you want to update the code

`git checkout newfeature`

- Fetch the changes from the upstream code

`git fetch upstream`

- Remove/Stash any local changes, if any 

`git stash`

- Rebase to the latest branch in upstream (let say master in this case)

`git rebase upstream/master`

And suppose in between any merge conflicts occur then 

- Fix the conflicts in the project then add the files by using 

`git add <file name>` or `git checkout -- <file name>`

- After the changes have been fixed run 

`git rebase --continue`

Now the changes from the upstream have been applied and the work/ local changes you made has been applied on top of it

- Force push to your branch to update the PR by using

`git push --force origin newfeature`


### Undoing a merge

You can return to the state before you started the merge at any time like this: 

`git merge --abort`

or in case you've made a mistake while resolving a conflict and realize this only after completing the merge, you can still easily undo it like this:

`git reset --HARD`

It just roll back to the commit before the merge happened and start over again.


## Squashing the commits

Often while working on a feature you might create multiple commits but usually it is expected to submit entire feature change is in the form of one single commit before sending pull request back upstream. To squash all the commits into one we do rebasing. 

First, you need to take a look at the commits you've made with `git log` and figure out the commits that you want to squash. If you wanted to squash the into one, you'd open up an an interactive rebase like this:

` git rebase -i HEAD~3`

where -i is for interactive rebase
      ~3 stands for the number of commits you want to squash
      
The above command will bring you into your editor with some text that will look something like this:

`pick df94881 Allow install to SD` 
 
`pick a7323e5 README newfeature`

`pick 3ead26f rm classpath from git` 

To squash those commits into one, change to something like this:

`pick df94881 Allow install to SD` 
 
`squash a7323e5 README newfeature`

`squash 3ead26f rm classpath from git` 

Then, save/quit, and you'll be brought to into another editor session, describe the changes as well as you can and save/quit again. Now you're commits are squashed into one and you're ready to submit a `pull request`.


## Submitting a Pull Request

Once you've commited and squashed your changes, push them to your remote like this:

`git push origin newfeature`

Once you push a new branch up to your repository, GitHub will prompt you to create a pull request (assuming that you’re using your browser and not the GitHub native apps). The maintainers of the original project can use the pull request to pull your changes across to their repository and, if they approve of the changes, merge them into the main repository.

Then, click on the little button that says 'Pull Request'. This will bring you to a page asking you to describe your change. Describe it thoroughly.


## Renaming a commit message after pushed

Sometimes after sending a pull request you just realised that the you've made an error while writing a commit message and the project maintainer has asked you to rename it which you can do like this:

`git commit --amend -m "New commit message"`

and then to push it back to the upstream and update your pull request which can be done like this:

`git push --force origin newfeature`


## Permanently removing commit from remote branch / Revert a commit already pushed to a remote repository

Reverting a commit means to create a new commit that undoes all changes that were made in the bad commit.
You've just pushed your local branch to a remote branch, but then realized that one of the commits should not be there, or that there was some unacceptable typo in it. This can be fixed in many ways.

- Correct the mistake in a new commit

Simply remove or fix the bad file in a new commit and push it to the remote repository.

- Revert the full commit

Sometimes you may want to undo a whole commit with all changes. Instead of going through all the changes manually, you can simply tell git to revert a commit, which does not even have to be the last one. The bad commit remains there, but it no longer affects the the current master and any future commits on top of it. You can revert a commit like this:

`$ git revert dd61ab32`

where `dd61ab32` is the commit id.

- Revert the full commit with history rewriting 
  
#### Delete the last commit
  
Let's say we have a remote `https://github.com/harshitagupta30/open-event-android.git` with branch development that currently points to commit `dd61ab32`. We want to remove the top commit which we can do like this: 

`git push https://github.com/harshitagupta30/open-event-android.git +dd61ab32^:develpoment`

Where git interprets x^ as the parent of x and + as a forced non-fastforward push. 

If you have the development branch checked out locally, you can also do it in two simpler steps: 

First reset the branch to the parent of the current commit

`git reset HEAD^ --hard`

Force-push it to the remote.

`git push https://github.com/harshitagupta30/open-event-android.git -f` 

#### Delete the second last/any commit
 
Let's say the bad commit `dd61ab32` is not the top commit, but a slightly older one, e.g. the second last one. You want to remove it, but keep all commits that followed it. In other words, you want to rewrite the history and force the result back to `remote/master`. The easiest way to rewrite history is to do an interactive rebase down to the parent of the offending commit like this:

`git rebase -i dd61ab32^`

This will open an editor and show a list of all commits since the commit we want to get rid of:

`pick dd61ab32`

`pick dsadhj278`

`... `

Simply remove the line with the offending commit, likely that will be the first line _(vi: delete current line = dd)_. Save and close the editor _(vi: press :wq and return)_. T

`git rebase --continue`

Resolve any conflicts if there are any, and your local branch should be fixed. Force it to the remote like this:

`git push https://github.com/harshitagupta30/open-event-android.git -f` 



### Renaming a branch
You can rename the current branch like this: 

`git branch -m old-name-of-branch-on-github new-name-for-branch-you-want`








