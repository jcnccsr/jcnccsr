---
layout: page
title: Git Basics
description: >
    Notes I keep for myself while learning Git and version control.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What is Git?

Git is a version control system.

It allows me to keep track of changes made to files over time. I can see what changed, compare versions, restore previous versions, and work on different versions of a project.

For example, imagine I'm working on a C program.

Today, the program works. Tomorrow, I make some changes, but accidentally introduce a bug.

Without version control, I might have to manually undo everything.

With Git, I can inspect my previous changes and restore an earlier version of my work.

Git is useful for:

- Tracking changes to source code.
- Keeping a history of a project.
- Reverting mistakes.
- Experimenting without damaging the main version.
- Working on different features independently.
- Collaborating with other developers.
- Sharing projects through platforms such as GitHub.

### Git versus GitHub

These are two different things:

| Git | GitHub |
|---|---|
| Version control software | Online hosting platform |
| Runs locally on my computer | Hosts remote repositories |
| Tracks changes and history | Makes repositories accessible online |
| Works without an internet connection | Requires a network connection for remote operations |

GitHub is not the only hosting platform. GitLab and Codeberg are other examples.

## Installing Git

On macOS, I can install Git through Homebrew:

```bash
brew install git
```

On Debian-based Linux systems:

```bash
sudo apt update
sudo apt install git
```

Check the installation:

```bash
git --version
```

## Configuring Git

Before using Git, I should configure my identity.

```bash
git config --global user.name "My Name"
git config --global user.email "my@email.com"
```

These settings identify the author of commits.

The `--global` option means that these settings apply to all my repositories unless I override them in a particular repository.

I can inspect my configuration:

```bash
git config --list
```

I can also configure the default branch name:

```bash
git config --global init.defaultBranch main
```

This tells Git to use `main` as the default branch when initializing new repositories.

## Creating a repository

A repository is a project managed by Git.

Let's create a small project:

```bash
mkdir my-project
cd my-project
```

Initialize Git:

```bash
git init
```

Git creates a hidden directory called `.git`.

This directory contains the repository's history, configuration, and other internal information.

I should not manually modify its contents.

I can check the repository's status:

```bash
git status
```

## The three main areas of Git

Understanding these three areas is fundamental to using Git.

1. Working directory
2. Staging area
3. Repository

### Working directory

This is where I create and modify files.

For example:

```bash
touch main.c
```

At this point, Git knows that a new file exists, but it has not been included in a commit.

### Staging area

The staging area is where I prepare changes that I want to include in my next commit.

I can add a file:

```bash
git add main.c
```

I can add all changes in the current directory:

```bash
git add .
```

### Repository

Once I commit my staged changes, Git saves a snapshot in the repository's history.

```bash
git commit -m "Add main.c"
```

A commit is a recorded snapshot of the staged changes.

The general workflow looks like this:

```text
Working directory
       |
       | git add
       v
 Staging area
       |
       | git commit
       v
   Repository
```

This is the basic Git workflow I will use constantly.

## Making changes and committing

Let's create a simple C program:

```c
#include <stdio.h>

int main(void)
{
    printf("Hello, Git!\n");
    return 0;
}
```

Save it as `main.c`.

Add it to the staging area:

```bash
git add main.c
```

Commit it:

```bash
git commit -m "Add initial C program"
```

The message describes what this commit contains.

I should write meaningful commit messages rather than vague ones such as:

```text
changes
stuff
update
test
```

Better examples:

```text
Add player movement
Fix pointer arithmetic bug
Create initial project structure
Update README documentation
```

## Inspecting changes

Git provides several commands for inspecting the state and history of a project.

### git status

```bash
git status
```

Shows which files have been modified, staged, or are untracked.

This is one of the commands I should use frequently.

### git diff

```bash
git diff
```

Shows changes in my working directory that have not yet been staged.

For example, if I modify `main.c`, I can see exactly what changed.

### git diff --staged

```bash
git diff --staged
```

Shows changes that are already staged and will be included in the next commit.

### git log

```bash
git log
```

Displays the commit history.

A shorter version:

```bash
git log --oneline
```

This displays commits in a compact format.

I can also see a graph of branches and commits:

```bash
git log --oneline --graph --all
```

### git show

```bash
git show
```

Displays information about the most recent commit, including its changes.

I can also inspect a particular commit by providing its hash:

```bash
git show COMMIT_HASH
```

## Understanding commits

Each commit represents a snapshot of the project's tracked state.

Every commit has:

- A unique identifier (hash).
- An author.
- A date.
- A commit message.
- A reference to its parent commit or commits.

Git uses these relationships to construct the project's history.

For example:

```text
A --- B --- C --- D
```

Each letter represents a commit.

Commit D is the latest commit in this simple history.

I can move between different points in the project's history, compare them, and create new work based on an earlier commit.

## Undoing changes

Sometimes I make mistakes. Git provides several ways to undo them.

I should understand the difference between these commands before using them.

### Discarding unstaged changes

```bash
git restore main.c
```

This restores the file to its last committed state, discarding its unstaged modifications.

**Warning:** This can permanently discard work that I have not saved elsewhere.

To unstage a file without discarding its modifications:

```bash
git restore --staged main.c
```

This removes it from the staging area but keeps the changes in the working directory.

### Amending the latest commit

Suppose I forgot to include a file in my latest commit.

I can stage the missing file:

```bash
git add missing_file.c
```

Then update the latest commit:

```bash
git commit --amend
```

This replaces the previous commit with a new one.

I can also update the commit message.

I should be careful when amending commits that have already been shared with others.

### Reverting a commit

If I want to undo the changes introduced by a particular commit without rewriting history, I can use:

```bash
git revert COMMIT_HASH
```

Git creates a new commit that reverses the changes.

This is generally safer for commits that have already been pushed and shared.

### Resetting

Git also has `reset`, which moves the current branch reference to another commit.

For example:

```bash
git reset --soft HEAD~1
```

Moves the current branch back one commit but keeps the changes staged.

```bash
git reset --mixed HEAD~1
```

Moves the branch back one commit and leaves the changes unstaged. This is the default reset mode.

```bash
git reset --hard HEAD~1
```

Moves the branch back one commit and discards changes in the working directory and staging area.

**Warning:** `--hard` can destroy uncommitted work. I should not use it casually.

For now, I should become comfortable with `restore`, `revert`, and `status` before experimenting with reset.

## Branches

A branch allows me to work on a separate line of development.

For example, imagine I have a working game and want to experiment with adding a new feature.

Instead of modifying the main version directly, I can create a branch.

### Listing branches

```bash
git branch
```

Displays local branches.

### Creating a branch

```bash
git branch player-movement
```

Creates a branch called `player-movement`.

### Switching branches

```bash
git switch player-movement
```

Switches to that branch.

### Creating and switching in one command

```bash
git switch -c player-movement
```

This creates the branch and switches to it.

### Returning to the main branch

```bash
git switch main
```

### Deleting a branch

After finishing and merging a feature, I can delete its branch:

```bash
git branch -d player-movement
```

The `-d` option deletes a branch only when Git considers it safely merged.

I can force-delete a branch with `-D`, but I should be careful because it may contain work I still need.

## Merging branches

Once I finish working on a feature, I can merge it into another branch.

For example, I have:

```text
main
  |
  A --- B

player-movement
          \
           C --- D
```

I want to merge `player-movement` into `main`.

First, switch to `main`:

```bash
git switch main
```

Then merge:

```bash
git merge player-movement
```

Git combines the changes from the feature branch into the current branch.

If everything goes well, the merge completes automatically.

### Merge conflicts

Sometimes Git cannot automatically combine changes.

This is called a merge conflict.

For example, two branches might modify the same lines of a file differently.

Git may insert conflict markers:

```text
<<<<<<< HEAD
printf("Hello!\n");
=======
printf("Greetings!\n");
>>>>>>> player-movement
```

I must edit the file and decide which changes to keep.

After resolving the conflict:

```bash
git add main.c
git commit
```

The exact final step can vary depending on the merge operation, but this is the usual workflow for completing a conflicted merge.

## Remote repositories

So far, everything has happened locally on my computer.

A remote repository is another copy of a Git repository, usually hosted on a server such as GitHub.

Remotes allow me to share my work and synchronize changes between computers.

### Adding a remote

Suppose I create a repository on GitHub.

GitHub provides a repository URL.

I can connect my local repository to it:

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

Here:

- `remote` manages remote repositories.
- `add` adds a remote.
- `origin` is the conventional name for the remote.
- The URL identifies the remote repository.

### Inspecting remotes

```bash
git remote -v
```

Displays configured remote repositories and their URLs.

### Pushing changes

To upload my local commits to GitHub:

```bash
git push -u origin main
```

The `-u` option sets the upstream relationship between my local branch and the remote branch.

After this initial push, I can usually just use:

```bash
git push
```

### Pulling changes

To download changes from the remote repository and integrate them into my current branch:

```bash
git pull
```

Pull generally performs a fetch followed by an integration step (usually a merge, unless configured otherwise).

### Fetching changes

```bash
git fetch
```

Downloads information about remote changes without integrating them into my current branch.

This allows me to inspect what has changed before deciding to merge or rebase.

### Cloning a repository

If I want to download an existing repository:

```bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

Git creates a local copy of the repository, including its history.

## The .gitignore file

Sometimes I don't want Git to track certain files.

For example:

- Compiled programs.
- Object files.
- Temporary files.
- Build directories.
- Dependency directories.
- Local configuration files containing secrets.

I can create a file named `.gitignore` in the root of my repository.

Example:

```gitignore
# Compiled C programs
*.o
main

# Temporary files
*.tmp
*.log

# Build directory
build/

# Node.js dependencies
node_modules/

# Environment variables and secrets
.env
```

Git will ignore matching untracked files.

An important detail: `.gitignore` does not automatically stop tracking files that Git already tracks. If a file is already tracked, I must remove it from the index separately.

For example:

```bash
git rm --cached .env
```

This removes the file from Git's tracking while keeping the local file.

I should never commit passwords, API keys, or other secrets.

## Tags

Tags allow me to mark specific commits.

They are often used to identify software releases.

For example:

```bash
git tag v1.0.0
```

Creates a lightweight tag at the current commit.

To create an annotated tag with a message:

```bash
git tag -a v1.0.0 -m "First stable release"
```

List tags:

```bash
git tag
```

Push a tag to a remote:

```bash
git push origin v1.0.0
```

Push all local tags:

```bash
git push --tags
```

## A typical Git workflow

Here is a workflow I expect to use regularly.

### 1. Check the repository

```bash
git status
```

### 2. Make changes

Edit my source code and create or modify files.

### 3. Inspect the changes

```bash
git diff
```

### 4. Stage the changes

```bash
git add .
```

### 5. Inspect the staged changes

```bash
git diff --staged
```

### 6. Commit

```bash
git commit -m "Add player movement"
```

### 7. Push to GitHub

```bash
git push
```

The basic cycle is:

```text
Modify files
     |
     v
  git add
     |
     v
 git commit
     |
     v
  git push
```

I should remember that `git push` is only needed when I want to send my commits to a remote repository. Local commits do not require an internet connection.

## Useful Git commands reference

| Command | Purpose |
|---|---|
| `git init` | Initialize a repository |
| `git clone URL` | Clone a repository |
| `git status` | Check repository status |
| `git add FILE` | Stage a file |
| `git add .` | Stage changes under the current directory |
| `git commit -m "message"` | Create a commit |
| `git log --oneline` | Display commit history |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |
| `git show` | Inspect a commit |
| `git restore FILE` | Discard unstaged changes |
| `git restore --staged FILE` | Unstage a file |
| `git revert HASH` | Create a commit that reverses another commit |
| `git branch` | List branches |
| `git switch BRANCH` | Switch branches |
| `git switch -c BRANCH` | Create and switch to a branch |
| `git merge BRANCH` | Merge a branch into the current branch |
| `git branch -d BRANCH` | Delete a merged branch |
| `git remote -v` | List remote repositories |
| `git fetch` | Download remote information |
| `git pull` | Fetch and integrate remote changes |
| `git push` | Upload local commits |
| `git tag` | List tags |
| `git stash` | Temporarily save uncommitted changes |

## A few habits worth developing

1. **Check `git status` frequently.** It tells me what Git sees.
2. **Make small commits.** Each commit should represent a reasonably coherent change.
3. **Write meaningful commit messages.** They help me understand the history later.
4. **Inspect changes before committing.** `git diff` and `git diff --staged` are useful.
5. **Use branches for experiments and features.** I don't need to risk breaking the main branch.
6. **Keep secrets out of repositories.** Especially passwords, tokens, and private keys.
7. **Don't blindly execute destructive commands.** Understand `reset --hard`, `restore`, and `clean` before using them.
8. **Push regularly.** A remote repository gives me another copy of my committed work, although it is not a complete backup strategy by itself.

## My current understanding

Git keeps track of the history of my project.

I modify files in my working directory, stage the changes with `git add`, and record them with `git commit`.

Branches allow me to work on different features independently. Merging combines those changes, while remotes allow me to synchronize my work with platforms such as GitHub.

I don't need to memorize every Git command. I should understand the basic workflow and learn additional commands when I need them.

The most important commands to become comfortable with are:

```bash
git status
git add
git commit
git diff
git log
git switch
git merge
git pull
git push
```

These will take me a long way.