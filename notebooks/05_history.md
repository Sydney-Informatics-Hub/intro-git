# Exploring History

<div class="questions">

### Questions

- How can I identify old versions of files?
- How do I review my changes?
- How can I recover old versions of files?

</div>

<div class="objectives">

### Objectives

- Explain what the HEAD of a repository is and how to use it.
- Identify and use Git commit numbers.
- Compare various versions of tracked files.
- Restore old versions of files.
</div>  

As we saw in the previous episode, we can refer to commits by their
identifiers.  You can refer to the _most recent commit_ of the working
directory by using the identifier `HEAD`.

We've been adding one line at a time to `mean.py`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.  Before we start,
let's make a change to `mean.py`, adding yet another line.

```sh
nano mean.py
cat mean.py
```

```python
import pandas as pd
dataframe = pd.read_csv("input.csv")
means = dataframe.mean()
# an ill-considered comment
```

Now, let's see what we get.

```sh
git diff HEAD mean.py
```

```abc
diff --git a/mean.py b/mean.py
index 67d0b5b..c9869a3 100644
--- a/mean.py
+++ b/mean.py
@@ -1,3 +1,4 @@
 import pandas as pd
 dataframe = pd.read_csv("input.csv")
 means = dataframe.mean()
+# an ill-considered change
```

This is the same as what you would get if you leave out `HEAD` (try it).  The
real goodness in all this is when you can refer to previous commits.  We do
that by adding `~1`  (where "~" is "tilde", pronounced [**til**-d*uh*]) 
to refer to the commit one before `HEAD`.

```sh
git diff HEAD~1 mean.py
```

If we want to see the differences between older commits we can use `git diff`
again, but with the notation `HEAD~1`, `HEAD~2`, and so on, to refer to them:


```sh
git diff HEAD~2 mean.py
```

```abc
diff --git a/mean.py b/mean.py
index ffd919b..c9869a3 100644
--- a/mean.py
+++ b/mean.py
@@ -1 +1,4 @@
 import pandas as pd
+dataframe = pd.read_csv("input.csv")
+means = dataframe.mean()
+# an ill-considered change
```

We could also use `git show` which shows us what changes we made at an older commit as 
well as the commit message, rather than the _differences_ between a commit and our 
working directory that we see by using `git diff`.

```sh
git show HEAD~2 mean.py
```

```abc
commit 3c865ca8570879e5ae8bbf3253283bf33d89bd14
Author: Mike Lynch <m.lynch@sydney.edu.au>
Date:   Mon Oct 24 09:56:51 2022 +1100

    Start a script to calculate the mean

diff --git a/mean.py b/mean.py
new file mode 100644
index 0000000..ffd919b
--- /dev/null
+++ b/mean.py
@@ -0,0 +1 @@
+import pandas as pd
```

In this way, we can build up a chain of commits. The most recent end of the
chain is referred to as `HEAD`; we can refer to previous commits using the `~`
notation, so `HEAD~1` means "the previous commit", while `HEAD~123` goes back
123 commits from where we are now.

We can also refer to commits using those long strings of digits and letters that
`git log` displays. These are unique IDs for the changes, and "unique" really
does mean unique: every change to any set of files on any computer has a unique
40-character identifier. Our first commit was given the ID
`3c865ca8570879e5ae8bbf3253283bf33d89bd14` so let's try this:

```sh
git diff 3c865ca8570879e5ae8bbf3253283bf33d89bd14 mean.py
```

```abc
diff --git a/mean.py b/mean.py
index ffd919b..1da11d6 100644
--- a/mean.py
+++ b/mean.py
@@ -1 +1,4 @@
 import pandas as pd
+dataframe = pd.read_csv("input.csv")
+means = dataframe.mean()
+# an ill-considered change
```

That's the right answer, but typing out random 40-character strings is annoying,
so Git lets us use just the first few characters (typically seven for normal
size projects):

```sh
git diff 3c865ca mean.py
```

```abc
diff --git a/mean.py b/mean.py
index ffd919b..1da11d6 100644
--- a/mean.py
+++ b/mean.py
@@ -1 +1,4 @@
 import pandas as pd
+dataframe = pd.read_csv("input.csv")
+means = dataframe.mean()
+# an ill-considered change
```

All right! So we can save changes to files and see what we've changed. Now, how
can we restore older versions of things? Let's suppose we change our mind about
the last update to `mean.py` (the "ill-considered change").

`git status` now tells us that the file has been changed,
but those changes haven't been staged:

```sh
git status
```

```abc
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   mean.py

no changes added to commit (use "git add" and/or "git commit -a")
```

We can put things back the way they were
by using `git checkout`:

```sh
git checkout HEAD mean.py
```

```abc
Updated 1 path from 37beb0d
```

```sh
cat mean.py
```

```abc
import pandas as pd
dataframe = pd.read_csv("input.csv")
means = dataframe.mean()
```

As you might guess from its name, `git checkout` checks out (i.e., restores)
an old version of a file. In this case, we're telling Git that we want to
recover the version of the file recorded in `HEAD`, which is the last saved
commit. If we want to go back even further, we can use a commit identifier
instead:

```sh
git checkout 3c865ca mean.py
```

```abc
Updated 1 path from e79e40d
```

```sh
cat mean.py
```

```abc
import pandas as pd
```

```sh
git status
```

```abc
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   mean.py

```

Notice that the changes are currently in the staging area.
Again, we can put things back the way they were
by using `git checkout`:

```sh
git checkout HEAD mean.py
```

:::{.callout-important}
## Don't Lose Your HEAD

Above we used

```sh
git checkout 3c865ca mean.py
```

to revert `mean.py` to its state after the commit `3c865ca`. But be careful! 
The command `checkout` has other important functionalities and Git will misunderstand
your intentions if you are not accurate with the typing. For example, 
if you forget `mean.py` in the previous command:

```sh
git checkout 3c865ca
```

```abc
Note: switching to '3c865ca'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 3c865ca Start a script to calculate the mean
```

This might be one of the most alarming messages for git beginners: what it
means is that HEAD, which normally points to the most recent commit of the
current branch, has now been changed to point to a specific commit in your
history. As the warning indicates, its main use is to be able to inspect an
earlier commit.

To get things back to normal, reattach your `HEAD` with `git checkout main`.
:::


It's important to remember that we must use the commit number that identifies
the state of the repository *before* the change we're trying to undo. A common
mistake is to use the number of the commit in which we made the change we're
trying to discard. In the example below, we want to retrieve the state from
before the most recent commit (`HEAD~1`), which is commit `f22b25e`:

![Git Checkout](../fig/git-checkout.svg)

So, to put it all together, here's how Git works in cartoon form:

![https://figshare.com/articles/How_Git_works_a_cartoon/1328266](../fig/git_staging.svg)

:::{.callout-note}
## Simplifying the Common Case

If you read the output of `git status` carefully, you'll see that it includes this hint:


```abc
(use "git checkout -- <file>..." to discard changes in working directory)
```

`git checkout` without a version identifier restores files to the state saved 
in `HEAD`. The double dash `--` is needed to separate the names of the files
being recovered from the command itself: without it, Git would try to use the
name of the file as the commit identifier.
:::

The fact that files can be reverted one by one tends to change the way people
organize their work. If everything is in one large document, it's hard (but not
impossible) to undo changes to the introduction without also undoing changes
made later to the conclusion. If the introduction and conclusion are stored in
separate files, on the other hand, moving backward and forward in time becomes
much easier.

<div class="challenge">

### Challenge: Recovering Older Versions of a File

Alice has made changes to the Python script that she has been working on for
weeks, and the modifications she made this morning "broke" the script and it no
longer runs. She has spent ~ 1hr trying to fix it, with no luck...

Luckily, she has been keeping track of her project's versions using Git! Which
commands below will let her recover the last committed version of her Python
script called `data_cruncher.py`?


1. `git checkout HEAD`
2. `git checkout HEAD data_cruncher.py`
3. `git checkout HEAD~1 data_cruncher.py`
4. `git checkout <unique ID of last commit> data_cruncher.py`
5. Both 2 and 4

<details>
<summary>Solution</summary>

The answer is `5. Both 2 and 4`.

The `checkout` command restores files from the repository, overwriting the files
in your working directory. Answers 2 and 4 both restore the *latest*
version *in the repository* of the file `data_cruncher.py`. Answer 2 uses
`HEAD` to indicate the *latest*, whereas answer 4 uses the unique ID of the last
commit, which is what `HEAD` means. 
 
Answer 3 gets the version of `data_cruncher.py` from the commit *before* `HEAD`,
which is NOT what we wanted.

Answer 1 can be dangerous! Without a filename, `git checkout` will restore **all
files** in the current directory (and all directories below it) to their state
at the commit specified. This command will restore `data_cruncher.py` to the
latest commit version, but it will also restore *any other files that are
changed* to that version, erasing any changes you may have made to those files!
As discussed above, you are left in a *detached* `HEAD` state, and you don't
want to be there.
</details>
</div>  

<div class="challenge">

### Challenge: Reverting a Commit

Alice is collaborating with colleagues on her Python script.  She
realizes her last commit to the project's repository contained an error, and 
wants to undo it.  Alice wants to undo correctly so everyone in the project's
repository gets the correct change. The command `git revert [erroneous commit ID]` will create a  new commit that reverses the erroneous commit.  
    
The command `git revert` is different from `git checkout [commit ID]` because `git checkout` returns the files not yet committed within the local repository to a previous state, whereas `git revert` reverses changes committed to the local and project repositories.   
   
Below are the right steps and explanations for Alice to use `git revert`,
what is the missing command?  

1. `________ # Look at the git history of the project to find the commit ID`
2. Copy the ID (the first few characters of the ID, e.g. 0b1d055).
3. `git revert [commit ID]`
4. Type in the new commit message.
5. Save and close

<details>
<summary>Solution</summary>

The command `git log` lists project history with commit IDs.  
 
The command `git show HEAD` shows changes made at the latest commit, and lists
the commit ID; however, Alice should double-check that it is the correct commit,
and no one else has committed changes to the repository.

</details>
</div>  

<div class="challenge">

### Challenge: Understanding Workflow and History

What is the output of the last command in

```sh
cd means
echo "This is a script to calculate means" > doc.txt
git add doc.txt
echo "It is a work in progress" >> doc.txt
git commit -m "Started some documentation"
git checkout HEAD doc.txt
cat doc.txt
```

1. ```abc
   It is a work in progress
   ```
2. ```abc
   This is a script to calculate means
   ```
3. ```abc
   This is a script to calculate means
   It is a work in progress
   ```
4. ```abc
  Error because you have changed doc.txt without committing the changes
  ```

<details>
<summary>Solution</summary>

The answer is 2. 
 
The command `git add doc.txt` places the current version of `doc.txt` into the
staging area. The changes to the file from the second `echo` command are only
applied to the working copy, not the version in the staging area.
 
So, when `git commit -m "Started some documentation"` is executed, the version
of `doc.txt` committed to the repository is the one from the staging area and
has only one line.
  
At this time, the working copy still has the second line (and  `git status` will
show that the file is modified). However, `git checkout HEAD doc.txt` replaces
the working copy with the most recently committed version of `doc.txt`.
  
So, `cat doc.txt` will output 

```abc
This is a script to calculate means
```

</details>
</div>  

<div class="challenge">

### Challenge: Checking Understanding of `git diff`

1. Consider this command: `git diff HEAD~9 mean.py`. What do you predict this command
will do if you execute it? What happens when you do execute it? Why?

2. Try another command, `git diff [ID] mean.py`, where [ID] is replaced with
the unique identifier for your most recent commit. What do you think will happen,
and what does happen?

<details>
<summary>Solution</summary>

TODO FIXME: this needs a solution!

</details>
</div>  


## Getting Rid of Staged Changes

`git checkout` can be used to restore a previous commit when unstaged changes have
been made, but will it also work for changes that have been staged but not committed?
Make a change to `mean.py`, add that change, and use `git checkout` to see if
you can remove your change.


## Explore and Summarize Histories

Exploring history is an important part of Git, and often it is a challenge to find
the right commit ID, especially if the commit is from several months ago.

Imagine the `means` project has more than 50 files.
You would like to find a commit that modifies some specific text in `mean.py`.
When you type `git log`, a very long list appeared.
How can you narrow down the search?

Recall that the `git diff` command allows us to explore one specific file,
e.g., `git diff mean.py`. We can apply a similar idea here.

```sh
git log mean.py
```
Unfortunately some of these commit messages are very ambiguous, e.g., `update files`.
How can you search through these files?

Both `git diff` and `git log` are very useful and they summarize a different part of the history for you.

Is it possible to combine both? Let's try the following:

```sh
git log --patch mean.py
```
You should get a long list of output, and you should be able to see both commit messages and 
the difference between each commit.

Question: What does the following command do?

```sh
git log --patch HEAD~9 *.txt
```



<div class="keypoints">

### Key Points

- `git diff` displays differences between commits.
- `git checkout` recovers old versions of files.
</div>  
