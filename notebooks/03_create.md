# Creating a Repository

<div class="questions">

### Questions

- Where does Git store information?

</div>

<div class="objectives">

### Objectives

- Create a local Git repository.
- Describe the purpose of the `.git` directory.

</div>  


<div class="keypoints">

### Key Points

- `git init` initializes a repository.
- Git stores all of its repository data in the `.git` directory.

</div>  

Once Git is configured, we can start using it.



First, let's create a new directory in the `Desktop` folder for our work and
then change the current working directory to the newly created one:

```bash
$ cd ~/Desktop
$ mkdir planets
$ cd planets
```

Then we tell Git to make `planets` a [repository]({{ page.root }}{% link reference.md %}#repository)
-- a place where Git can store versions of our files:


```bash
$ git init
```

```output
Initialized empty Git repository in /Users/alice/Desktop/planets/.git/
````

It is important to note that `git init` will create a repository that
can include subdirectories and their files---there is no need to create
separate repositories nested within the `planets` repository, whether
subdirectories are present from the beginning or added later. Also, note
that the creation of the `planets` directory and its initialization as a
repository are completely separate processes.

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

```bash
$ ls
```

But if we add the `-a` flag to show everything, we can see that Git has
created a hidden directory within `planets` called `.git`:

```bash
$ ls -a
```
{: .language-bash}

```output
.	..	.git
```

Git uses this special subdirectory to store all the information about the project, 
including the tracked files and sub-directories located within the project's directory.
If we ever delete the `.git` subdirectory, we will lose the project's history.

Next, we will change the default branch to be called `main`.
This might be the default branch depending on your settings and version
of git.
See the [setup episode]({{ page.root }}{% link notebooks/02-setup.md %}) for more information on this change.

```bash
$ git checkout -b main
```
```output
Switched to a new branch 'main'
```


We can check that everything is set up correctly
by asking Git to tell us the status of our project:

```bash
$ git status
```

```output
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

If you are using a different version of `git`, the exact
wording of the output might be slightly different.

:::.{callout-challenge}
## Places to Create Git Repositories

# FIXME

Along with tracking information about our base project (the project we have
already created), 
Alice would also like to track information about moons.
Despite Bob's concerns, Alice creates a `moons` project inside her `planets` 
project with the following sequence of commands:

```bash
$ cd ~/Desktop   # return to Desktop directory
$ cd planets     # go into planets directory, which is already a Git repository
$ ls -a          # ensure the .git subdirectory is still present in the planets directory
$ mkdir moons    # make a subdirectory planets/moons
$ cd moons       # go into moons subdirectory
$ git init       # make the moons subdirectory a Git repository
$ ls -a          # ensure the .git subdirectory is present indicating we have created a new Git repository
```
Is the `git init` command, run inside the `moons` subdirectory, required for 
tracking files stored in the `moons` subdirectory?
 
:::{.callout-solution}

## Solution

No. Alice does not need to make the `moons` subdirectory a Git repository 
because the `planets` repository can track any files, sub-directories, and 
subdirectory files under the `planets` directory.  Thus, in order to track 
all information about moons, Alice only needed to add the `moons` subdirectory
to the `planets` directory.
 
Additionally, Git repositories can interfere with each other if they are "nested":
the outer repository will try to version-control
the inner repository. Therefore, it's best to create each new Git
repository in a separate directory. To be sure that there is no conflicting
repository in the directory, check the output of `git status`. If it looks
like the following, you are good to go to create a new repository as shown
above:

```bash
$ git status
```

```output
fatal: Not a git repository (or any of the parent directories): .git
```
:::

:::

:::{.callout-challenge}

## Correcting `git init` Mistakes

Bob explains to Alice that a nested repository is redundant and may cause confusion
down the road. Alice would like to remove the nested repository. How can Alice undo 
her last `git init` in the `moons` subdirectory?

:::{.callout-solution}

## Solution -- USE WITH CAUTION!

### Background
Removing files from a Git repository needs to be done with caution. But we have not learned 
yet how to tell Git to track a particular file; we will learn this in the next episode. Files 
that are not tracked by Git can easily be removed like any other "ordinary" files with

```bash
$ rm filename
```
Similarly a directory can be removed using `rm -r dirname` or `rm -rf dirname`.
If the files or folder being removed in this fashion are tracked by Git, then their removal 
becomes another change that we will need to track, as we will see in the next episode.

### Solution

Git keeps all of its files in the `.git` directory.
To recover from this little mistake, Alice can just remove the `.git`
folder in the moons subdirectory by running the following command from inside the `planets` directory:

```bash
$ rm -rf moons/.git
```
But be careful! Running this command in the wrong directory will remove
the entire Git history of a project you might want to keep.
Therefore, always check your current directory using the command `pwd`.
:::

:::