---
layout: page
title: Git
description: >
    Notes I keep for setting up Git and GitHub.
hide_description: true
sitemap: false
---

I keep these steps for setting up Git and GitHub. I learned them from [The Odin Project][odin].

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Git
I use `git` to keep track of changes to my files. I think of it as a bit like a save-history shortcut, though it does more than `CTRL` + `S`.

On Debian or Ubuntu, I can install it with:

```bash
sudo apt install git
```

```bash
git config --global user.name "jcnccsr"
git config --global user.email jcnccsr@gmail.com
```

I can check the values I set with:
```bash
git config --get user.name
git config --get user.email
```

To ignore macOS `.DS_Store` files globally, I can run:

```bash
echo .DS_Store >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

## GitHub
I set `main` as the default branch for new repositories with:

```bash
git config --global init.defaultBranch main
```

## SSH

I check whether I already have an ED25519 SSH public key:

```bash
ls ~/.ssh/id_ed25519.pub
```

If that file is missing, I can create a new SSH key:

```bash
ssh-keygen -t ed25519
```

I can create a key with an email comment like this, then add its public key to GitHub:
```bash
ssh-keygen -t ed25519 -C "jcnccsr@gmail.com"
```

I can test the SSH connection to GitHub with:
```bash
ssh -T git@github.com
```

[odin]: https://www.theodinproject.com/lessons/foundations-setting-up-git