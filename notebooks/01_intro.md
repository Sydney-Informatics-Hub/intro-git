# Automated Version Control

<div class="questions">

### Questions

- What is version control and why should I use it?
</div>

<div class="objectives">

### Objectives

- Understand the benefits of an automated version control system.
- Understand the basics of how automated version control systems work.

</div>  


## What we'll cover in this workshop

This workshop will introduce you to Git and show you how it can be used at a
couple of different levels.

1. First, we'll create a local git repository, and use it to keep track of changes to a very simple Python program. 

2. We'll then learn how to push code from our local repository to GitHub. These two steps may be everything you want to use as a beginner, to write code (or your thesis) and to back it up online on GitHub.

3. Next, we'll level up to look at how GitHub can be used to share your code with a collaborator. 

4. We'll then learn about branches, and how they can be used to maintain different versions of the same code, and help to organise your team's software development or co-developed data science project.

5. And in the final section, we'll show you how to fork a repository and create a pull request, which is how Git and GitHub are used to manage contributions to open source software projects.

6. We'll finish with some best practices for open research with Git.

## What is version control?

We'll start by exploring how version control can be used to keep track of what
one person did and when. Even if you aren't collaborating with other people,
automated version control is much better than this situation:

!["Piled Higher and Deeper" by Jorge Cham, http://www.phdcomics.com](../fig/phd101212s.png)

Or this one:

```sh
script1_current.py
script1_last_working.py
script1_newfeature.py
script1_nicetohavefeature.py
...
...
script100_last_working.py
script100_nicetohavefeature.py
```


We've all been in this situation before: it seems unnecessary to have multiple nearly-identical versions of the same document or script - but life just seems to get in the way.

Some word processors let us deal with this a little better, such as Microsoft Word's 
[Track Changes](https://support.office.com/en-us/article/Track-changes-in-Word-197ba630-0f5f-4a8e-9a77-3712475e806a).

Version control systems start with a base version of a document - or a script file - and then record changes you make each step of the way. You can think of it as a recording of your progress: you can rewind to start at the base document and play back each change you made, eventually arriving at your more recent version.

![Changes Are Saved Sequentially](../fig/play-changes.svg)

Once you think of changes as separate from the document itself, you can then think about "playing back" different sets of changes on the base document, ultimately resulting in different versions of that document. For example, two users can make independent sets of changes on the same document. 

![Different Versions Can be Saved](../fig/versions.svg)

Unless multiple users make changes to the same section of the document - a conflict - you can  incorporate two sets of changes into the same base document.

![Multiple Versions Can be Merged](../fig/merge.svg)

## Version control systems

A **version control system** is a tool that keeps track of these changes for us, effectively creating different versions of our files. It allows us to decide
which changes will be made to the next version (each record of these changes is called a **commit**), and keeps useful metadata
about them. The complete history of commits for a particular project and their metadata make up a **repository**.
Repositories can be kept in sync across different computers, facilitating collaboration among different people.

## The Long History of Version Control Systems

Automated version control systems are nothing new.
Tools like [RCS](https://en.wikipedia.org/wiki/Revision_Control_System), [CVS](https://en.wikipedia.org/wiki/Concurrent_Versions_System), or [Subversion](https://en.wikipedia.org/wiki/Apache_Subversion) have been around since the early 1980s and are used by many large companies.
However, many of these are now considered legacy systems (i.e., outdated) due to various  limitations in their capabilities.

More modern systems, such as Git and [Mercurial](https://swcarpentry.github.io/hg-novice/), are *distributed*, meaning that they do not need a centralized server to host the repository.
These modern systems also include powerful merging tools that make it possible for multiple authors to work on the same files concurrently.

The vast, vast majority of developers of both open-source and commercial software today use git as their version control system of choice - which is why we are teaching it in this course, and use it across all of our projects at SIH.

<div class="keypoints">

### Key points

- Version control is like an unlimited 'undo' (kind of, and not always)
- Version control allows members of a team to work in parallel
- Version control systems like GitHub are how people contribute to open source software

</div>  
