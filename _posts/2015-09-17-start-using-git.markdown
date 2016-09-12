---
layout: post
title:  "Starting using Git + References"
date:   2015-06-17 00:00:00 +0100
categories:
---
This is not going to be a static post (or at least I hope it doesn't). It will contain a quick story of how I started with git, a basic git explanation (as you can find in billions of sites out there it's pointless to repeat it here) and I will use this page as my personal reference for commands (nowadays I keep everything in some notes within [OneNote](https://www.onenote.com/)).Well, my story with git is the following: I was quite used to [VCS](https://en.wikipedia.org/wiki/Version_control) (Version Control System) since I started working, from that time to a while ago I've already worked with [Endevor](https://en.wikipedia.org/wiki/Endevor), [VSS](https://en.wikipedia.org/wiki/Microsoft_Visual_SourceSafe), [SVN](https://en.wikipedia.org/wiki/Apache_Subversion) and [TFS](https://en.wikipedia.org/wiki/Team_Foundation_Server). SVN was far my favourite, because it was light, fast, reliable and usineg it through command line wasn't painful.
<!--more-->
 About 2-3 months ago I was introduced to [GIT](https://en.wikipedia.org/wiki/Git_(software)), I've always been curious about using it but I didn't have a change (I tried once or twice, but only for a couple of hours). In the beginning the concept of using local and remote repository didn't make sense for me, but after starting working with it, this is my favourite VCS! In the beginning, I was using [SourceTree](https://www.sourcetreeapp.com/), an [Atlassian](https://www.atlassian.com/) tool. But a [friend](http://www.nepomuceno.ninja/Managing-Azure-with-GO/) recommended (in this case, pushed me) to use PowerShell with [Posh-Git](https://github.com/dahlbyk/posh-git). So, I started using it, initially for some shell-only commands, then for all the commands (but checking the history), and in the end I am even using it to see the history. And that is it.

## My command references:**

### Staging files
In git, before commit, you need to explicitly stage the files you want to commit. So when you commit you are going to commit only the staged files. Adding one file: `git add {file path}` Adding all modified files: `git add .` Adding all added/deleted/modified files: `git add --all` Forcing to add a file (if it's in the [.gitignore](http://git-scm.com/docs/gitignore) file) `git add --force {same commands as before}`You can also use * for multiple files. For instance, all .txt files inside the folder text would be: `git add text\*.txt`

### Removing (unstage) files
The same way you can stage a file(s) you can unstage the files before a commit. You can select a single file or all files within a folder passing its path, or you can unstage all files.
{% highlight git %}
git reset {path} // single file or folder
git reset . // all files
{% endhighlight %}

### Commit
Commit is actually saving the changes of the staged files. Once you run, it's going to open a screen ([vi-like](https://en.wikipedia.org/wiki/Vi)) and you can add a message (in [vi style](https://www.cs.colostate.edu/helpdocs/vi.html) as well) `git commit` If you want to add the message in line in your commit, you can use the parameter `-m`. If you haven't pushed to remote, you can even amend new things to the last commit. (if you actually need to do that, it's because you screwed something up.
{% highlight git %}
git commit [-m "{message"}] [-a] [--amend]
{% endhighlight %}

### Branches
Create a new branch: `git branch {name of the branch}` Delete a branch: `git branch -d {name of the branch}` Force a deletion a branch: `git branch -D {name of the branch}` List all local: `git branch` List all remote: `git branch -r` List all: `git branch -a` All + tracking information: `git remote show origin`

Sometimes your command is not showing some new branches, or it's showing branches that does not exist. Run the following commands to update the list:

{% highlight git %}
git remote update
git fetch origin --prune // or git remote prune origin`
{% endhighlight %}

### Stashing
Git has a great feature to put code cache into so cache and retrieve it, it's called stash. 2 main uses of that are:

* When you are unable to checkout another branch because you have uncommitted files. Stash them, change the branch, do whatever you want, checkout back to your branch and retrieve the stashed files.
* When you have started working in the wrong branch, and you want to take your changes to the right one. Stash the files, checkout the new branch and retrieve them

The commands for stashing:

* to stash the files: `git stash`
* to retrieve the file stashed: `git stash apply`
* to list stashes: `git stash list`
* git retrieve a specific stash: `git stash apply stash@{X}` : where X is the number in the list

[more about stashing](https://git-scm.com/book/en/v1/Git-Tools-Stashing)

### Changing the remote (origin)
{% highlight git %}
git remote remove origin
git remote add origin {Git server url}
git push
{% endhighlight %}

### Storing Credentials
It can be a bit boring if you need to type your password each time you do a pull/push/update. So there are some commands to store you credentials. You can store either:

*   [wincret](https://help.github.com/articles/caching-your-github-password-in-git/) - Not storing, but using your Windows Credentials
*   [cache](http://git-scm.com/docs/git-credential-cache) - cache into the memory (it doesn't touch the disk
*   [store](http://git-scm.com/docs/git-credential-store) - store unencrypted into the disk

{% highlight git %}
git config --global credential.helper {type}
{% endhighlight %}

### Pushing branches
Pushing branches to the origin is a really easy task, but the wrong configuration can make you do a *huge* mess. Types:

* nothing - it doesn't push *anything* automatically. You need to explicitly say the branch's name
* current - pushes only the current branch you are to the branch with the same name on the receiving end. *I'm currently using this one*
* upstream - pushes only the current branch you are to the branch it's configured as upstream on the receiving end
* simple - it works just like upstream, but if the remote branch doesn't have the same name, it breaks
* matching - pushes all the branch which have branches with the same name on the received end. *I don't recommend this one, as you can push things without noticing.*
{% highlight git %}
git config --global push.default {type}
{% endhighlight %}

### Squashing all commits of a branch
This shouldn't be something you do everyday, but sometimes you need to merge all the commits of your branch into a single one and the normal rebase/squash doesn't work. This list of command is quite useful.
{% highlight git %}
git checkout yourBranch
git reset $(git merge-base master {yourBranch})
git add -A
git commit -m "one commit on yourBranch"
{% endhighlight %}

[Reference](http://stackoverflow.com/questions/25356810/git-how-to-squash-all-commits-on-branch) For now, that's everything. I will improve this post constantly, so soon there will be more commands and more explanations.

Thanks for coming :)
