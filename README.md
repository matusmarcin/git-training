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

Now see the status of your repo.

```
git status
```

Make git track files (or directories):

```
git add hello.txt
git add assets
```

Now check the `status` again and see there are changes *to be committed* and *not staged* (if you have modified something). Let's stage those changed files too.

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

Now the changes are committed into our repository (*locally*) so let's push them to remote origin.

```
git push remote origin
```

Depeding on your circumstances, a `git push` might be enough.

Let's what we have done using `log`.

```
git log
git log --oneline
```

We can see our commit so hurray, great job everybody.

Okay, those are the uber-basics. Let's get to the basics now.

# Branches

You cannot really say you can use git unless you're using branches. Branches in git are really easy to use and often part of your daily workflow.

Inspect the branches on your repo:

```
git branch
```

Remembed this show only *local* branches. Explore the origin using `-a`:

```
git branch -a
```

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

How did I get this far into a tutorial without a `git pull`? It's the opposite of `push`. With `pull` you get changes from the remote repo into your own. Unless there are conflicts, git merges your changes automagically.

```
git pull
```

