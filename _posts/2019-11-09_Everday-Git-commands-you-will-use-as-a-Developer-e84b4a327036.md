# Everday Git commands you will use as a Developer

If you are professionally devloping for sometime, you know, probably the single most important tool in your arsenal is Git.

![](https://cdn-images-1.medium.com/max/1200/1*dLwq1wDlnEurHfdOix4FMA.jpeg)

If you are professionally developing for sometime, you know, probably the single most important tool in your arsenal is **Git.**

So here I am trying to list down, most of the git commands that I use on an average week working as a Developer.

**Installing Git for the very first time into a new machine**

**sudo apt-get install git**

See where Git is located:

**which git**

Get the version of Git:

**git --version**

#### Configure Git

After Git is installed on your system, you need to configure Git so that it can understand who is pushing the code to the remote repository, as many people could be working on the project. Run the below commands which are one time only — that is after git is installed for the first time in a machine. And note, we will need to do the same each time I set up a new Cloud based IDE for the first time.

git config --global user.name "**Your name**"  
git config --global user.email "**yourname@emailprovider.com**"

After this, Git on our machine will use these credentials every time I push some code to the remote repository. To view all Git configurations using the command below.

git config --list

Initialise Git in the project root:

**git init**

Get every file in the project ready to commit:

**git add .**

In above, the dot (“.”) signifies the current directory and all the directories beneath it.

Get custom file ready to commit:

**git add index.html**

Commit changes:

**git commit -m "Message"**

Add and commit in one step:

**git commit -am “Message”**

**Push my own branch to remote repo — i.e. NOT to master**

I am working on a local git repository. There are two branches, **master** and **feature\_x.**

I want to push **feature\_x** to the remote repo, but I do not want to push the changes on the **master** branch.

git checkout feature\_x  
git push origin feature\_x

So the regular commands to push from the feature\_x branch of my local machine to the remote git repository.

git add .  
git commit -m 'Making changes in feature\_x branch'  
git push origin feature\_x

Show current branch:

**git branch**

List or Show all the available branches (both local and remote)

**git branch -a**

**Create branch:**

**git branch branchname**

**Change to branch:**

**git checkout branchname**

Create and change to new branch:

**git checkout -b branchname**

The **\-b** option is a convenience flag that tells Git to run `**git branch branchname**` before running `**git checkout branchname**`.

Rename branch:

**git branch -m branchname new\_branchname  
  
**

**To delete the local GIT branch we can try one of the following commands:**

git branch -d branch\_name  
git branch -D branch\_name

as you can see above, we have 2 different argument, one with ‘d’ and one with ‘D’.

The **\-d** option stands for — delete, which would delete the local branch, only if you have already pushed and merged it with your remote branches.

The **\-D** option stands for — delete — force, which deletes the branch regardless of its push and merge status, so be careful using this one!

Update your branch when the original branch from official repository has been updated (applicable when working in teams and many people are committing to a branch)

**git fetch name\_of\_your\_remote**

**However, when branches get deleted on origin, your local repository won’t take notice of that. You’ll still have your locally cached versions of those branches (which is actually good) but git branch -a will still list them as remote branches.**

You can clean up that information locally like this:

git remote prune origin

Your local copies of deleted branches are not removed by this. So to achieve that run below

git fetch — prune

#### Control the default behavior of the [$ git push command](https://git-scm.com/docs/git-config#Documentation/git-config.txt-pushdefault) by setting push.default in your git config file

You can control the default behavior of the \\$ git push command by setting push.default in your git config.

push.default

It defines the action **git push** should take if no refspec is given on the command line, no refspec is configured in the remote, and no refspec is implied by any of the options given on the command line. Possible values are:

**nothing**: do not push anything

**matching:** push all matching branches

All branches having the same name in both ends are considered to be matching.

**upstream**: push the current branch to its upstream branch (tracking is a deprecated synonym for upstream)

**current:** push the current branch to a branch of the same name — **This to my mind is most useful and I have configured my local Development machine’s machine’s gitconfig file to take this option.**

**simple:** like upstream, but refuses to push if the upstream branch’s name is different from the local one.

**To view the current configuration: (If I dont have any custom-configuratons setup, then I shall get nothing after running the below command )**

git config --global push.default

**To set a new configuration:**

git config --global push.default current

**Pulling the latest update from a specific branch**

The “pull” command is used to download and integrate remote changes. **The target** (to which branch the data should be integrated into) is always the currently checked out HEAD branch.

**The source** (from which branch the data should be downloaded from) can be specified in the command’s options.

Before using “git pull”, make sure the correct local branch is checked out. Then, to perform the pull, simply specify which remote branch you want to integrate. Assuming I am in **‘master’** branch in my local machine, and I want to incorporate changes from a remote repository’s **‘dev’** branch into the local machine’s branch.

git checkout dev  
git pull origin dev

The branch-name option can be omitted, however, if a tracking relationship with a remote branch is set up. In most cases, however, your local branch will already have a proper [tracking connection](https://www.git-tower.com/learn/git/ebook/en/desktop-gui/remote-repositories/inspecting-remote-data#start) with a remote branch set up. This configuration provides default values so that the pull command already knows where to pull from without any additional options:

```
git pull
```

It’s often clearer to separate the two actions `git pull` does. The first thing it does is update the local tracking branch that corresponds to the remote branch. This can be done with `git fetch`.

The second is that it then merges in changes, so in its default mode, **_git pull is shorthand for git fetch followed by git merge FETCH\_HEAD._**

[More precisely, git pull runs git fetch with the given parameters and calls git merge to merge the retrieved branch heads into the current branch. With — rebase, it runs git rebase instead of git merge.](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-pull.html)

**What’s the difference between git fetch and git pull?**

Before we talk about the differences between these two commands, lets stress their similarities: both are used to download new data from a remote repository.

**git fetch** really only downloads new data from a remote repository — but it doesn’t integrate any of this new data into your working files. **It fetches all of the branches from the repository. This also downloads all of the required commits and files from the other repository.**

git pull origin master

**git fetch** doesn’t integrate any of this new data into your working files. Fetch is great for getting a fresh view on all the things that happened in a remote repository. Due to its “harmless” nature, fetch will never destroy or manipulate anything in your local machine.

**git pull**, in contrast, is used with a different goal in mind: to update our current HEAD branch with the latest changes from the remote server. This means that pull not only downloads new data; it also directly integrates it into our current working copy files. This has a couple of consequences:

Since **“git pull”** tries to merge remote changes with your local ones, a so-called “merge conflict” can occur. Check out our in-depth tutorial on How to deal with merge conflicts for more information.

Like for many other actions, it’s highly recommended to start a “git pull” only with a clean working copy. This means that you should not have any uncommitted local changes before you pull. Use Git’s Stash feature to save your local changes temporarily.

**Merging two branches**

This is probably the most important and sensitive area to deal with when working in a team.

**Lets say we are working in “dev” branch and want to integrate the changes from “dev” branch back into “master”.**

Before merging our code of master branch into one of our project’s long-running branches (i.e. in this case the “dev” and “master” branch), make sure that your local repository is up to date. Both our local feature / bugfix / “dev” branch and the receiving branch should be updated with the latest changes from our remote server.

The target of this integration (i.e. the branch that _receives_ changes) is always the currently checked out HEAD branch. So, all we have to do is check out the branch we wish to merge into and then run the git merge command:

git checkout master   
git pull  
git merge dev

**Merge Conflicts after doing git merge**

If two people changed the same lines in the same file, or if one person decided to delete it while the other person modified it, Git simply cannot know what is correct. Git will then mark the file as having a conflict — which you’ll have to solve before you can continue your work. And in terminal you will see something like this.

```
CONFLICT (content): Merge conflict in index.jsAutomatic merge failed; fix conflicts and then commit the result.
```

Git hasn’t automatically created a new merge commit. It has paused the process while you resolve the conflict. If you want to see which files are unmerged at any point after a merge conflict, you can run git status:

```
$ git statusOn branch masterYou have unmerged paths.  (fix conflicts and run "git commit")Unmerged paths:  (use "git add <file>..." to mark resolution)    both modified:      index.jsno changes added to commit (use "git add" and/or "git commit -a")
```

Take a look at the contents of the conflicted file. Git was nice enough to mark the problematic area in the file by enclosing it in “<<<<<<< HEAD” and “>>>>>>> \[other/branch/name\]”.

![](https://cdn-images-1.medium.com/max/800/0*Q0eFt6ZMbNqB3uba.png)
undefined

The contents after the first marker originate from our current working branch. After the angle brackets, Git tells us where (from which branch) the changes came from. A line with “=======” separates the two conflicting changes.

This means the version in `HEAD` (our `master` branch, because that was what I had checked out when I ran the **git merge** command) is the top part of that block (everything above the `=======`). This is the receiving branch.

And the version in my `dev` branch looks like everything in the bottom part.

Our job is now to clean up these lines. In a team its advisable to consult other teammate who wrote the conflicting changes to decide which code is finally correct.

**Merging-changes-from-another-branch — To completely overwrite one branch’s all files and commits with another branch**

I have two branches, **old-branch** and **new-dev-branch**. **new-dev-branch** is the latest one and I no more need the **old-branch** changes in **new-dev-branch** branch, yet I don’t want to delete them.

So I just want to dump all the contents of **new-dev-branch** into **old-branch** so that they both point to the same commit. Here’s the commands.

**git checkout old-branch  
git tag old-email-branch # This command is optional  
git reset --hard new-dev-branch**

It very important to note that **git reset — hard** is a potentially dangerous command, since it throws away all your uncommitted changes. For safety, you should always check that the output of git status is clean (that is, empty) before using it.

**Git merge (apply the latest state of one branch into another branch) without including commits from one branch to another When you want to merge your “dev” branch into master**

So this is the simple case where I have a working branch on which I am doing all the experimentation, and at the end of my work, I want to apply all these changes to another branch ( that other branch could be master or any other branch) .

First move into the target branch in Terminal, to which I want to apply the changes. So, here

git checkout master

**Then merge the latest state of the “dev” branch — BUT THIS WILL NOT BE HARD REPLACEMENT OF THE MASTER BRANCH WITH THE DEV BRANCH — SO THERE WILL BE MERGE-CONFLICT**

git merge — squash dev

And then normally I can do a commit

git commit -m “Add new feature”

**The — squash option will squash all of your intermediate changes into one big change. So, it will appear to the other developers as if his changes coming from the dev branch were one giant commit.**

This will merge in the way that target branch would include only its own commits + merge commit and not include the commits from the second branch

This is also useful, when I myself want to merge the commits from a dev branch to the master branch in the local machine itself, before pushing to the remote repo

I create a separate “dev” branch and make all intermediate commits.

Once the code is in good state, make a merge to master. So the master wouldn’t contain “intermediate” commits but only “normal” commits.

Delete the “dev” branch with all intermediate commits.

#### Setting up alias in .gitconfig file with which I can combine multiple commands in a single command

In $HOME/.gitconfig or .git/config include the below along with what ever was already existing in that file

So, here I wanted to create an alias like below,

git p "some message"

Meaning, whenever I run the above command the below commands will run sequentially

git add -A  
git commit -m ”some message”  
git push origin master

So in $HOME/.gitconfig — I had to add the following lines

\[alias\]  
 p = “!f(){ git add -A && git commit -m \\”$1\\”; git push origin master; };f”

and now in Terminal just run the below command

git p “message …”

And it will, under the hood run the following sequence of commands.

git add -A  
git commit -m ”message …”  
git push origin master

#### git alias setup, so that when I run the following command from my local machine’s feature\_branch, it will push to remote repo’s same branch, i.e. by the same name (feature\_branch)

git c 'some message'

This is very useful, as most of the time you will not work in the master branch, you will rather work in some of your own feature branch. And you want to push from that feature\_branch regularly to the remote branch regularly without running multiple git commands.

First set up the default behaviour of the **git push** command, as discussed earlier

git config --global push.default current

This will make sure, that when you run only **git push** without specifying the branch name, it will push to the remote repo from the local machine’s existing branch to the same branch in remote repo. That is, it should pushes the current branch to update a branch with the same name on the receiving end.

**Now in ~/.gitconfig file add the below**

\[alias\]  
  c = "!f(){ git add . && git commit -m \\"$1\\"; git push; };f"

Now when you run **git c ‘some message’** from **current\_branch\_i\_am\_in** it will actually run under the hood the following commands sequentially.

git add -A  
git commit -m ‘some message’  
git push origin current\_branch\_i\_am\_in

#### Some Regular Issue you will face when operating Git

**Issue -1 — After creating a new repositoty you have to run the following as part of the standard commands / steps**

```
git remote add origin git@github.com:username/new_repo
```

But if the above command throws **“fatal: remote origin already exists.”** then run the below command to remove it.

**git remote remove origin**

#### Issue-2 — When I can not push to github — and after passing < git push > getting below error

“remote contains work that you do not have locally” 

Got the error — “Updates were rejected because the remote contains work that you do hint: not have locally. This is usually caused by another repository pushing hint: to the same ref. You may want to first integrate the remote changes hint: (e.g., ‘git pull …’) before pushing again.”

If you are sure you want to overwrite the remote repo’s codes with your local codes — run the below to force push

git push -f origin master

**Issue — 3 — After pushing a commit how to change the comments in that commit — Applicable for the most recent commit**

**git commit — amend**

It will open in-Termianl- text editor, edit the commit message and save the commit. By following the command at the bottom of the Terminal. Ctrl + O for writing out. Then Enter > It will show the file within .git directory, where the amed is being saved. > Enter again > Ctrl + X to exit. Then run below command.

**git push origin master — force**

**Gitignore a file that has already been uploaded / pushed to Git**

If a file is already being tracked by Git, adding the file to .gitignore won’t stop Git from tracking it.

You’ll need to do **_git rm_** the offending file(s) first, then add to your .gitignore.

So, I first run

**git rm -r — cached .idea**

And then updated .gitignore file properly to exclude all .idea folders > then the regular commands

git add .   
git commit -m “updaing gitignore”  
git push

**git stash — How to Save Your Changes Temporarily**

Often, when you’ve been working on part of your project, things are in a messy state and you want to switch branches for a bit to work on something else. The problem is, you don’t want to do a commit of half-done work just so you can get back to this point later. This is where you have to use git stash command.

Stashing takes the dirty state of your working directory — that is, your modified tracked files and staged changes — and saves it on a stack of unfinished changes that you can reapply at any time.

Stashing Your Work

To demonstrate, you’ll go into your project and start working on a couple of files and possibly stage one of the changes. If you run git status, you can see your dirty state:

**git status  
**\# On branch master  
\# Changes to be committed:  
\#   (use "git reset HEAD <file>..." to unstage)  
\#      modified:   index.html  
\## Changes not staged for commit:  
\#   (use "git add <file>..." to update what will be committed)  
\#      modified:   lib/test.js

Now you want to switch branches, but you don’t want to commit what you’ve been working on yet; so you’ll stash the changes. To push a new stash onto your stack, run git stash:

**git stash**

Saved working directory and index state \\

"WIP on master: 049d078 added the index file"

HEAD is now at 049d078 added the index file

(To restore them type "git stash apply")

**Look Your working directory is clean, just as it would have been if I had committed the changes**

**git status**

On branch master  
nothing to commit, working directory clean

At this point, you can easily switch branches and do work elsewhere; your changes are stored on your stack. To see which stashes you’ve stored, you can use git stash list:

**git stash list**

**Continuing Where You Left Off**

Git’s Stash is meant as a temporary storage. When you’re ready to continue where you left off, you can restore the saved state easily:

**git stash apply**

#### git rebase

In Git, there are two main ways to integrate changes from one branch into another: the merge and the rebase

Git’s rebase command reapplies your changes onto another branch. As opposed to merging, which pulls the differences from the other branch into yours, rebasing switches your branch’s base to the other branch’s position and walks through your commits one by one to apply them again.

Is common to first checkout to a branch and then run the rebase command with the name of the branch you wish to rebase on to:

**git checkout my\_feature\_branch  
git rebase dev**

By [Rohan Paul](https://medium.com/@paulrohan) on [November 9, 2019](https://medium.com/p/e84b4a327036).

[Canonical link](https://medium.com/@paulrohan/everday-git-commands-you-will-use-as-a-developer-e84b4a327036)

Exported from [Medium](https://medium.com) on December 12, 2020.