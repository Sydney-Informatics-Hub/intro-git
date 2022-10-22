# Merging

<div class="questions">

### Questions

- How does Git help to merge changes on two branches?

</div>

<div class="objectives">

### Objectives

- Understand what fast-forwarding means
- Understand how git tries to merge two branches with divergent histories

</div>  


<div class="keypoints">

### Key Points

- `git merge BRANCH` merges changes from one branch to another
- Git will do its best to figure out how to combine changes to both branches
- Git can't guarantee that merged code still works

</div>

If Alice is happy with the changes she's made to her script, she'll want to
bring the changes back to the main branch. 

First, Alice has to commit her changes to `development`:

```bash
$ git add mean.py
$ git commit mean.py
```

Usually, when bringing changes from one branch to another, we'll start on the 
branch which we want to change - in this case we're bringing changes from
`development` to `main`, so we'll checkout main:

```bash
$ git checkout main
```

```abc
Switched to branch 'main'.
```

To bring changes from the `development` branch to `main`, Alice uses the
`git merge` command, specifying which branch she wants to merge:

```bash
$ git merge development
```

```abc
Updating deadbea..934893
Fast-forward
 mean.py | 1+
 1 file changed, 1 insertion(+)
```

Note that a single merge can make changes to multiple files: after the merge, 
git provides us with some feedback on what files were changed by the merge, and
how extensive the changes were. In this case, we've only made changes to our
script.

Git has also given us some information about how it performed the merge: this
is the two hashes at the top in 'Updating hash1 .. hash2' and the message
'Fast-forward'.

To understand what this means, we need to explore what a git repository looks
like as a data structure

# Git commits as a data structure

The commits which make up the history of a Git repository with multiple
branches form a mathematical structure called a directed acyclic graph.

Each commit is a node in the graph. The graph is directed because every edge
links a commit to one or more parents, and this relationship is one-way.

And the graph is acyclic because loops aren't permitted - if you traverse the
graph following a commit's parents back to the root commit, you won't pass 
through any of the same commits twice.

[ Illustration of a DAG ]

# Fast-forwards

These are the simplest merge: where the two commits we are merging have a
common shared history. This was the case in the example we just merged.
Nothing new had been added to the `main` branch while Alice was working on
her `development` branch, so there's no need to work out how to combine the
two histories.

In these cases, Git can 'fast-forward': it simply updates the head of our
`main` branch to the commit at the tip of the `develop` branch.

[ picture here of the fast-forward scenario]

Because the merge is using commits which already exist, Git doesn't prompt
us for a commit message when it can fast-forward.

# Merges

We are now going to see what happens when we ask git to merge two branches
which have diverged: that is, where there are new commits on either branch.

In this example, 

```bash
git checkout development
nano mean.py
```

We then commit this change

```bash
git add mean.py
git commit mean.py -m "One line near the top"
```

Then, let's make a change to mean.py on the main branch

```bash
git checkout main
nano mean.py
```

We'll add another line in a different part of the script, save and commit
the change.

```bash
git add mean.py
git commit mean.py -m "One line near the bottom"
```

Our two branches now have different histories: each branch has a commit which 
doesn't exist in the other branch.

We can compare the two files across branches using `git diff`

```bash
git diff development
```

```abc
FIXME
```
Once again, because this is a diff from `development` to `main`, the changes
we've made in `main` which aren't in `development` show as inserts (+) and
the changes in `development` which aren't in `main` show as deletes (-)

We can now ask Git to merge the changes from `development` into our main branch

```bash
git merge development
```

```abc
Merge branch 'development'
# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```

When we do this, git has prompted us for a commit message: this is similar to
what we've seen earlier when we've committed a change, but in this case, git
has pre-filled the commit message with `Merge branch 'development'`

The reason git is asking for a message is because merging two branches which
have diverged will create a new commit, called a merge commit.

In our first merge, because fast-forwarding was possible, there was no need
for a merge commit.

You have the option here to add a more complicated message explaining why you're
merging, but for now we'll just accept git's default message and save:

```abc
Auto-merging mean.py
Merge made by the 'ort' strategy.
 mean.py | 2 ++
 1 file changed, 2 insertions(+)
```

This output means that git has successfully merge the two versions of mean.py.
Git reports that it's used the 'ort' strategy - historically, git has had a
number of different algorithms which it uses to try to combine commits. 'ort'
is a relatively recent addition: the details of how the different strategies
work is not something we can go into today.

If we check our log:

```bash
git log --oneline
```

We can see that this merge has created a new commit, and that the fact that
it's a merge commit is indicated in the history.

You can also see that commits from both main and development are now visible
in the log.

Let's see what the file actually looks like after the merge.

```bash
nano mean.py
```

We can see that git has added the new line from `development` at the bottom, 
and kept the new line from `main` at the top. From Git's point of view, the
merge was a success - it's managed to bring the divergent histories of this
file into one text and kept the features of both.

[ FIXME - this would be a point to introduce the idea that a merged file 
could contain bugs and that git can't check this]

TODO - some exercises?







