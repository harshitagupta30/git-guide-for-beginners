# Git Guide For Open Source Beginners

This is a guide containing tricks which can help you to overcome your git fear.  

##Table of Contents:

1. [Forking a GitHub repository](#forking-a-github-repository)
 
 * Forking a repository
 * Forking a particular branch of a repository

2. [How to connect to original project / Setting up remote](#setting-up-remote)

3. [Working with Branches](#setting-up-a-branch-and-working-with-it)

4. [Opening a Pull Request](#opening-a-pull-request)

5. [Merge conflicts](#merge-conflicts)
  
  * [Resolving merge conflicts after the PR sent and other commits get merged leading to conflicts](#resolving-merge-conflicts-after-the-pr-has-been-sent-without-creating-a-new-commit)

6. Squashing Commits

7. Undoing commits
  
  * Deleting commits locally
  * Deleting commits after been pushed to remote

8. Changing the commit message after push

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


##Setting up remote

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


##Setting up a branch and working with it

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


##Opening a Pull Request

Once you push a new branch up to your repository, GitHub will prompt you to create a pull request (I’m assuming you’re using your browser and not the GitHub native apps). The maintainers of the original project can use this pull request to pull your changes across to their repository and, if they approve of the changes, merge them into the main repository.


##Merge Conflicts

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


###Resolving merge conflicts after the PR has been sent without creating a new commit  

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

