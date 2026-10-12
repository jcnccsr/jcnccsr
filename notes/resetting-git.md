---
layout: page
title: Resetting A Repository
description: >
  Notes I keep for resetting a Git repository.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Starting With an Existing Repository

If I do not have a local copy yet, I can clone the repository:

```bash
git clone <remote repository>
cd <repository>
```

I can check which remote URL is configured with:

```bash
git remote -v
```

## Removing and Reinitializing Git History

If I really intend to remove all Git history, I can remove the `.git` directory:

```bash
rm -rf .git
```

Then I can initialize a new repository:

```bash
git init
```

## Reconnecting the Repository to GitHub

I get the repository URL from GitHub and add it as the `origin` remote:

```bash
git remote add origin <url>
```

I stage and commit the files before pushing:

```bash
git commit -m "Reset repository"
```

## Force-Pushing to GitHub

I use this only when I intend to replace the remote branch history:

```bash
git push --force origin main
```

