---
layout: post
title: Did you mess up with git commit? No Worry!
subtitle: Let's go back to where it works
tags: [git]
comments: true
---

Everytime I learn about `git`, I frequently was likely to focus on adding/updating files. Because of that, I went through silly and stupid procedure or left those stupid whenever I did make mistakes. 

It is NOW time for us to learn smart ways to revert it!

### Scenario 1; Wrong Commit Message? No Problem!
It it really common that you forget adding more files or put a bad message to a commit that you just made. 
What would you do in this case? Are you going to leave it as it is or do you want to fix that?
I hope that you want to do the latter. Here are steps you need to follow to do so.

``` bash
(master)[you@machine]$>git commit --amend -m "<new message>"
```

{: .box-warning}
**Warning:** This commint will create a new commit number.

### Scenario 2; Made commit to a wrong branch? No Problem!
Imagine that you made a below commit to `master` branch. However, you figured out that this commit was supposed to belong to `foo` branch.

``` bash
(master)[you@machine]$>git add test.c
(master)[you@machine]$>git commit -m "Completed new foo feature"
```
Ouch! you noticed the branch name is `master` instead of `foo`. Don't worry. It will cover.

``` bash
(master)[you@machine]$>git branch
```
The above command will show you two branches; `master` and `foo`. Okay. `foo` is our final destination. In addition, we need a commit number that we want move to `foo`. 

``` bash
(master)[you@machine]$>git log
```

The above command will show you a bunch of commit log and copy/paste the commit number.

``` bash
(master)[you@machine]$>git checkout foo
```

It enables you to switch your current branch to `foo`.

``` bash
(foo)[you@machine]$>git cherry-pick <the commit number>
```

Okay. Here we go. `foo` branch, now, has that commit.

But! we should fix our `master` branch as well.

``` bash
(foo)[you@machine]$>git checkout master
```

``` bash
(master)[you@machine]$>git reset --<soft or mix or hard> <the commit number>
```
* `soft` : `soft` will reset the commit, but changes will remain in working stage.
* `mix`  : `mix` is the default. `mix` will reset the commit, but changes will remain in working directory.
* `hard` : `hard` will reset the commit, but changes will NOT remain anywhere.
