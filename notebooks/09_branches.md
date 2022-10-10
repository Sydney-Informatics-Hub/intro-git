# Branches

<div class="questions">

### Questions

- What are branches, and why are they useful?

</div>

<div class="objectives">

### Objectives

- Create an alternate version of your code on a branch.
- Understand how branches can help manage software development.

</div>  


<div class="keypoints">

### Key Points

- `git checkout -b branch` creates a branch.
- `git merge BRANCH` merges changes from one branch to another

</div>


Part of software development is breaking things. You have code that works, 
but you need to add a new feature or fix a bug. Sometimes, these fixes are
small and self-contained, but in many cases, they require the equivalent of
putting the codebase up on some blocks, getting underneath it with a spanner
and pulling all the bits out.

While you are in this active development stage, your code will be broken.
You still want to be able to commit small changes while you're working on it.
The problem is, each of these incremental commits will still be, in some sense,
broken code. They might be passing more tests, but the whole thing is still
not ready.

It's good to be able to get a copy of the last working version of the code
while this active development is ongoing. You may need to deploy it somewhere,
or you may just need to use it.

With the tools we've covered so far, Alice and Bob could make a note of the
most recent working commit of their script:

    34478ad Works great

This would give them the ability to retrieve the last version before they
broke it. The problem with this is that they'd need to remember the 34478ad
hash (FIXME and is going back and forward between that and HEAD a problem)

Git provides us with a better way to keep alternate versions of a single
codebase - a branch

We've already been using a `main` branch. Let's create a new branch where
we're free to break things

```bash
$ git checkout -b development
```

```output
Switched to a new branch 'development'
```

Notice that the command to create a new branch is the same as the command we've
been using to checkout different points on our repository's commit history -
what makes it create a new branch is the `-b` flag, followed by our new branch
name

Our new branch is a copy of the last state of the branch we were on when we
created it - in this case, `main`.

We can see all the branches in our local repo if we use the `git branch`
command: this also indicates which branch we currently have checked out using
an asterisk.

```bash
$ git branch
```

```output
* develop
  main
```

Alice wants to reorganise her script, so she makes a change:

```bash
$ nano mean.py
```

And commits the changes

```bash
$ git add mean.py
$ git commit
```

Now Alice's development branch doesn't work - which is not unusual. If Bob
asks her to use the script to compute the mean of some values, she can switch
her repository back to the `main` branch, which is still working

```bash
$ git checkout main
```

We can check that the content of `mean.py` is as we left it:

```bash
$ cat mean.py
```

```output
FIXME
```

After calculating some means for Bob, Alice returns to her development branch,
and fixes her refactored script

```
$ git checkout development
$ nano mean.py
```

If Alice is happy with her refactored script, she'll want to bring the changes
back to the main branch. 

First, Alice has to commit her changes to `develop`:

```bash
$ git add mean.py
$ git commit mean.py
```

Usually, when bringing changes from one branch to another, we'll start on the 
branch which we want to change - in this case we're bringing changes from
`development` to `main`, so we'll checkout main

```bash
$ git checkout main
```

```output
Switched to branch 'main'.
```
We've already seen how `git diff` can be used to show the differences between 
commits on a single branch. Alice can use it to double-check what she's
changed on the `development` branch:

```bash
$ git diff development
```

```output
FIXME
```
Notice that the lines which Alice has added in `development` have '-' signs
next to them, which can be a bit counterintutive. `git diff` is returning what
changes would have to be made to change `development` into `main`, so it's
inverted from what you might expect: it shows new code in `development` which
would have to be removed, and old code which is not in `development` which 
would have to be reinstated.

Once Alice is happy with her changes, she can use `git merge` to bring the
changes across from `development` to `main`

```bash
$ git merge development
```

```output
Updating deadbea..934893
Fast-forward
 mean.py | 1+
 1 file changed, 1 insertion(+)
```
A single merge can bring across changes in multiple files: after the merge, 
git provides us with some feedback on what files were changed by the merge, and
how extensive the changes were.

