# Introduction to git

Welcome to this introduction to git! We'll have a look at some basic commands to get you covered.

## Get started

Create a directory, change into it and create an empty repository:

```
git init
```

Git will reply

```
Initialized empty Git repository in .git/
```

## Adding files to your git

The files you want git to track need to be added to the repository **explicitly**. Saving a file to the `git`-folder on your hard drive is **not enough**!

Create a file `tutorial.md` and add it to git:

```
git add tutorial.md
```

To add all files that exist in the working directory, you may use

```
git add .
```

## Taking snapshots

Everytime you want to take a snapshot of the current state of your working directory, you need to `commit` things to git:

```
git commit
```

This will prompt you for a commit message. Make sure to describe your commits in a meaningful way (relevant: https://xkcd.com/1296/ ).

![Use speaking commit messages!](git_commit.png)

## Making changes

Now, we do two things: we add a new file to the git, and we modify the content of an existing file. 

What needs to be done to add a new file to the git? Correct, we need to add it:

```
git add git_commit.png
```

And what happens to the changes made to `tutorial.md`? Let's check using `git status`:

```
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    new file:   git_commit.png

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   tutorial.md
```

As you can see, git is about to include the new file and it noticed your changes to `tutorial.md`. **But**, these changes are not yet "added to commit"! So, if you do a `git commit` just now, only the new file is added! To also include the changes made to `tutorial.md` to the revision, you need to add the file again using `git add tutorial.md`. Alternatively, and maybe a little handier, you can use

```
git commit -a
```

This automatically adds all changes to the commit and then commits the revision all in one step. Be aware, though, that this doesn't add new files. New files have to be explicitly added as shown above.





## Branches in git

In a single git repository, you can have multiple branches. A branch is a parallel line of development for which you create a copy of the current state of the working directory. Use 

```
git branch braching
```
to create a branch called `branching`. If you now run

```
git branch
```
it says that your repository consists of two branches at this point:

```
  branching
* master
```

The `master` branch is the trunk of the repository. It is created automatically together with the empty repository. The asteriks marks the currently active branch. To switch to the `branching` branch, use

```
git switch branching
```

You can now make changes to your file `tutorial.md` and commit them using

```
git commit -a
```

When you switch back to the master branch (`git switch master`) you will notice that your changes are no longer visible.


## Merging branches

When you work with branches, you will come to the point where you need to merge the changes made in the branch back into your `master`-branch. To do so, switch to the `master`-branch and type

```
git merge branching
```

to merge the changes made in the `branching`-branch back into your `master`. If there are no conflicts, you're done.  In our tutorial, of course, you're not done yet since there is a conflict on the `tutorial.md`.

### Using a merge tool

To resolve conflicts on text-based files, it is recommended to use a diff viewer. That is a tool which can show two versions of files next to each other and help you comparing the versions. A very good choice that works across all platforms is [**meld**](https://meldemerge.org)^[[https://meldemerge.org](https://meldemerge.org)].

If you need help configuring your git installation to use `meld` as your merge- and diff-tool, you can [start reading here this stackoverflow answer](https://stackoverflow.com/a/34119867)^[[https://stackoverflow.com/a/34119867](https://stackoverflow.com/a/34119867)].

You can use this approach to configure using meld as both your difftool as well as your mergetool. Run these commands in git bash **on Linux** (or **Mac**):

```
git config --global diff.tool meld
git config --global difftool.meld.path "/usr/bin/meld"
git config --global difftool.meld.cmd 'meld "$LOCAL" "$REMOTE"'
git config --global difftool.prompt false

git config --global merge.tool meld
git config --global mergetool.meld.path "/usr/bin/meld"
git config --global mergetool.prompt false
git config --global mergetool.meld.cmd 'meld "$LOCAL" "$MERGED" "$REMOTE" --output "$MERGED"'
```

If you're **on Windows**, most of the commands are the same but with an important difference:

```
git config --global diff.tool meld
git config --global difftool.meld.path "C:\Program Files (x86)\Meld\Meld.exe"
git config --global difftool.meld.cmd 'meld "$LOCAL" "$REMOTE"'
git config --global difftool.prompt false

git config --global merge.tool meld
git config --global mergetool.meld.path "C:\Program Files (x86)\Meld\Meld.exe"
git config --global mergetool.prompt false
git config --global mergetool.meld.cmd 'meld "$LOCAL" "$MERGED" "$REMOTE" --output "$MERGED"'
```

This might differ slightly, if you chose to install meld to a different location on your hard drive.

It is recommended to play around with this a little bit. For example, you can also use 

```
git config --global mergetool.meld.cmd 'meld "$LOCAL" "$BASE" "$REMOTE" --output "$MERGED"'
```

as your mergetool command (on both Linux/Mac and Windows). This will result in a different version being shown in the center pane while merging: `$MERGED` is the partially merged version of the file. It will contain text-based indicators of where the version came from. `$BASE` is the version of the file as it was when the branch-version of the file was originally created.

Just to mention: using a diff viewer is helpful for all kinds of merges, not only when you merge in the changes from a local branch.

### Doing the actual merge

However, when you're ready using the merge-tool of your choice type

```
git mergetool tutorial.md
```

In case you're using meld, it opens meld with three file panes. Regardless of your configuration of using `$BASE` or `$MERGED`, you have to edit the middle pane for the final output of the merged file version. Saving the middle file and closing meld finishes the merge process. Running a `git status` after a successful merge shows:

```
On branch master
Your branch is based on 'origin/master', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)

All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
    modified:   tutorial.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    tutorial.md.orig

```

If you now run 
```
git commit -a
```
the merge is done and a new merged revision is created. As you can see, there is also a new file `tutorial.md.orig` which contains the automatically merged version.

Have a look at `gitk` to see a nice graph-based visualization of what you just did.
