---
layout: page
title: Setting Up Development Environment
description: >
  Notes I keep for setting up my development environment.
hide_description: true
sitemap: false
---

These are the setup steps and tools I tend to use. I keep them here so I can repeat the setup when I need to.

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## How I Set Up a Terminal

One thing I like about learning programming is that I can do it in different places, as long as I have a computer. For me, that can mean a PC or laptop, and sometimes even a phone.

### On Android

On Android, I can install Termux through [F-Droid][fdroid].

### On Windows

On Windows, I can open PowerShell and run:

```bash
wsl --install
```

Then I can install Debian:

```bash
wsl --install -d Debian
```

### On Mac

On Mac, I like [iTerm2][iterm2], so I use it instead of the default terminal. I install Homebrew first:

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Homebrew prints the next steps for the shell. They usually look like this:

```zsh
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

I can check that it is installed with:

```zsh
brew --version
```

Then I install iTerm2:

```zsh
brew install --cask iterm2
```
## How I Set Up a Unix Environment

Once my terminal is ready, these are the Linux setup commands I use:

```bash
sudo apt update && sudo apt upgrade -y
```

I install a package needed for later steps:

```bash
sudo apt update && sudo apt install -y software-properties-common
```

Then I install some tools:
```bash
sudo apt update && sudo apt install -y build-essential git wget curl
```

I can then install [Neovim][neovim]:

```bash
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz
sudo rm -rf /opt/nvim-linux-x86_64
sudo tar -C /opt -xzf nvim-linux-x86_64.tar.gz
```

I add this to my shell config (`.bashrc` or `.zshrc`):

```bash
export PATH="$PATH:/opt/nvim-linux-x86_64/bin"
```

I can make Neovim my default Vim with:

```bash
sudo update-alternatives --install /usr/bin/vi vi /usr/bin/nvim 60 && sudo update-alternatives --install /usr/bin/vim vim /usr/bin/nvim 60 && sudo update-alternatives --set vi /usr/bin/nvim && sudo update-alternatives --set vim /usr/bin/nvim
```

I use NvChad to get started with a Neovim configuration:

```bash
git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
```

After lazy.nvim finishes downloading plugins, I run `:MasonInstallAll` and `:TSInstallAll`.
I then delete the `.git` directory from the nvim folder.

To update it, I run:

```bash
Lazy sync
```

I can install Oh My Bash with:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```
I like the `mairan` theme.

On iTerm2, I install Oh My Zsh with:
```zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

I like the `fino` theme.

### Running Jekyll

To install this repository's dependencies, I run:

```bash
bundle install
```

Then I start a local development server:

```bash
bundle exec jekyll serve
```

[fdroid]: https://f-droid.org/en/
[iterm2]: https://iterm2.com/ 
[neovim]: https://neovim.io/doc/install/

