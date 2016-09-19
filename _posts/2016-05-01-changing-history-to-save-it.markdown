---
layout: post
title:  "Changing history to save it"
date:   2016-05-01 00:00:00 +0100
categories: git vcs
---
[Rewriting history](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
_Case:_ This is a real case, where it was needed to do some git tricks to avoid leaking some important information. One of these days, some friends built a lib to access an external dependencies and one of them just committed a our connection string, not only that, but also he committed it a LOT of times in different places (don’t ask me how he managed to do it, some people just have a gift to build a mess).
<!--more-->
As we wanted to make the project available open source, we couldn’t just open the repository, even after changing the key, as the history would contain the original one. In the end we decided to kill all the commits which were done before another friend finally changed all the keys to obfuscate the right one. I tried to reproduce a fake log in a local git, it looked something like: `git l -3`
![git-rewrite-history-01]({{ site.url }}/assets/2016-05-01-changing-history-to-save-it/git-rewrite-history-01.png)

And if you looked for, the key was easily spotted `git show 12e77ba`
![git-rewrite-history-02]({{ site.url }}/assets/2016-05-01-changing-history-to-save-it/git-rewrite-history-02.png)

So, how to get rid of the commits with the key? Git has an impressive power to rewrite the history (what might be dangerous sometimes), as our scenario was slightly harder than this, we just decided to do a squash for first commits and lose this change forever.
Squash is a feature for git which allows you to merge multiples commits into one single commit. Of course you lose the track of the changes you did, as you just save the final state of the file (last commit).
You can squash using the `rebase` command from git. Which you need to run in the interactive way selecting the number of commit easier than your head. `git rebase -i HEAD~8`
![git-rewrite-history-03]({{ site.url }}/assets/2016-05-01-changing-history-to-save-it/git-rewrite-history-03.png)

As you can read, the squash option is the letter s. So you need to select all your commits which will be squashed to the first one. As our first commit is the `7ce4c01`, we need to squash all the commits from it until the one which changed the keys, which is the commit `12e77ba`. To do it, it’s just to change the work pick to s from the lines 2 to 5 (you don’t change the first one as you are going to squash the next commits into it). To change, I just ran the command `:2,5s/pick/s` in vim. So now it looks like this:
![git-rewrite-history-04]({{ site.url }}/assets/2016-05-01-changing-history-to-save-it/git-rewrite-history-04.png)

Once you save and quit from vim, it shows a screen with the final commit messages:
![git-rewrite-history-05]({{ site.url }}/assets/2016-05-01-changing-history-to-save-it/git-rewrite-history-05.png)

Again, save and quit vim, and the rebase is going to be completed.
![git-rewrite-history-06]({{ site.url }}/assets/2016-05-01-changing-history-to-save-it/git-rewrite-history-06.png)

We can check we don’t the commits anymore in history, and the first commit has the right value:
![git-rewrite-history-07]({{ site.url }}/assets/2016-05-01-changing-history-to-save-it/git-rewrite-history-07.png)
![git-rewrite-history-08]({{ site.url }}/assets/2016-05-01-changing-history-to-save-it/git-rewrite-history-08.png)

Then it’s only a matter of forcing a push to the remote with `git push —force`. After that I was thinking how much damage one can do with this, either on purpose or not.
But then I remembered that we have here and you can set policies to avoid people forcing pushes and a good backup policy 😃

Cheers
