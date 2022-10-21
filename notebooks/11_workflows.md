# Branch-based workflows

<div class="questions">

### Questions

- How often should I create branches?
- How should I organise my branches?
- Can I delete branches safely?

</div>

<div class="objectives">

### Objectives

- Understand how a convention for naming branches can make your repository
easier to manage
- Understand how and when to delete branches without losing history

</div>  


<div class="keypoints">

### Key Points

- Creating a branch is a cheap, fast operation
- Git offers complete flexibility for naming branches
- You can pick a naming convention which best suits your needs

</div>

We've just seen an example of creating a branch to do some development work, 
and merging the changes back to the main branch.

Our repository only has a few files, but if we were working on a repository
with dozens or hundreds of files, creating a branch may seem like it's
inefficient, especially if the change we're working on only affects one or
two files.

However, git is designed for working efficiently with large codebases. When
you create a branch, internally, git isn't creating another copy of your 
entire repository. Instead, it's storing a reference to the HEAD of the 
branch which you're starting from, and saying - here's a new branch which 
starts at this commit.

[ a diagram showing how a branch starts ]

So, internally, a new branch isn't really a "copy" of anything, it's a reference
to a new set of commits. You will only add new content to the repository 
when you add some new changes as a new commit, and the only extra content which
your branch needs is the difference those changes represent.

This is why you'll see people saying things like "branches are free" or
"branches are cheap" in Git, and it's one of the differences between Git and
older version control systems, where a branch really did equal a copy of your
entire codebase.

It also gives us a guiding principal for how to branch our code. In general,
small branches are better - ideally, restricted to one feature or bugfix.

Large or busy software development teams often use a convention for naming
branches: [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)
is one popular example, although it's fallen out of fashion in favour of other
workflows which are suited to modern commercial software deployment practices.

These are likely to be overkill for the sorts of code you'll be working on,
but they have some features which are useful at a small scale.

One good convention is two-part naming strategy where the first part indicates
what kind of branch this is, and the second part gives more details:

* feature-matrix-division
* bugfix-divide-by-zero
* release-v1.2

A 'feature' branch adds a new ability or interface to the code; a bugfix is
to correct a mistake.

And release branches allow you to collect a set of features for a planned
release into a single branch where they can be tested together and then
merged back to the main branch when they're all ready to go.

