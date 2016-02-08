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
git commit -a -m ""