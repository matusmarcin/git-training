# git-training
Up your Git game.

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

This is how we merge `development` into the `master` branch.



## Rebase

## Reset

## Stashing

# Git Workflows

## Feature Branch Workflow

### Pull Request (PR)

## Forking Workflow

### Pull Request (PR)