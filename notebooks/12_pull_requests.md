# Pull requests

<div class="questions">

### Questions

- How can I contribute to someone else's project with a pull request?

</div>

<div class="objectives">

### Objectives

- Understand how to fork someone else's codebase
- Understand how to contribute your changes back to theirs with a pull request

</div>  


<div class="keypoints">

### Key Points

- Forking allows you to create your own copy of an existing open-source project
- Pull requests are a way to offer your changes back to a project

</div>

We've just seen a branching workflow for allowing collaboration between a team
of developers, each of whom has a local repo and who share a repo on GitHub.

In this section, we'll step through what's called a fork-based workflow, which
is a way to contribute code to another team's project.

A fork-based workflow has the following steps:

1. You fork the projects repo on GitHub - this is similar to cloning, but you're
creating a clone of the repo in your own GitHub account, not on your local machine.

2. You create a local copy of your fork of the project

3. You create a branch on your repo where you'll work on the changes that you
want to contribute

4. You push your changes to your remote copy of the project's repo

5. On GitHub, you create a pull request for the maintainer of the original
project

It's called a pull request because you're asking the maintainer to pull your
contribution into their version of the project.

After you've made the pull request, the maintainer can accept it straight away,
or start a form of collaboration to talk about the changes - this can take the 
form of a code review. We won't have time to cover that in this workshop, but
it's a very common form of quality control for open source projects, and is
also used within teams.

### Outline of this section

1. Bob already has a copy of the repository, so he doesn't need to fork it

3. Bob starts a branch called `my-cool-feature`

4. Bob makes a change to mean.py 

5. Bob pushes his change to GitHub

6. We get Alice to also make a change to her main branch of mean.py

7. Alice pushes her change to GitHub

8. Bob creates a pull request based on his `my-cool-feature` branch 

9. Alice confirms to see if the pull request is there

10. Alice tries to merge and we get to the conflict.

