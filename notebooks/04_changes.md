# Tracking Changes

<div class="questions">

### Questions

- How do I record changes in Git?
- How do I check the status of my version control repository?
- How do I record notes about what changes I made and why?

</div>

<div class="objectives">

### Objectives

- Go through the modify-add-commit cycle for one or more files.
- Explain where information is stored at each stage of that cycle.
- Distinguish between descriptive and non-descriptive commit messages.
</div>  


<div class="keypoints">

### Key Points

- `git status` shows the status of a repository.
- Files can be stored in a project's working directory (which users see), the staging area (where the next commit is being built up) and the local repository (where commits are permanently recorded).
- `git add` puts files in the staging area.
- `git commit` saves the staged content as a new commit in the local repository.
- Write a commit message that accurately describes your changes.

</div>  


First let's make sure we're still in the right directory. 
You should be in the `mean` directory.

```bash
$ cd ~/Desktop/mean
```

We'll create a file called `mean.py`. 

This will be a very simple Python script which uses Pandas, a popular Python
library for working with data, to calculate the mean of a set of values in
a CSV script. If you're new to Python or you don't have it set up on your
laptop, don't worry - in this workshop we're just using it as a text file
to illustrate how Git managescode.

We'll use `nano` to edit the file; you can use whatever editor you like.
In particular, this does not have to be the `core.editor` you set globally earlier. But remember, the bash command to create or edit a new file will depend on the editor you choose (it might not be `nano`). For a refresher on text editors, check out ["Which Editor?"](https://swcarpentry.github.io/shell-novice/03-create/) in [The Unix Shell](https://swcarpentry.github.io/shell-novice/) lesson.

```bash
$ nano mean.py
```

Type the text below into the `mean.py` file:

```python
import pandas as pd
```

Let's first verify that the file was properly created by running the
list command (`ls`):


```bash
$ ls
```

```output
mean.py
```

`mean.py` contains a single line, which we can see by running:

```bash
$ cat mean.py
```

```output
import pandas as pd
```

If we check the status of our project again, Git tells us that it's noticed
the new file:

```bash
$ git status
```

```output
On branch main

No commits yet

Untracked files:
   (use "git add <file>..." to include in what will be committed)

	mean.py

nothing added to commit but untracked files present (use "git add" to track)
```

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of. We can tell Git to track a file using `git add`:

```
$ git add mean.py
```

and then check that the right thing happened:

```bash
$ git status
```

```output
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   mean.py

```

Git now knows that it's supposed to keep track of `mean.py`,
but it hasn't recorded these changes as a commit yet.
To get it to do that, we need to run one more command:

```bash
$ git commit -m "Start a script to calculate the mean"
```

```output
[main (root-commit) b03ceb6] Start a script to calculate the mean
 1 file changed, 1 insertion(+)
 create mode 100644 mean.py
```

When we run `git commit`, Git takes everything we have told it to save by
using `git add` and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a [commit]({{ page.root }}{% link reference.md %}#commit)
(or [revision]({{ page.root }}{% link reference.md %}#revision)) and its short identifier is `b03ceb6`. Your commit will likely have a different identifier.

We use the `-m` flag (for "message")
to record a short, descriptive, and specific comment that will help us remember later on what we did and why.
If we just run `git commit` without the `-m` option,
Git will launch `nano` (or whatever other editor we configured as `core.editor`)
so that we can write a longer message.

[Good commit messages][commit-messages] start with a brief (<50 characters) statement about the
changes made in the commit. Generally, the message should complete the sentence "If applied, this commit will" <commit message here>.
If you want to go into more detail, add a blank line between the summary line and your additional notes. Use this additional space to explain why you made changes and/or what their impact will be.

If we run `git status` now:

```bash
$ git status
```

```output
On branch main
nothing to commit, working directory clean
```

it tells us everything is up to date. If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`:

```bash
$ git log
```

```output
commit b03ceb64040ce8347c8f9dd1530088e0621b31f9 (HEAD -> main)
Author: Mike Lynch <m.lynch@sydney.edu.au>
Date:   Wed Oct 12 09:58:50 2022 +1100

    Start a script to calculate the mean
```

`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes the commit's full identifier
(which starts with the same characters as the short identifier printed by
the `git commit` command earlier), the commit's author, when it was created,
and the log message Git was given when the commit was created.

:::{.callout-note}
The default way Git outputs this has changed recently. Older versions
of git would print the log straight to the command line. If you installed 
git recently, it will send the output through a program called a pager, which
displays it as if it were in a little read-only editor. This is useful when
there's a lot of information in the history.

Unfortunately, it's a bit opaque for people who aren't 100% comfortable with
Unix command-line tools.
*   To get out of the pager, press <kbd>q</kbd>.
*   To move to the next page, press <kbd>Spacebar</kbd>.
*   To search for `some_word` in all pages, press <kbd>/</kbd> and type `some_word`. Navigate through matches pressing <kbd>N</kbd>.

You can configure how Git uses pagers for displaying logs and other sorts of
information.
:::

:::{.callout-note}
## Where Are My Changes?

If we run `ls` at this point, we will still see just one file called `mean.py`.
That's because Git saves information about files' history in the special `.git`
directory mentioned earlierso that our filesystem doesn't become cluttered
(and so that we can't accidentally edit or delete an old version).
:::

Now suppose Alice adds more code to her script. (Again, we'll edit with
`nano` and then `cat` the file to show its contents; you may use a different
editor, and don't need to `cat`.)

```bash
$ nano mean.py
$ cat mean.py
```

```python
import pandas as pd
dataframe = pd.read_csv("rgb.csv")
```

When we run `git status` now,
it tells us that a file it already knows about has been modified:

```bash
$ git status
```

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   mean.py

no changes added to commit (use "git add" and/or "git commit -a")
```

The last line is the key phrase: "no changes added to commit". We have changed
this file, but we haven't told Git we will want to save those changes (which
we do with `git add`) nor have we saved them (which we do with `git commit`).
So let's do that now. It is good practice to always review our changes before
saving them. We do this using `git diff`. This shows us the differences between
the current state of the file and the most recently saved version:

```bash
$ git diff
```

```output
diff --git a/mean.py b/mean.py
index ffd919b..51d2079 100644
--- a/mean.py
+++ b/mean.py
@@ -1 +1,2 @@
 import pandas as pd
+dataframe = pd.read_csv("rgb.csv")
```

Note that, as with `git log`, your installation of Git may display this 
information in a pager rather than writing it straight to the command line.

The output of `git diff` is cryptic because it is actually a series of commands
for tools like editors and `patch` telling them how to reconstruct one file
given the other. If we break it down into pieces:

1.  The first line tells us that Git is producing output similar to the Unix `diff` command
    comparing the old and new versions of the file.
2.  The second line tells exactly which versions of the file
    Git is comparing;
    `ffd919b` and `51d2079` are unique computer-generated labels for those versions.
    100644 is the permissions on the file - who is allowed to read, write or run
    it
3.  The third and fourth lines once again show the name of the file being changed.
4.  The remaining lines are the most interesting, they show us the actual differences
    and the lines on which they occur.
    In particular,
    the `+` marker in the first column shows where we added a line.

After reviewing our change, it's time to commit it:

```bash
$ git commit -m "Add a line which loads the data from a CSV file"
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   mean.py

no changes added to commit (use "git add" and/or "git commit -a")
```

Whoops: Git won't commit because we didn't use `git add` first.
Let's fix that:

```bash
$ git add mean.py
$ git commit -m "Add a line which loads the data from a CSV file"
```

```output
[main cfccbd9] Add a line which loads the data from a CSV file
 1 file changed, 1 insertion(+)
```

Git insists that we add files to the set we want to commit
before actually committing anything. This allows us to commit our
changes in stages and capture changes in logical portions rather than
only large batches.

For example, suppose we're adding a few citations to relevant research to our
thesis. We might want to commit those additions, and the corresponding
bibliography entries, but *not* commit some of our work drafting the
conclusion (which we haven't finished yet).

To allow for this, Git has a special *staging area* where it keeps track of
things that have been added to the current [changeset]({{ page.root }}{% link
reference.md %}#changeset) but not yet committed.

:::{.callout-note}
## Staging Area

If you think of Git as taking snapshots of changes over the life of a project,
`git add` specifies *what* will go in a snapshot (putting things in the staging
area), and `git commit` then *actually takes* the snapshot, and makes a
permanent record of it (as a commit). If you don't have anything staged when
you type `git commit`, Git will prompt you to use `git commit -a` or `git
commit --all`, which is kind of like gathering *everyone* to take a group
photo! However, it's almost always better to explicitly add things to the
staging area, because you might commit changes you forgot you made. (Going back
to the group photo simile, you might get an extra with incomplete makeup
walking on the stage for the picture because you used `-a`!) Try to stage
things manually, or you might find yourself searching for "git undo commit"
more than you would like!
:::

![The Git Staging Area](../fig/git-staging-area.svg)

Let's watch as our changes to a file move from our editor to the staging area
and into long-term storage. First, we'll add another line to the file:

```bash
$ nano mean.py
$ cat mean.py
```

```output
import pandas as pd
dataframe = pd.read_csv("rgb.csv")
means = dataframe.mean()
```

```bash
$ git diff
```

```output
diff --git a/mean.py b/mean.py
index 51d2079..a6abcee 100644
--- a/mean.py
+++ b/mean.py
@@ -1,2 +1,3 @@
 import pandas as pd
 dataframe = pd.read_csv("input.csv")
+means = dataframe.mean()
```

So far, so good: we've added one line to the end of the file (shown with a `+`
in the first column). Now let's put that change in the staging area
and see what `git diff` reports:

```bash
$ git add mean.py
$ git diff
```

There is no output: as far as Git can tell, there's no difference between what
it's been asked to save permanently and what's currently in the directory.
However, if we do this:

```bash
$ git diff --staged
```

```output
diff --git a/mean.py b/mean.py
index 51d2079..a6abcee 100644
--- a/mean.py
+++ b/mean.py
@@ -1,2 +1,3 @@
 import pandas as pd
 dataframe = pd.read_csv("input.csv")
+means = dataframe.mean()
```

it shows us the difference between the last committed change and what's in the
staging area.

Let's save our changes:

```bash
$ git commit -m "Calculates the means"
```

```output
[main 99dd636] Calculates the means
 1 file changed, 1 insertion(+)
```

check our status:

```bash
$ git status
```

```output
On branch main
nothing to commit, working directory clean
```

and look at the history of what we've done so far:

```bash
$ git log
```

```output
commit 99dd6362bf798f4c145be76849a638eef083d59d (HEAD -> main)
Author: Mike Lynch <m.lynch@sydney.edu.au>
Date:   Wed Oct 12 11:53:17 2022 +1100

    Calculates the means

commit cfccbd9b68ab5080ef70da1a69dfa644f97744af
Author: Mike Lynch <m.lynch@sydney.edu.au>
Date:   Wed Oct 12 10:19:41 2022 +1100

    Add a line which loads the data from a CSV file

commit b03ceb64040ce8347c8f9dd1530088e0621b31f9
Author: Mike Lynch <m.lynch@sydney.edu.au>
Date:   Wed Oct 12 09:58:50 2022 +1100

    Start a script to calculate the mean
```
:::{.callout-note}
## Word-based diffing

Sometimes, e.g. in the case of the text documents a line-wise
diff is too coarse. That is where the `--color-words` option of
`git diff` comes in very useful as it highlights the changed
words using colors.
:::

## Limit Log Size

To avoid having `git log` cover your entire terminal screen, you can limit the
number of commits that Git lists by using `-N`, where `N` is the number of
commits that you want to view. For example, if you only want information from
the last commit you can use:

```bash
$ git log -1
```

```output
commit 99dd6362bf798f4c145be76849a638eef083d59d (HEAD -> main)
Author: Mike Lynch <m.lynch@sydney.edu.au>
Date:   Wed Oct 12 11:53:17 2022 +1100

    Calculates the means
```

You can also reduce the quantity of information using the `--oneline` option:


```bash
$ git log --oneline
```

```output
99dd636 (HEAD -> main) Calculates the means
cfccbd9 Add a line which loads the data from a CSV file
b03ceb6 Start a script to calculate the mean
```

You can also combine the `--oneline` option with others. One useful
combination adds `--graph` to display the commit history as a text-based
graph and to indicate which commits are associated with the
current `HEAD`, the current branch `main`, or
[other Git references][git-references]:

[FIXME - maybe move this to the branching lesson where it's more relevant]


```bash
$ git log --oneline --graph
```

```output
* 99dd636 (HEAD -> main) Calculates the means
* cfccbd9 Add a line which loads the data from a CSV file
* b03ceb6 Start a script to calculate the mean
```
:::

```{.callout-important}
## Directories

Two important facts you should know about directories in Git.

1. Git does not track directories on their own, only files within them.

   Try it for yourself:

   ```bash
   $ mkdir extras
   $ git status
   $ git add extras
   $ git status
   ```

   ```output
   On branch main
   nothing to commit, working tree clean
   ```

   Note, our newly created empty directory `extras` does not appear in
   the list of untracked files even if we explicitly add it (_via_ `git add`) to our
   repository. This is the reason why you will sometimes see `.gitkeep` files
   in otherwise empty directories. Unlike `.gitignore`, these files are not special
   and their sole purpose is to populate a directory so that Git adds it to
   the repository. In fact, you can name such files anything you like.

2. If you create a directory in your Git repository and populate it with files,
   you can add all files in the directory at once by:

   ```bash
   git add <directory-with-files>
   ```
   
   Try it for yourself:

   ```bash
   touch extras/file1 extras/file2
   git status
   git add extras
   git status
   ```

   Before moving on, we will commit these changes.

   ```bash
   $ git commit -m "Add some empty files"
   ```
:::

To recap, when we want to add changes to our repository,
we first need to add the changed files to the staging area
(`git add`) and then commit the staged changes to the
repository (`git commit`):

![The Git Commit Workflow](../fig/git-committing.svg)

:::{.callout-challenge}

## Choosing a Commit Message

Which of the following commit messages would be most appropriate for the
last commit made to `mean.py`?

1. "Changes"
2. "Added line 'means = dataframe.mean()' to mean.py"
3. "Script now calculates mean values"

## Solution

Answer 1 is not descriptive enough, and the purpose of the commit is unclear;
and answer 2 is redundant to using "git diff" to see what changed in this commit;
but answer 3 is good: short, descriptive, and imperative.
:::

:::{.callout-challenge}
## Committing Changes to Git

Which command(s) below would save the changes of `myfile.txt` to my local Git
repository?
1. ```bash
   $ git commit -m "my recent changes"
   ```
2. ```bash
   $ git init myfile.txt
   $ git commit -m "my recent changes"
   ```
3. ```bash
   $ git add myfile.txt
   $ git commit -m "my recent changes"
   ```
4. ```bash
   $ git commit -m myfile.txt "my recent changes"
   ```
:::{.callout-solution}
## Solution

1. Would only create a commit if files have already been staged.
2. Would try to create a new repository.
3. Is correct: first add the file to the staging area, then commit.
4. Would try to commit a file "my recent changes" with the message myfile.txt.
:::
:::

:::{.callout-challenge}
## Committing Multiple Files

The staging area can hold changes from any number of files that you want to
commit as a single snapshot.

1. Add a comment to `mean.py` (in Python, comments are lines starting with `#`)
2. Create a new file `README.md` with a line describing the script
3. Add changes from both files to the staging area, and commit those changes.

:::{.callout-solution}
## Solution

The output below from `cat mean.py` reflects only content added during 
this exercise. Your output may vary.

First we make our changes to the `mean.py` and `README.md` files:

```bash
$ nano mean.py
$ cat mean.py
```

```output
# Script to calculate the mean
import pandas as pd
dataframe = pd.read_csv("input.csv")
means = dataframe.mean()
```
```
$ nano README.md
$ cat README.md
```

```output
A script to calculate the mean of values in a CSV
```

Now you can add both files to the staging area. We can do that in one line:

```bash
$ git add mean.py README.md
```

Or with multiple commands:

```bash
$ git add mean.py
$ git add README.md
```
Now the files are ready to commit. You can check that using `git status`. If you are ready to commit use:

```bash
$ git commit -m "Added some documentation"
```

```output
[main b68244c] Added some documentation
 2 files changed, 2 insertions(+)
 create mode 100644 README.md
```
:::
:::

:::{.callout-challenge}
## `bio` Repository

* Create a new Git repository on your computer called `bio`.
* Write a three-line biography for yourself in a file called `me.txt`, commit your changes
* Modify one line, add a fourth line
* Display the differences between its updated state and its original state.

:::{.callout-solution}
## Solution

If needed, move out of the `means` folder:

```bask
$ cd ..
```

Create a new folder called `bio` and 'move' into it:

```bash
$ mkdir bio
$ cd bio
```

Initialise git:

```bash
$ git init
```

Create your biography file `me.txt` using `nano` or another text editor.
Once in place, add and commit it to the repository:

```bash
$ git add me.txt
$ git commit -m "Add biography file" 
```

Modify the file as described (modify one line, add a fourth line).
To display the differences
between its updated state and its original state, use `git diff`:

```bash
$ git diff me.txt
```

:::
:::

[commit-messages]: https://chris.beams.io/posts/git-commit/
[git-references]: https://git-scm.com/book/en/v2/Git-Internals-Git-References

{% include links.md %}
