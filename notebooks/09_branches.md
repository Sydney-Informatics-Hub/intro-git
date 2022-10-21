# Branches

<div class="questions">

### Questions

- What is a Git branch?
- Why are branches useful?

</div>

<div class="objectives">

### Objectives

- Create an alternate version of your code on a branch.
- Understand how branches can help us break our code with confidence

</div>  


<div class="keypoints">

### Key Points

- `git checkout -b` creates a new branch.
- branches allow us to keep alternative versions of our code in a repository
- `git diff` can show us the differences betweem branches

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
while this active development is ongoing - this means that if you need to use
the code, you don't have to finish your development work first.

With the tools we've covered so far, Alice and Bob could make a note of the
most recent working commit of their script:

    34478ad Works great

This would give them the ability to retrieve the last version before they
broke it. The problem with this is that they'd need to remember the 34478ad
hash (FIXME and is going back and forward between that and HEAD a problem)

Git provides us with a better way to keep alternate versions of a single
codebase - a branch.

In this section of the workshop, we'll just create local branches [FIXME]

We've already been using a `main` branch. Let's create a new branch where
we're free to break things. 

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
and fixes her refactored script.

```
$ git checkout development
$ nano mean.py
```


We've already seen how `git diff` can be used to show the differences between 
commits on a single branch. Alice can use it to double-check what she's
changed on the `development` branch:

```bash
$ git diff main
```

```output
FIXME
```
This output shows what changes would need to be made to the `main` branch to 
make them the same as Alice's `development` branch. So lines which Alice has
added in `development` would have to be inserted, and are marked with a `+`
sign.

