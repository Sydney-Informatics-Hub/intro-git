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

Once Git is configured, we can start using it.



First, let's create a new directory in the `Desktop` folder for our work and
then change the current working directory to the newly created one:

```sh
cd ~/Desktop
mkdir mean
cd mkdir mean
```

Then we tell Git to make `mean` a repository - a place where Git can store versions of our files:


```sh
git init
```

```sh
Initialized empty Git repository in /Users/alice/Desktop/mean/.git/
```

It is important to note that `git init` will create a repository that
can include subdirectories and their files---there is no need to create
separate repositories nested within the `mean` repository, whether
subdirectories are present from the beginning or added later. Also, note
that the creation of the `mean` directory and its initialization as a
repository are completely separate processes.

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

```sh
ls
```

But if we add the `-a` flag to show everything, we can see that Git has
created a hidden directory within `mean` called `.git`:

```sh
# also use `-1` flag to get the output as a column
ls -a1
```

```sh
.
..
.git
```

Git uses this special subdirectory to store all the information about the project, 
including the tracked files and sub-directories located within the project's directory.
If we ever delete the `.git` subdirectory, we will lose the project's history.

Next, we will change the default branch to be called `main`.
This might be the default branch depending on your settings and version
of git.
See the [setup episode](../setup.html) for more information on this change.

```sh
git checkout -b main
```

```abc
Switched to a new branch 'main'
```


We can check that everything is set up correctly
by asking Git to tell us the status of our project:

```sh
git status
```

```abc
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

If you are using a different version of `git`, the exact
wording of the output might be slightly different.


<div class="challenge">

### Challenge: Places to Create Git Repositories

Along with tracking information about our base project (the project we have
already created), 
Alice would also like to track information about moons.
Despite Bob's concerns, Alice creates a `moons` project inside her `mean` 
project with the following sequence of commands:

```sh
cd ~/Desktop   # return to Desktop directory
cd mean        # go into mean directory, which is already a Git repository
ls -a          # ensure the .git subdirectory is still present in the mean directory
mkdir moons    # make a subdirectory mean/moons
cd moons       # go into moons subdirectory
git init       # make the moons subdirectory a Git repository
ls -a          # ensure the .git subdirectory is present indicating we have created a new Git repository
```
Is the `git init` command, run inside the `moons` subdirectory, required for 
tracking files stored in the `moons` subdirectory?

<details>
<summary>Solution</summary>

No. Alice does not need to make the `moons` subdirectory a Git repository 
because the `mean` repository can track any files, sub-directories, and 
subdirectory files under the `mean` directory.  Thus, in order to track 
all information about moons, Alice only needed to add the `moons` subdirectory
to the `mean` directory.
 
Additionally, Git repositories can interfere with each other if they are "nested":
the outer repository will try to version-control
the inner repository. Therefore, it's best to create each new Git
repository in a separate directory. To be sure that there is no conflicting
repository in the directory, check the output of `git status`. If it looks
like the following, you are good to go to create a new repository as shown
above:

```sh
git status
```

```abc
fatal: Not a git repository (or any of the parent directories): .git
```

</details>
</div> 



<div class="challenge">

### Challenge: Correcting `git init` Mistakes

Bob explains to Alice that a nested repository is redundant and may cause confusion
down the road. Alice would like to remove the nested repository. How can Alice undo 
her last `git init` in the `moons` subdirectory?

<details>
<summary>Solution</summary>

** USE WITH CAUTION! **

#### Background
Removing files from a Git repository needs to be done with caution. But we have not learned 
yet how to tell Git to track a particular file; we will learn this in the next episode. Files 
that are not tracked by Git can easily be removed like any other "ordinary" files with

```sh
rm filename
```
Similarly a directory can be removed using `rm -r dirname` or `rm -rf dirname`.
If the files or folder being removed in this fashion are tracked by Git, then their removal 
becomes another change that we will need to track, as we will see in the next episode.

#### Solution

Git keeps all of its files in the `.git` directory.
To recover from this little mistake, Alice can just remove the `.git`
folder in the moons subdirectory by running the following command from inside the `mean` directory:

```sh
rm -rf moons/.git
```
But be careful! Running this command in the wrong directory will remove
the entire Git history of a project you might want to keep.
Therefore, always check your current directory using the command `pwd`.

</details>
</div> 

:::{.callout-tip}
## Legitimate uses for nesting

But what happens if you WANT to have a nested repository, for example when you're using a public project that you want to get updates for as part of your repository? Or a standard code library used across your entire lab/team that you all collectively maintain in another GitHub repo? This is the use case for something called [submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules). Submodules are beyond the scope of this course, but we want to mention them here, in case you do need to do a version of the above, mixing open source software and your own tools in one repository.

:::


<div class="keypoints">

### Key Points

- `git init` initializes a repository.
- Git stores all of its repository data in the `.git` directory.

</div>  