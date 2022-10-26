# Open research

<div class="questions">

### Questions

- How can version control help my work be more open?
- How can I make my code easier to cite?
- What licensing information should I include with my work?

</div>

<div class="objectives">

### Objectives

- Explain how a version control system can help with open science
- Explain how to provide a guide for people to be able to cite code
- Explain how to find guidance on the right license for your code

</div>  


> *The opposite of "open" isn't "closed".*
>
> *The opposite of "open" is "broken".*
>
> *--- John Wilbanks*

Increasingly, every part of the research life-cycle, from data collection, 
analysis and computation, and pre-publication and review, can take place on 
open platforms.

Version control, and public repositories on GitHub or an
institutional repo, can serve as the equivalent of a shareable electronic
lab notebook for computational work.

* The conceptual stages of your work are documented, including who did what and when. Every step is stamped with an identifier (the commit ID) that is for most intents and purposes unique.
* You can tie documentation of rationale, ideas, and other intellectual work directly to the changes that spring from them.
* You can refer to what you used in your research to obtain your computational results in a way that is unique and recoverable.
* With a version control system such as Git, the entire history of the repository is easy to archive for perpetuity.

This makes it easier for computational research to be reproducible. Analytical
tools can be refined and improved, but earlier versions are still available
if required. Many academic journals require that any code used to generate results shown in the publication be provided and shared on GitHub, so, depending on discipline, you may not be able to publish without using git.

## Making code citable

Anything that is hosted in a version control repository (data, code, papers,  etc.) can be turned into a citable object.

To help people cite your code, you can include a file called `CITATION` or `CITATION.txt` that describes how to reference your project; for an example, see
the [citation guide for scikit-learn](https://scikit-learn.org/stable/about.html#citing-scikit-learn) or many other large open-source projects.

More detailed advice, and other ways to make your code citable can be found
[at the Software Sustainability Institute blog](https://www.software.ac.uk/how-cite-and-describe-software).

## Licensing your code

When a repository with source code, a manuscript or other creative works becomes
public, it should include a file `LICENSE` or `LICENSE.txt` in the base
directory of the repository that clearly states under which license the content
is being made available. This is because creative works are automatically
eligible for intellectual property (and thus copyright) protection. Reusing
creative works without a license is dangerous, because the copyright holders
could sue you for copyright infringement.

A license solves this problem by granting rights to others (the licensees) that
they would otherwise not have. What rights are being granted under which
conditions differs, often only slightly, from one license to another. In
practice, a few licenses are by far the most popular, and [choosealicense.com](https://choosealicense.com/) will help you find a common license that suits
your needs.  Important considerations include:

* Whether you want to address patent rights.
* Whether you require people distributing derivative works to also distribute their source code.
* Whether the content you are licensing is source code.
* Whether you want to license the code at all.

Choosing a license that is in common use makes life easier for contributors and
users, because they are more likely to already be familiar with the license and
don't have to wade through a bunch of jargon to decide if they're ok with it.
The [Open Source Initiative](https://opensource.org/licenses) and [Free Software Foundation](https://www.gnu.org/licenses/license-list.html) both maintain lists
of licenses which are good choices.

[This article](https://doi.org/10.1371/journal.pcbi.1002598) provides an excellent overview of
licensing and licensing options from the perspective of scientists who
also write code.

At the end of the day what matters is that there is a clear statement as to what
the license is. Also, the license is best chosen from the get-go, even if for a
repository that is not public. Pushing off the decision only makes it more
complicated later, because each time a new collaborator starts contributing,
they, too, hold copyright and will thus need to be asked for approval once a
license is chosen.


:::{.callout-tip}

## How to Track Large Data or Image Files using Git?

Large data or image files such as `.md5` or `.psd` file types can be tracked within 
a GitHub repository using the [Git Large File Storage](https://git-lfs.github.com)
open source extension tool.  This tool automatically uploads large file contents to 
a remote server and replaces the file with a text pointer within the GitHub repository.

Try downloading and installing the Git Large File Storage extension tool, then add 
tracking of a large file to your GitHub repository.  Ask a colleague to clone your
repository and describe what they see when they access that large file. 

If/when you need to do this in your own research, it may be wise to consider one of the tools discussed [here](https://neptune.ai/blog/best-data-version-control-tools) OR to reach out to SIH or the corresponding support unit at your University for ideas on how to version control your data most effectively.
:::

<div class="keypoints">

### Key Points

- Open research is more useful and highly-cited than closed.
- Adding a citation guide makes your work easier to cite.
- There are a range of open-source licenses to suit different cases.

</div>