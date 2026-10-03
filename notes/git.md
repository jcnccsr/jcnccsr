---
layout: page
title: Git
description: >
    This chapter shows how to install and set up git and GitHub.
hide_description: true
sitemap: false
---

This chapter shows how to install and set up git and GitHub. I got this how-to info from [Odin's Project][odin].

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Git
I use `git`. It acts like a magical `CTRL` + `S` keyboard shortcut.

To install:

```bash
sudo apt install git
```

```bash
git config --global user.name "jcnccsr"
git config --global user.email jcnccsr@gmail.com
```

To verify:
```bash
git config --get user.name
git config --get user.email
```

For ignore those `.DS_Store` files in Mac,

```bash
echo .DS_Store >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

## GitHub
Then set the default branch to `main`:

```bash
git config --global init.defaultBranch main
```

## SSH

Check if ED25519 algorithm SSH key is already installed:

```bash
ls ~/.ssh/id_ed25519.pub
```

If empty, create a new SSH key:

```bash
ssh-keygen -t ed25519
```

Output the `.pub` for GitHub:
```bash
ssh-keygen -t ed25519 -C "jcnccsr@gmail.com"
```

Attempt to ssh to GitHub:
```bash
ssh -T git@github.com
```

[odin]: https://www.theodinproject.com/lessons/foundations-setting-up-git