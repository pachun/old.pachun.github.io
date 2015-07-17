---
layout: post
title: "git with people"
description: ""
category: 
tags: []
---
#old me

```
# change the code
git add .
git commit -am “feature description”
git push origin master
```

__git add .__ is bad [because it has unintended
consequences](https://github.com/pachun/mlp-ios-rubymotion).

__"feature description"__ sucks. [There's a standard for commit
messages](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message).

A coworker of mine used to say that to use git well, you have to
understand its underlying data structures. [That being the goal, I recommend
checking out gitx. It's a tool that visually depicts commits and
branches](http://rowanj.github.io/gitx/).

#new me

```
# create and switch to a new branch
git checkout -b feature-or-bugfix

# change the code

# run the tests and fix the failures

# stage and commit (i use gitx)
gitx


# repeat
  # make more changes
  # run the tests and fix the failures
  # stage and commit

# ABR! always be refactoring!

# show the world
git push origin feature-or-bugfix

# submit a pull request and tag some people for review

# receive and respond to feedback

# squash your commits with an interactive rebase
git rebase -i HEAD~[number of commits you made]

# rebase your changes on top of master
git checkout master
git pull --rebase
git checkout feature-or-bugfix
git rebase master

# force push your rebased branch back up
git push -f origin feature-or-bugfix

# merge to master and push new master to github
git checkout master
git merge feature-or-bugfix
git push origin master
```

I began writing explanations for each of the steps, but decided to slim it down.
_Why_ is out of this post's scope. Trying to include _why_'s for each of these steps
resulted in my first work as a novelist. Publisher pending.

You'll have to look up _how_'s for some of those steps, and _why_'s for all of
them on your own. Shut up, it's good for you. In particular, you'll probably
need some guidance doing your first interactive rebase.

Good luck and keep improving!
