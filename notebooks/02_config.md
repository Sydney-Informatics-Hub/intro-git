# Configuring Git

<div class="questions">

### Questions

- How do I configure Git?

</div>

<div class="objectives">

### Objectives

- Configure `git` the first time it is used on a computer.
- Understand the meaning of the `--global` configuration flag.

</div>  

![One does not simply understand Git](../fig/meme_lotr_git.jpg)


When we use Git on a new computer for the first time,
we need to configure a few things. Below are a few examples
of configurations we will set as we get started with Git:

*   Our name and email address,
*   What our preferred text editor is
*   That we want to use these settings globally (i.e. for every project, so we don't need to set it every time we create a new project/repository)

On a command line, Git commands are written as `git verb options`,
where `verb` is what we actually want to do and `options` is additional optional information which may be needed for the `verb`. So here is how I set up my new laptop:

```sh
git config --global user.name "Mike Lynch"
git config --global user.email "m.lynch@sydney.edu.au"
```
Please use your own name and email address instead of mine. This user name and email will be associated with your subsequent Git activity,
which means that any changes pushed to [GitHub](https://github.com/),
[BitBucket](https://bitbucket.org/),
[GitLab](https://gitlab.com/) or
another Git host server
after this lesson will include this information.

**For this lesson, we will be interacting with [GitHub](https://github.com/) and so the email address used should be the same as the one used when setting up your GitHub account.** If you are concerned about privacy, please review [GitHub's instructions for keeping your email address private](https://help.github.com/articles/keeping-your-email-address-private/). 

:::{.callout-tip}
## Keeping your email private

If you elect to use a private email address with GitHub, then use that same email address for the `user.email` value, e.g. `username@users.noreply.github.com` replacing `username` with your GitHub one.
:::

:::{.callout-tip}
## Line Endings

As with other keys, when you hit <kbd>Enter</kbd> or <kbd>↵</kbd> or on Macs, <kbd>Return</kbd> on your keyboard,
your computer encodes this input as a character.
Different operating systems use different character(s) to represent the end of a line.
(You may also hear these referred to as newlines or line breaks.)
Because Git uses these characters to compare files,
it may cause unexpected issues when editing a file on different machines. 
Though it is beyond the scope of this lesson, you can read more about this issue 
[in the Pro Git book](https://www.git-scm.com/book/en/v2/Customizing-Git-Git-Configuration#_core_autocrlf).

You can change the way Git recognizes and encodes line endings
using the `core.autocrlf` command to `git config`.
The following settings are recommended:

On macOS and Linux:

```sh
git config --global core.autocrlf input
```
And on Windows:

```sh
git config --global core.autocrlf false
```
:::



:::{.callout-tip}
## Exiting Vim

Note that Vim is the default editor for many programs. If you haven't used Vim before and wish to exit a session without saving
your changes, press <kbd>Esc</kbd> then type `:q!` and hit <kbd>Enter</kbd> or <kbd>↵</kbd> or on Macs, <kbd>Return</kbd>.
If you want to save your changes and quit, press <kbd>Esc</kbd> then type `:wq` and hit <kbd>Enter</kbd> or <kbd>↵</kbd> or on Macs, <kbd>Return</kbd>.
:::

### Using other text editors

If you'd like to use another plain text editor, you can also configure git to use it. Below we provide (and hide) the configuration options for a number of possible options:

<details>

<summary>Details</summary>

#### Atom

```sh
git config --global core.editor "atom --wait"
```

#### nano

```sh
git config --global core.editor "nano -w"
```

#### BBEdit (Mac, with command line tools)

```sh
git config --global core.editor "bbedit -w"
```

#### Sublime Text (Mac)

```sh
git config --global core.editor "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl -n -w"
```

#### Sublime Text (Win, 32-bit install)

```sh
git config --global core.editor "'c:/program files (x86)/sublime text 3/sublime_text.exe' -w"
```

#### Sublime Text (Win, 64-bit install)

```sh
git config --global core.editor "'c:/program files/sublime text 3/sublime_text.exe' -w"
```

#### Notepad (Win)

```sh
git config --global core.editor "c:/Windows/System32/notepad.exe"
```

#### Notepad++ (Win, 32-bit install)

```sh
git config --global core.editor "'c:/program files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

#### Notepad++ (Win, 64-bit install)

```sh
git config --global core.editor "'c:/program files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

#### Kate (Linux)

```sh
git config --global core.editor "kate"
```

#### Gedit (Linux)

```sh
git config --global core.editor "gedit --wait --new-window"
```

#### Scratch (Linux)

```sh
git config --global core.editor "scratch-text-editor"
```

#### Emacs

```sh
git config --global core.editor "emacs"
```

#### Vim

```sh
git config --global core.editor "vim"
```

#### VS Code

```sh
git config --global core.editor "code --wait"
```


It is possible to reconfigure the text editor for Git whenever you want to change it.

</details>

## Default branch naming

Git (2.28+) allows configuration of the name of the branch created when you
initialize any new repository.  We'll set it to `main`:

```sh
git config --global init.defaultBranch main
```


:::{.callout-note}
## Some history of default Git branch naming

Source file changes are associated with a "branch." For new learners in this lesson, it's enough to know that branches exist, and this lesson uses one branch.  By default, Git will create a branch called `master`  when you create a new repository with `git init` (as explained in the next Episode). This term evokes the racist practice of human slavery and the 
[software development community](https://github.com/github/renaming)  has moved to adopt more inclusive language. 

In 2020, most Git code hosting services transitioned to using `main` as the default 
branch. As an example, any new repository that is opened in GitHub and GitLab default 
to `main`.  However, Git has not yet made the same change.  As a result, local repositories 
must be manually configured have the same main branch name as most cloud services.  

For versions of Git prior to 2.28, the change can be made on an individual repository level.  The  command for this is in the next episode.  Note that if this value is unset in your local Git configuration, the `init.defaultBranch` value defaults to `master`.
:::

The five commands we just ran above only need to be run once: the flag `--global` tells Git
to use the settings for every project, in your user account, on this computer.

You can check your settings at any time:

```sh
git config --list
```

You can change your configuration as many times as you want: use the
same commands to choose another editor or update your email address.

:::{.callout-tip}
## Proxy

In some networks you need to use a
[proxy](https://en.wikipedia.org/wiki/Proxy_server). If this is the case, you
may also need to tell Git about the proxy:

```sh
git config --global http.proxy proxy-url
git config --global https.proxy proxy-url
```

To disable the proxy, use

```sh
git config --global --unset http.proxy
git config --global --unset https.proxy
```
:::

:::{.callout-tip}
## Git Help and Manual

Always remember that if you forget the subcommands or options of a `git` command, you can access the
relevant list of options typing `git <command> -h` or access the corresponding Git manual by typing
`git <command> --help`, e.g.:

```sh
git config -h
git config --help
```
While viewing the manual, remember the `:` is a prompt waiting for commands and you can press <kbd>Q</kbd> to exit the manual.

More generally, you can get the list of available `git` commands and further resources of the Git manual typing:

```sh
git help
```
:::

<div class="keypoints">

### Key Points

- Use `git config` with the `--global` option to configure a user name, email address, editor, and other preferences once per machine.

</div>  