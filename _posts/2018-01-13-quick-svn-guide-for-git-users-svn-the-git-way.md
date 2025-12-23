---
title: "Quick SVN guide for Git users; SVN: The Git Way"
date: 2018-01-13
layout: single
classes: wide
author_profile: false
read_time: true
excerpt: "This is a quick Subversion (SVN) guide for Git users. It helps you get started with SVN right away, the Git way."
categories:
  - version-control
  - svn
  - git
tags:
  - svn
  - git
  - version-control
  - workflow
---
Why would a Git user want to switch to SVN, you ask?

Well, sometimes you just don't have a choice. Imagine working on a project that has been maintained in SVN for a decade. "But migrating an SVN codebase to Git is not a big deal at all." But there are things like CI/CD integrations to worry about too. That isn't a really big deal either but sometimes people take "Don't fix what ain't broke." a little too seriously.

Reasons aside, having good Version Control System (Distributed VCS for that matter) concepts, I didn't want to go through SVN guides from scratch to start with. While there were plenty of resources on the web regarding SVN to Git migration, I couldn't find a quick and concise guide that would help me work with an SVN repo right away. If you are like me, you will find this article helpful. The following steps show you how you can work with SVN the Git way.

## Cloning a new repo
Checking out a repo is similar to how we do it in Git.

```bash
svn checkout <path-to-your-repo-branch> <path-to-checkout>
```

#### Example
The following checks out your code to your current working directory.

```bash
svn checkout https://mysvnrepo.com/myrepo/trunk .
```

## Creating a new topic branch
In SVN, branches (and tags) are nothing but simply a copy of one branch. A literal copy-paste of the files, unlike a pointer to commits in Git. This fact took me a while to digest and get used to.

The following commands are SVN equivalent to `git checkout -b branch`.

```bash
svn copy <path-to-a-branch> <path-for-new-branch> -m "Message"
```

#### Example
```bash
svn copy --parents https://mysvnrepo.com/myrepo/trunk https://mysvnrepo.com/myrepo/branches/feature-branch
svn switch https://mysvnrepo.com/myrepo/branches/feature-branch
```

## Working on the repo
### Adding new files
To add new files, you would use:

```bash
svn add <path-to-file>
```

As for modified files, we don't need to add them. We can commit straight away.

```bash
svn commit -m "Commit message"
```

To commit only specific files, we need to list files after the commit message.

```bash
svn commit -m "Commit message" <path-to-file-1> <path-to-file-2>
```

If we want to commit a single file, we can do the following too.

```bash
svn commit <path-to-file> -m "Commit message"
```

### Checking out new changes
The following is the SVN equivalent to `git fetch && git merge` or `git pull`.

```bash
svn update
```

### Merging your feature branch to trunk
Merging a branch in SVN is similar to how we do it in Git.

```bash
svn merge <path-to-branch-to-branch>
```

#### Example
```bash
svn update
svn switch https://mysvnrepo.com/myrepo/trunk
svn update
svn merge https://mysvnrepo.com/myrepo/branches/feature-branch
svn commit -m "Merge feature branch to trunk"
```

### Deleting feature branch after merging
To delete a feature branch (or any branch for that matter), `svn delete` is used.

#### Example
```bash
svn delete https://mysvnrepo.com/myrepo/branches/feature-branch -m "Delete feature branch after merging"
```
