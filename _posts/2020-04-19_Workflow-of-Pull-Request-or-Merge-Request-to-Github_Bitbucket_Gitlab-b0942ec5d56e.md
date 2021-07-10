# Workflow of Pull Request or Merge Request to Github / Bitbucket / Gitlab

Probably the most regular part of a Developer’s day to day workflow, when working in a team, is raising Pull Request or PR (as its called…

![](https://cdn-images-1.medium.com/max/800/1*xxvKV6swdzP_B1JLbCUEAQ.jpeg)

Raising and managing **Pull Request or PR** (as its called in Github and Bitbucket ) or **Merge Request or MR** (as its called in Gitlab) is probably the most regular part of a Developer’s day to day workflow, when working in a team.

In this post I shall discuss the steps and workflows and issues you may face while raising your PR for a Repo.

And I will assume that you have forked the main upstream Repository and work on that fork and finally create a PR / MR to the main Repository. If you are working on a branch of the same Repository, the whole process is actually a bit more simpler and even then most of the steps I will mention here will still remain same.

#### First part of the workflow

**Working on the forked Repo and merging the dev-branch (or whatever name you give to your forked repo’s branch) of the fork with master of the main upstream Repo.**

A> Fork the repo from the upstream remote repo to your personal github

B> Create a Branch in the forked repository (This is Optional)

C> Work on that branch, and at the end merge it with the Master Branch (in the forked repo itself, i.e if you have actually worked on a separate Branch of your forked repo)

Lets say my Upstream Repo name is **institutions-web** under the Organization name of MyOrganization.

So basically you have to fork the repo from MyOrganization to your personal github > then clone the repo into local machine to work locally > after development-work is done in my locally cloned repo from the root of the project run following command

$ git remote add upstream git@github.com:MyOrganization/institutions-web.git

i.e. the very first time before running < git fetch >

The above step configures a git remote for a fork. This step is ONLY required ONCE per repository, which is Add a new remote upstream repository to sync with the fork.

Now, I actually have to sync my local forked Repo with the remote Repo, before raising a new PR, this is VERY IMPORTANT as it will avoid all merge-conflict later on. So run below command to fetch project branches from the upstream repository to get all the commits.

$ git fetch upstream

Your will get below form of response in Terminal

remote: Enumerating objects: 1, done.

remote: Counting objects: 100% (1/1), done.

remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0

Unpacking objects: 100% (1/1), done.

From github.com:MyOrganization/institutions-web

a6cd1e82..68ce35a3 master -> upstream/master

**After fetching from the remote branch, you would still have to merge the commits. So you can actually replace**

$ git fetch upstream

with

$ git pull upstream master

since git pull is essentially git fetch + git merge.

And now, Merge the changes from upstream/master into your local master branch. This brings your fork’s master branch into sync with the upstream repository, without losing your local changes.

$ git checkout master

$ git merge upstream/master

#### Generally follow the below principles

You will probably find things more manageable if you work on a feature branch rather than your own master branch. That is, start with git checkout -b my-feature before you start making any changes. If you will be regularly contributing upstream, your life is easiest if your master branch always reflects the state of the upstream master branch at some point in time, because this permits you to update your local master branch with a simple

git pull upstream master

Avoiding merging code into your feature branches. Regularly rebase on upstream/master if you need to incorporate upstream changes.

Now continue your regular development work, in the local machine. And keep pushing regularly

git add -A

git commit -m “adding storybook, a development environment for UI components”

git push origin feature-branch-name

i.e. the format is -

git push origin <new\_topic-branch-name-which-I-created>

#### Second and Final part of the workflow to actualy raise the PR from Github page in the browser

Then in the browser navigate to the original URL of the Original repo.

Click on “Create Pull Request”.

#### Updating a Pull Request (that has already been raised) from my forked repo

The case is as below and often repeated — So I first forked a repo and then made a commit to that forked repo. I then opened a pull request. The pull request listed all the changes I wanted.

After reviewing my pull request, there were a number of changes that the repo owner wanted me to make before he accepted it. I have made those changes in my fork, now how do I update the pull request with those changes

**Ans is, basically I have to do nothing** — just make changes of the forked repo, in that branch from which I sent the PR ( e.g. master ), i.e. just do a regular commit in your own local repo in local machine. Push this relevant branch of your fork to Github. And the existing PR will show all these changes immediately.

And thats it, the PR will reflect this changes immediately.

#### What’s the difference between git fetch and git pull?

Before we talk about the differences between these two commands, lets stress their similarities: both are used to download new data from a remote repository.

**git fetch** really only downloads new data from a remote repository — but it doesn’t integrate any of this new data into your working files. It fetches all of the branches from the repository. This also downloads all of the required commits and files from the other repository.

git pull origin master

**git fetch** doesn’t integrate any of this new data into your working files. Fetch is great for getting a fresh view on all the things that happened in a remote repository. Due to its “harmless” nature, fetch will never destroy or manipulate anything in your local machine.

**git pull**, in contrast, is used with a different goal in mind: to update our current HEAD branch with the latest changes from the remote server. This means that pull not only downloads new data; it also directly integrates it into our current working copy files. This has a couple of consequences:

Since **git pull** tries to merge remote changes with your local ones, a so-called “merge conflict” can occur. Check out our in-depth tutorial on How to deal with merge conflicts for more information.

Like for many other actions, it’s highly recommended to start a **\*\*git pull\*\*** only with a clean working copy. This means that you should not have any uncommitted local changes before you pull. Use Git’s Stash feature to save your local changes temporarily.

So when you are working on Forked Repo, after fetching from the remote branch, you would still have to merge the commits. So you can actually replace

$ git fetch upstream

with

$ git pull upstream master

since git pull is essentially git fetch + git merge.

#### Squashing before raising a PR

One of the feature git, has enabled us to record all changes by the commit. And saved it as our history. So when something unexpected happens, you can rollback to specific commits.

But, too many commits may mess your git history. If you have a lot of fixup commits, and you merge all of them directly into master, the git history will be bloated (which is something we don’t want). So, if your change consists of two commits `X` and `Y`, we want to squash them into a single commit `Z`

![](https://cdn-images-1.medium.com/max/800/1*MNMDymJsK1ph4YL0kKiDLA.jpeg)
undefined

There’s also another way also to resolve this problem of too many commits for a single PR. Which is, as some folks resorts to it, by hastily creating a new branch, porting all changes to it with a patch file and creating a separate pull request. But this is headache both for the contributor and project maintainer. **There’s an easier way, which is git squash**

So before we start the process of squashing, first find out the number of commits we have made, we can inspect the total number of commits that have been made to the project with the following command:

git log

This will provide you with output that looks similar to this

Output

commit 46f196203a16b448bf86e0473246eda1d46d1273

Author: username-2 <email-2>

Date: Mon Dec 14 07:32:45 2015 -0400

Commit details

commit 66e506853b0366c87f4834bb6b39d941cd034fe3

Author: username1 <email-1>

Date: Fri Nov 27 20:24:45 2015 -0500

Commit details

The log shows all the commits made to the given project’s repository, so your commits will be mixed with the commits made by others. For projects that have an extensive history of commits by multiple authors, you’ll want to specify yourself as author in the command:

git log — author=your-username

Now if you know the number of commits you’ve made on the branch (just count it) that you want to rebase, you can simply run the git rebase command like so:

git rebase -i HEAD~x

e.g.

git rebase -i HEAD~8

If, however, you don’t know how many commits you have made on your branch, you’ll need to find which commit is the base of your branch, which you can do by running the following command:

git merge-base new-branch master

This command will return a long string known as a commit hash, something that looks like this:

Output  
66e506853b0366c87f4834bb6b39d341cd094fe9

We’ll use this commit hash to pass to the git rebase command:

git rebase -i 66e506853b0366c87f4834bb6b39d341cd094fe9

For either of the above commands, your command-line text editor will open with a file that contains a list of all the commits in your branch, and you can now choose whether to squash commits or reword them.

Now after running

git rebase -i HEAD~8

The following editor will open in the Terminal and this is where I have to choose which commit to Squash and which ones to Pick

When we squash commit messages, we are squashing or combining several smaller commits into one larger one.

In front of each commit you’ll see the word “pick,” so your file will look similar to this if you have two commits:

GNU nano 2.0.6 File: …username/repository/.git/rebase-merge/git-rebase-todo  
pick a1f29a6 Adding a new feature  
pick 79c0e80 Here is another new feature  
\# Rebase 66e5068..79c0e80 onto 66e5068 (2 command(s))

Now, for each line of the file you should replace the word “pick” with the word “squash” to combine the commits: And these lines will be arranged top to bottom like so, the most recent one (i.e. most recent commit) at the bottom most positon. And if I want to keep the lastest commit msg, all I have to do is edit the top-most line as pick and rest of all as squash.

GNU nano 2.0.6 File: …username/repository/.git/rebase-merge/git-rebase-todo

pick a1f29a6 Adding a new feature

squash 79c0e80 Here is another new feature

At this point, you can save and close the file ( Choosing the option shown in the Terminal, **Control + O for writing out, then just press Enter for choosing the default file to write to and then finally Control+X to exit)**. After first exit, it will open the section editor inside Terminal where I should be able to edit and add the comments for my commit. So the same flow here as well. Edit inside Terminal Editor > press Control+O to write out > Press Enter to save to default file > Control+X to Exit

#### One common issue you may face while squahing, which is this error — Git: “Cannot ‘squash’ without a previous commit” error while rebase

Why it will happen in most cases is that, you cannot squash older commits onto a new commit. Here is an example say you have 3 commits as below:

pick 01mn9h78 The lastest commit  
pick a2b6pcfr A commit before the latest  
pick 093479uf An old commit i made a while back

Now if you do

git rebase -i HEAD~3 

and you do something like

pick 01mn9h78 The lastest commit  
s a2b6pcfr A commit before the latest  
s 093479uf An old commit i made a while back

This will result in the error :

**error: cannot ‘squash’ without a previous commit You can fix this with ‘git rebase — edit-todo’ and then run ‘git rebase — continue’. Or you can abort the rebase with ‘git rebase — abort’.**

**Solution : When squashing commits , you should squash recent commits to old ones not vice versa thus in the example it will be something like this :**

**So put the ‘squash’ or ‘s’ word before the latest comment and ‘pick’ word before the oldest one**

s 01mn9h78 The lastest commit  
s a2b6pcfr A commit before the latest

pick 093479uf An old commit i made a while back

#### Now finally pushing or updating the PR

**Once you perform arebase, the history of your branch changes, and you are no longer able to use the git push command because the direct path has been modified.**

And you can check that by doing a **git status** You will get

our branch and ‘origin/master’ have diverged,  
and have 1 and 2 different commits each, respectively.  
 (use “git pull” to merge the remote branch into yours)  
nothing to commit, working tree clean

We will have to instead use the — force or -f flag to force-push the changes, informing Git that you are fully aware of what you are pushing.

At this point, we should ensure that we are on the correct branch by checking out the branch we are working on:

git checkout new-branch

Output  
Already on ‘new-branch’

Now we can perform the force-push:

git push -f

I could also do below, assuming I am in master branch, and all my squashing activities were in master branch

git push origin master — force

And now if I go to the exiting PR in Github, I will see the same PR has got fully updated with my latest changes, with just a single commit.

Note, all these squashing activities could have been done by the Repo’s Manager as well. The repository’s manager can squash all the commits in a pull request into a single commit by selecting “Squash and merge” on a pull request.

#### Another ISSUE — How to squash commits after the pull request has been opened ? That is, after I have created a PR, then squash my commits in my local machine and then when I go to the PR of the upstream Repo, I still see all the committs that were there previously.

**Solution — (the below will save you many times)**

The easiest way to squash all of these changes is probably start by resetting your current branch back to the upstream master branch:

$ git reset upstream/master

**The magical thing abuout the above command is, this will reset the repository, but not your working directory, to the state of the upstream/master branch.** Since it doesn’t modify the state of your working directory, this means that all your changes are preserved, but not the commit history. At this point, we see:

  
\\$ git status  
\[…\]  
Changes not staged for commit:  
(use “git add <file>…” to update what will be committed)  
(use “git checkout — <file>…” to discard changes in working directory)

modified: app/server.go  
 modified: smtp.go

no changes added to commit (use “git add” and/or “git commit -a”)

Now we can create a new commit:

$ git add -u  
$ git commit

**Now you have a single commit on top of the upstream master branch. You would then force push this to your own master branch, which would update the PR.**

(NB: if you’re worried about screwing something up or losing your changes or anything like that, either work on a new branch, or just make a local copy of your working directory and work on that instead.)

This is pretty much what I follow during my regular PR work flow process. Hope this helps.

By [Rohan Paul](https://medium.com/@paulrohan) on [April 19, 2020](https://medium.com/p/b0942ec5d56e).

[Canonical link](https://medium.com/@paulrohan/workflow-of-pull-request-or-merge-request-to-github-bitbucket-gitlab-b0942ec5d56e)

Exported from [Medium](https://medium.com) on December 12, 2020.