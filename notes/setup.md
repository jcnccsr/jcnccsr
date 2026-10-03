---
layout: page
title: Setting Up Development Environment
description: >
  Setting up the development environment.
hide_description: true
sitemap: false
---

Setting up development environment, the way I like it.

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Setting Up the Terminal

The good thing about learning programming is it's a hobby that I can do anywhere, anytime: Be at bed, desk, or outside, I just need a computer with me. By computer, I don't necessarily mean a PC or laptop objects. Yes, I do mean that, but I also include mobile phones.

### On Android

Install Termux. This requires [F-Droid][fdroid].

### On Windows

On Windows, open PowerShell and run:

```bash
wsl --install
```

Then install Debian:

```bash
wsl --install -d Debian
```

### On Mac

I like [iTerm2][iterm2], so I use it instead of the default terminal. I install Homebrew first:

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Homebrew will print instructions, usually like this:

```zsh
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

Check if it's installed:

```zsh
brew --version
```

Install iTerm2.

```zsh
brew install --cask iterm2
```
## Setting Up Unix

Now that the terminal is ready, let's ready up Linux:

```bash
sudo apt update && sudo apt upgrade -y
```

Install some packages for later steps:

```bash
sudo apt update && sudo apt install -y software-properties-common
```

Install the tools:
```bash
sudo apt update && sudo apt install -y build-essential git wget curl
```

Then install [Neovim][neovim]:

```bash
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz
sudo rm -rf /opt/nvim-linux-x86_64
sudo tar -C /opt -xzf nvim-linux-x86_64.tar.gz
```

Then add this to the shell config (.bashrc, .zshrc):

```bash
export PATH="$PATH:/opt/nvim-linux-x86_64/bin"
```

Make Neovim the default vim:

```bash
sudo update-alternatives --install /usr/bin/vi vi /usr/bin/nvim 60 && sudo update-alternatives --install /usr/bin/vim vim /usr/bin/nvim 60 && sudo update-alternatives --set vi /usr/bin/nvim && sudo update-alternatives --set vim /usr/bin/nvim
```

Install NvChad, Yes I know I'm lazy:

```bash
git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
```

Run `:MasonInstallAll` and `:TSInstallAll` command after lazy.nvim finishes downloading plugins.
Delete the `.git` directory from the nvim folder.

To update:

```bash
Lazy sync
```

Install Oh My Bash!

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```
I like the `mairan` theme.

On iTerm2,
```zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

I like the `fino` theme.

### Running Jekyll

The install the repo:

```bash
bundle install
```

Then run a dev instance:

```bash
bundle exec jekyll serve
```

[fdroid]: https://f-droid.org/en/
[iterm2]: https://iterm2.com/ 
[neovim]: https://neovim.io/doc/install/

