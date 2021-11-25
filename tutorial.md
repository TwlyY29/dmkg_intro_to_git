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





## Reverting last changes

In case you want to undo all the most recent changes, take these steps:

1. Make sure that your changes are not staged for commit by running `git status`. Your changes must appear under the section "`Changes not staged for commit`"

2. You have three options:
    1. To overwrite local changes:
        
        ```
        git checkout -- <file>
        ```
        
    2. To save local changes so you can re-use them later
        
        ```
        git stash
        ```
        
    3. To discard local changes to all files, permanently
        
        ```
        git reset --hard
        ```
        
        Be careful with this command, though. You'll lose any changes in the working directory. 

In case you have local changes that are staged already, [have a look at the documentation for undoing those](https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git/#undo-staged-local-changes)^[[https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git/#undo-staged-local-changes](https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git/#undo-staged-local-changes)].


## Working with the history

You can go back in your commit history by "checking out" specific revisions. Use 

```
git log
```

to view a list of the commits of your repository:

```
commit b1752014f34cec5499c00ace43932cfb93256880 (HEAD -> master, tag: 300-working_with_git_history)
Author: Mirco Schoenfeld <mirco.schoenfeld@uni-bayreuth.de>
Date:   Wed Nov 24 12:11:29 2021 +0100

    Added a section about working with the git history


commit 62d0f432cc77e529ebaca90c88af574e0b69da48 (tag: 200-advanced_features)
Author: Mirco Schoenfeld <mirco.schoenfeld@uni-bayreuth.de>
Date:   Wed Nov 24 12:03:07 2021 +0100

    Added documentation of advanced features. Most importantly, this includes reverting last changes


commit 07ebd3386ea1831b8b57b5306ba87cef2fed6223 (tag: 100-first_changes)
Author: Mirco Schoenfeld <mirco.schoenfeld@uni-bayreuth.de>
Date:   Wed Nov 24 06:44:32 2021 +0100

    Added a chapter "Making changes"

commit 67321e834d987743229f04eae49564544c8c2eab (tag: 000-get_started)
Author: Mirco Schoenfeld <mirco.schoenfeld@uni-bayreuth.de>
Date:   Wed Nov 24 05:44:32 2021 +0100

    Initial commit

```

At this point, it is clear why you should use meaningful commit messages - how else should you know what those revisions introduced?

You can use 

```
git show 07ebd338
```

to view the details and the related commit message of a specific revision. The identifier `07ebd338` are the first few characters of the commit-id presented by the `git log` command.

If you would like to go back in time to a specific revision, simply use

```
git checkout 67321e83
```

which takes you back to the initial commit. If you now apply other changes and commit them, all commits after commit `67321e83` are discarded. This is ok to do if you're working alone on the project. In collaborative scenarios, this will clash with what your collaborators have locally.

In case you screwed up really hard, git might still be able to save you. There are ways of "Redoing the undo" - [have a look here at the gitlab doc](https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git/#redoing-the-undo)^[[https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git/#redoing-the-undo](https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git/#redoing-the-undo)].

## Viewing differences between revisions

From the `git log` output, you can compare revisions and see what changed:

```
git diff 67321e83
```

This compares the current revision with revision `67321e83`. The output shows that one file was added and some changes were made to the file `tutorial.md`:

```
diff --git a/git_commit.png b/git_commit.png
new file mode 100644
index 0000000..9eefc8d
Binary files /dev/null and b/git_commit.png differ
diff --git a/tutorial.md b/tutorial.md
index 12c32aa..b2858a8 100644
--- a/tutorial.md
+++ b/tutorial.md
@@ -42,3 +42,8 @@ git commit
 
 This will prompt you for a commit message. Make sure to describe your commits in a meaningful way (relevant: https://xkcd.com/1296/ ).
 
+
+## Making changes
+
+
+ ....
```

To compare specific revisions, simply use

```
git diff 62d0f432 07ebd338
```

which compares the last two commits with each other. It will show that the most recent change involved the deletion of `git_commit.png`. Using the above-mentioned commands, it would be possible to restore that file again. [In case you want to read further have a look here the gitlab doc](https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git)^[[https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git](https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git)].
