---
layout: post
title: Advanced Git
---

Getting into the nitty gritty parts of git

![_config.yml]({{ site.baseurl }}/images/rebase/rebase.png)

## Git Rebase
<br>

#### What is git Rebase?

Git rebase is a way of aligning the git history from different branches into a final commit. Rather than having your project history spread out over multiple branches, git rebase simplifies project history by bringing everything together.

#### Advantages & Disadvantages of git Rebase

##### Advantages 

* Clean Project History 
* Allows you to bring branches together without merge commits

##### Disadvantages 

* It may be harder go back to find specifics on a branch within your git history
* May cause issues when working with a remote repo, if you merge at the wrong time.

<br>

#### Rebase Example

<br>

##### Initializing the Repo

With your repository setup you'll make an initial commit.
![_config.yml]({{ site.baseurl }}/images/rebase/01initial-commit.png)

<br>

##### Adding a Branch

With your repo setup and committed, now's a good time to setup a feature branch. 

```
git branch feature
git checkout feature
```
This creates a branch named feature, and moves you over to the feature branch.
![_config.yml]({{ site.baseurl }}/images/rebase/02create-branch.png)

<br>

##### Committing to the Branch
Now that you in your feature branch, it's time to add whatever feature of the project you're working on. This could be 10+ commits, or just two like in this example where I added a few things to the example HTML page.
![_config.yml]({{ site.baseurl }}/images/rebase/03branch-commits.png)

<br>

##### Jumpig Back to the Master Branch
Now that the feature branch is done, let's go back and add some changes to the master branch. First you'll need to jump back over to the master branch from the feature branch
```
git checkout master
```
If this was a group project with a remote repository, now would be a good time to pull down any changes to the master branch from the remote repo.
![_config.yml]({{ site.baseurl }}/images/rebase/04switch-master.png)

<br>

##### Adding Changes to the Master Branch

Adding two commits within the master branch.
![_config.yml]({{ site.baseurl }}/images/rebase/05master-commits.png)

<br>

##### Moving Back to Feature & Rebasing

Now that the master branch and feature branch are both ready to go, you can pull the git history of the feature branch by rebasing it. First you'll need to switch back to the brach you want to rebase which in this case is the feature branch, and then rebase it to the master branch
```
git checkout feature
git rebase master
```
![_config.yml]({{ site.baseurl }}/images/rebase/06switch-back-feature.png)
![_config.yml]({{ site.baseurl }}/images/rebase/07rebase-feature.png)

<br>

##### Feature Branch Changes

Now that the feature branch has been rebased, you can see the changes that were made in the JS file in the master branch are reflected in the feature branch.

![_config.yml]({{ site.baseurl }}/images/rebase/08feature-changes.png)

<br>

##### Merging Everything together

After rebasing the feature branch, you'll still need to move over to the master branch.
```
git checkout master
```
Now that you're in the master branch, you can merge the two branches together.
```
git merge feature
```
This will bring the two branches together. After doing this you'll be able to see the changes you made in both branches all being reflected in the master branch, while having clean git history.
![_config.yml]({{ site.baseurl }}/images/rebase/09final-steps.png)

<br>

#### When you shouldn’t rebase with a remote repo

If you’re working with a remote repo in a team setting and pull down a copy of the repo to start working on a feature branch, the master branch on the remote repo may become several commits ahead of the master branch on your local repo, due to your team adding to the remote repo.

After working on your feature branch, you’ll want to jump back over to the master branch and pull down any changes from the remote repo to make sure your copy of the master branch is up to date. Now that your master branch is up to date with the master branch on the remote repo, you can jump back over to the feature branch and rebase that branch to the master branch without issues.

If you were to rebase your feature branch to the master branch first, then pull down updates to the master branch from the remote after the fact, you could loose work being that your commits from the feature branch that are now in line with the master branch, will be washed out by the commits on the master branch pulled down from the remote repo that your master branch is out of sync.

<br>
<br>

## Git Reset, Checkout & Reverse

<br>

#### What is Git Checkout?
As seen in the rebase example, git checkout can be used to move back and forth between branches by stating "checkout" and then the name of whichever branch you want to jump over to.

Git checkout can also be used to view specific commits in your code by stating "checkout" and the id of the commit. Something key to note here is that when you move your HEAD pointer to a commit, you've entered what's called a "detached head state". If you were to make a commit in this time, it creates a commit that isn't attached to any sort of branch.

If you wanted to go back to a certain commit and create a branch that stemmed from that commit, you could no problem. Creating a branch first helps you avoid creating orphaned commits that aren't attached to any branch.

Examples
![_config.yml]({{ site.baseurl }}/images/checkout/git-log.png)
Here you can view a git repo with several commits. After entering "git log" in the terminal you can view each commit with the corresponding id number and commit message. You can use this id to jump strait to that commit using the checkout command.

<br>

![_config.yml]({{ site.baseurl }}/images/checkout/git-checkout.png)

After checking out a specific commit, you can see the warning about being in a "detached head state" and how you have the option to create a branch from this moment in git history. To jump back to the current state of the project, you can either jump back to a specific branch or just back to master.

<br>

#### What is Git Reset?
Similar to git checkout, git reset takes a git id as a parameter and jumps back to that commit. The key difference here is that it's removing the commits in-between where you are and the commit you specified. Here is an example using the same checkout repo as before:

![_config.yml]({{ site.baseurl }}/images/reset/reset-before.png)

Reseting back to commit "change 3" skips over and removes "commit 4". After running this command you are left with:

![_config.yml]({{ site.baseurl }}/images/reset/reset-after.png)


<br>

#### What is Git Revert?
Unlike resetting a git commit, reverting keeps your git history and commits more in tact, and reverts the changes made only in a specified commit. Say you had commit 1, 2, 3 and are about to make a 4th commit when you realizes the changes you made for commit 2 are no longer needed. If you used git reset 2, it'd delete the changes you made in commit 3, but if you used git revert 2 instead, the changes made in your third commit wouldn't be deleted.

Here is an example:

![_config.yml]({{ site.baseurl }}/images/revert/revert1.png)

![_config.yml]({{ site.baseurl }}/images/revert/revert2.png)

Reverting keeps the coming on file, removes the changes made in that commit after merging the changes.

<br>

## Git Submodules

![_config.yml]({{ site.baseurl }}/images/submodule.png)


#### What are git submodules?

A git submodule is a git repository inside another git repository, with its own history and commits.

<br>

#### When would you use a submodule?

If your project dependancies need to be tightly controlled and tracked, making them their own repo could be a good idea to track changes. Another scenario that might be beneficial for a sub-dependancy is when and if you outsource a portion of a project to another team. 

<br>

#### What are the advantages and disadvantages of git submodules?

One of the advantages I mentioned earlier is that it gives you the option to closely follow changes within a project like a sub-dependency folder, or server folder. One of the downsides of a submodule is that it could add complications to forking, cloning, and branching.

