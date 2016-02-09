# git-training
Up your Git game.

  * [Basics](#basics)
  * [Branches](#branches)
  * [Merging](#merging)
    * [Merging from a different branch](#merging-from-a-different-branch)
  * [Rebase](#rebase)
  * [Other useful commands](#other-useful-commands)
    * [Reset](#reset)
    * [Revert](#revert)
    * [Stashing](#stashing)
  * [Git Workflows](#git-workflows)
    * [Feature Branch Workflow](#feature-branch-workflow)
      * [Pull Request (PR)](#pull-request-pr)
    * [Gitflow Workflow](#gitflow-workflow)
    * [Forking Workflow](#forking-workflow)
  * [Resources](#resources)

## Basics

Get the repo:

```
git clone git@github.com:matusmarcin/git-training.git
```

Edit something (like this `README.md`) or create new files:

```
echo "hello world">hello.txt
mkdir assets
echo "assets will go here">assets/readme.txt
echo "another file">assets/file.txt
```

Now see the status of your repo:

```
git status
```

Make git track files (or directories):

```
git add hello.txt
git add assets
```

Now check the `status` again and see there are changes *to be committed* and *not staged* (if you have modified something). 

![git status](/assets/status.png)

Let's stage those changed files too:

```
git commit README.md
```

This will stage one file for commit `README.md` and open default editor for you to type the commit message.

Often you have *many* changed files so here you go: `-a` stands for *all*:

```
git commit -a
```

Let's also get rid of the editor and just type in the message using `-m`:

```
git commit -a -m "Fixed that nasty bug."
```

![git commit](/assets/commit-a-m.png)

Now the changes are committed into our repository (*locally*) so let's push them to remote origin.

```
git push remote origin
```

![git push](/assets/push.png)

Depeding on your circumstances, a `git push` might be enough.

Let's what we have done using `log`.

```
git log
git log --oneline
```

![git log oneline](/assets/log-oneline.png)

We can see our commit so hurray, great job everybody.

Okay, those are the uber-basics. Let's get to the basics now.

# Branches

_**Understand the sh*t out of branches by studying up on [this tutorial from Atlassian](https://www.atlassian.com/git/tutorials/using-branches)**_

You cannot really say you can use git unless you're using branches. Branches in git are really easy to use and often part of your daily workflow.

Inspect the branches on your repo:

```
git branch
```

Remembed this show only *local* branches. Explore the origin using `-a`:

```
git branch -a
```

![git branch](/assets/branch.png)

Probably just *master*. That's the default. Let's create a branch.

```
git branch development
git checkout development
```

We've created a branch called development and then switched to it with another command. This can be done with one just like so:


```
git checkout -b development
```

Now you can go ahead and do your branch-specific changes and commit them. When you try to push, you need to set what this branch will be called on the origin repo. This is done with `--set-upstream` or `-u`. We use the same name. (Why not?)

```
git push --u origin development
```

Now we have multiple branches and when running `git branch` we can see the current one highlighted with an asterisk.

To switch between existing branches just use `git checkout <branch>`.

# Merging 

_**Make friends with `merge` in this [tutorial by Atlassian](https://www.atlassian.com/git/tutorials/using-branches/git-merge).**_

As soon as you use branches, you need to be able to merge. 

Switch to `development` branch and make changes to `development.txt` file. Commit your changes. Let's have someone else do the same but also `push` their changes before you manage to. When you try to push them, git warns you to `pull` first. 

![git push but pull first](/assets/push-rejected.png)

How did I get this far into a tutorial without a `git pull`? It's the opposite of `push`. With `pull` you get changes from the remote repo into your own. Unless there are conflicts, git merges your changes automagically.

```
git pull
```

![git pull auto-merge no conflicts](/assets/pull-auto-merge.png)

That's it, you don't have to do anything. Unless it fails :-)

![git pull auto-merge with conflicts](/assets/pull-merge-conflict.png)

When this failes and you have conflict, git leaves the changes in the files. Below *HEAD* mark are your changes and above the *commit hash* are changes from the remote repo. Use your smarts to merge this properly or consult with the person that made those changes. Then, commit this to finish the merge:

```
git commit -a
```

## Merging from a different branch

Now let's assume you've done some development work on that branch and those changes are now ready to be merged back into `master`. This can be either simple or a bit tricky.

```
git checkout master
git merge development
```

This is how we merge `development` into the `master` branch. In various cases git cleverly decides the correct strategy and just merges it properly. However, if your work on `development` took a while and `master` was changed at the same time, you probably need to... follow your git workflow. Especially if you're going to have conflicts.

# Rebase

**Rebasing takes a branch and makes it a base for our current branch. It does not directly change the branch but the idea is that we will merge the current branch into that branch in the next step.**

_**Make sense out of rebase with these two tutorials from Atlassian: [`git rebase`](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase) and [Merging vs. Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)**_

Rebasing can cleanup git history nicely. Rebasing must be used with caution. A good rule is to never use `rebase` on already published code. So the use case here is, commit some stuff localy, then rebase it into a single commit or nicer commits and push.

Let's start with a new branch for our feature:

```
git checkout -b some-feature
```

Do some development... and then rebase:

```
git rebase master
git checkout master
git merge some-feature
```

And this is not so bad, we now all commits from `some-feature` at the top of the master. Perhaps we could've squashed those commits into one, so that the history looks real nice. This can be done with interactive rebase using `-i` flag.

```
git rebase -i master
```

Or let's say we don't want to change all the commits, only last three:

```
git rebase -i HEAD~3
```

Anyways, this gives us a text editor with all the commits and options to take them in, change the message, squash them together or leave them out.

![git rebase interactive](/assets/rebase-i.png)

```
reword abfce88 More development
fixup 4acb83d More development
fixup 93f53ee More development
```

This way we squash the two commits together with the third one - wihout including their commit message (`fixup`) - and `reword` the message for the first one. Reword option shows us the editor again to type in a new message.

The other day I have found [`git merge-base`](https://git-scm.com/docs/git-merge-base) but having never used it, I am just going to tell to read about it by yourself.

# Other useful commands

## Reset

Don't really do this. Only on your own local unpublished code. It's like a permanent undo.

```
git reset <commit>
```

[Read more about `reset`.](https://www.atlassian.com/git/tutorials/undoing-changes/git-reset)

## Revert

Revert is a good undo you can use, if you want to fix a screw up. It actually commits a new change that reverts exactly what you wanted to. This prevents git from losing history (so that everybody can know about your screw up) - and that's a good thing.

```
git revert <commit>
```

[Read more about `revert`.](https://www.atlassian.com/git/tutorials/undoing-changes/git-revert)

## Stashing

Would you ever decide you don't want to commit your work but don't want to loose it `stash` is the solution.

```
git stash
git stash apply
```

```
git stash list
git stash apply stash@{2}
```

Or create a branch from your stash!

```
git stash branch testchanges
```

# Git Workflows

Workflows are about how we organise our work in git repo. We already know most of the commands we need for this, we're only going to be using them. 

## Feature Branch Workflow

_**[This article is a much better resource.](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)**_

Simply put: *Every feature is a new branch.*

In reality, you'll probably have to deal with conflicts in case someone else merges their branch before you do, so you're probably better with something like this:

1. Create a branch (from `master`) for your new feature
2. Do your work
3. Checkout and pull `master`
4. Merge `master` into your branch to resolve conflicts and catch-up
5. Merge your branch into `master`

To do a really good job you can use **`rebase` in the step 4**. And you really should create **a pull request to merge your branch in step 5**.

### Pull Request (PR)

You can use GitHub to create a pull request (PR). Just find a button somewhere and click it. Then choose *base* as where you want your stuff to end up and *compare* as your stuff you want to merge. So the pull request is a request to merge *compare* into *base*, i.e. *your branch* into *master* or *development*.

![pull request](/assets/open-pr.png)

## Gitflow Workflow

_**This is [the guy who came up with this](http://nvie.com/posts/a-successful-git-branching-model/) and this is [another good article on it](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).**_

This is arguably the best and most used workflow. It has:

* `master`
* `development`
* `feature-*` branches created from `development`
* `hotfix-*` branches created from `master`
* `release-*` branches created from `development` to be merged into `master`

[<img src="/assets/git-model@2x.png" width="500px" alt="gitflow" />](http://nvie.com/img/git-model@2x.png)

_[Image source](http://nvie.com/posts/a-successful-git-branching-model/)_

## Forking Workflow

_**[Learn about forking workflow here!](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)**_

This one is different from the others above since the workflow is:

1. You fork the repository
2. Implement your changes
3. Create a PR for a project maintainer to merge your changes

You can and still should use _feature branch workflow_ inside your own repository.

With this workflow you have **two remotes** where you can call your own `origin` and the project maintainers `upstream`.

# Resources

* [git-scm.com - A book, reference and all](https://git-scm.com/doc)
* [Great tutorials by Atlassian](https://www.atlassian.com/git/tutorials)
* [Learn Git in 15 minutes](https://try.github.io/)
* [List of good resourced compiled by GitHub](https://help.github.com/articles/good-resources-for-learning-git-and-github/)

Have fun!
