---
layout: post
title: "why i squash commits"
description: ""
category: 
tags: []
---

When I was introduced to the concept of squashing commits, I had a difficult
time accepting it. My only experience with git at the time had been using it as a commit history
time machine. I didn't like the squash workflow because it reduced my time machine's
granularity. Who would want that?

I wasn't committing enough. My commits were a collection of changes with vague
messages, rather than concise changes with descriptive, contextual-providing
messages. Squashing the kinds of commits I had been making, wouldn't help
anyone.

As you're working on a feature or bug, you should be committing pretty
frequently. For example, if you're changing the name of a class's instance
variable, make that its own commit.

By the time your objective is complete, you
should have a bunch of commits like this. I like to leave those commits
un-squashed when I submit my pull request. I feel it helps reviewers follow your
train of thought. Once I have everyone is in agreement that the code is ready to
be rebased and merged, I squash the commits, rebase them, and then merge them.

I squash commits at this point because once my changes are merged, my coworkers
will inevitably look at the log and when that happens, they don't want to see
the twenty,
low-level commits I made to complete the overarching objective. They want to
see one commit, encapsulating that overall change. The smaller commits which
were made to complete the overall goal are now unimportant to us. We either want
the changed section of code as a single bundle, or we don't, in which case we
only need to revert one commit.
