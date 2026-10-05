---
layout: page
title: Resetting A Repository
description: >
  This chapter guides me on how to reset a repository.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Existing git repository

`if (!repository)`, clone it:

```bash
git clone <remote repository>
cd <repository>
```

To check the remote url of the repo:

```bash
git remote -v
```

## Resetting git repo

Remove all git history:

```bash
rm -rf .git
```

Reinitialize git:

```bash
git init
```

## Reconnect Repo to GitHub

Get and copy the repo's url, then run `remote add`:

```bash
git remote add origin <url>
```

Wrap up the local repo (`git add` & `git commit`):

```bash
git commit -m "Reset repository"
```

## Force `git push` to GitHub

```bash
git push --force origin main
```

