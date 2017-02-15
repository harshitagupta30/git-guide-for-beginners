# Git Guide For Open Source Beginners

This is a guide containing tricks which can help you to overcome your git fear.  

##Table of Contents:

1. [Forking a GitHub repository](#forking-a-github-repository)
 
 * Forking a repository
 * Forking a particular branch of a repository

2. [How to connect to original project / Setting up remote](#setting-up-remote)

3. Branching and pull requests

5. Merge conflicts
  
  * Resolving merge conflicts without creating new commit.
  * Fixing PR when other changes get merged leading to conflicts

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

Now you're getting ready to start hacking, you'll want to switch off of the `master` branch and onto a different branch for your _new feature_. It's **important** to do this because you can only have one **Pull Request per branch**, so if you want to submit more than one fix, you'll need to have multiple branches and which can be created like this: 

`git branch <newfeature>`

Then switch to it like this:

`git checkout newfeature`

Or to do this whole thing in one you can do like this:

`git checkout -b <newfeature>`

