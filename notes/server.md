---
layout: page
title: Raspberry Pi Setup
description: >
    Notes I keep for myself while learning to set up and administer a Raspberry Pi as a Linux server.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What I Need to Know

A Raspberry Pi is a small single-board computer capable of running a full operating system. I can use it as a desktop computer, a development machine, or a server.

For my projects, I want to use it as a **headless Linux server**. This means that I don't need a monitor, keyboard, or mouse connected to it during normal operation.

I will manage it remotely from my Mac using SSH.

My Raspberry Pi 4 will eventually host my Cursus learning platform and its database.

The general arrangement is:

- **Mac Mini:** Where I write, test, and develop my applications.
- **Raspberry Pi:** Where I deploy and run my applications.
- **Git:** How I transfer and manage my source code.
- **Cloudflare Tunnel:** How I access my services remotely without opening incoming ports on my router.

## 1. What I Need

Before setting up the Raspberry Pi, I need:

- Raspberry Pi 4.
- A suitable USB-C power supply (5V, 3A).
- A microSD card. I prefer at least 32 GB.
- An Ethernet cable or Wi-Fi connection.
- A Mac or another computer to prepare the operating system.

For a server, I don't need a desktop environment. A command-line-only operating system is sufficient.

## 2. Installing Raspberry Pi OS

I use **Raspberry Pi Imager**, the official tool for installing an operating system onto a microSD card.

Website:

https://www.raspberrypi.com/software/

I download and install Raspberry Pi Imager on my Mac.

### Choosing the operating system

For a headless server, I choose:

**Raspberry Pi OS Lite (64-bit)**

It is based on Debian Linux and does not include a graphical desktop environment.

This is useful because:

- It uses fewer system resources.
- It has fewer unnecessary applications.
- I can administer everything through the terminal.
- It provides access to Debian packages and development tools.

### Writing the operating system

1. Insert the microSD card into my Mac.
2. Open Raspberry Pi Imager.
3. Select Raspberry Pi 4 as the device.
4. Select Raspberry Pi OS Lite (64-bit).
5. Select the microSD card as the storage device.
6. Open the operating system customisation settings.

**Important:** Selecting the storage device incorrectly can erase another disk. I must double-check before writing.

### Configuring the operating system

Before writing the image, I configure the following settings.

**General settings:**

- Hostname: `cursus-pi`
- Username: A username I choose.
- Password: A strong, unique password.
- Wi-Fi: My network name and password, if I am not using Ethernet.
- Time zone: `Asia/Dubai`.
- Keyboard layout: My preferred layout.

**Services:**

- Enable SSH.
- Choose public-key authentication if I have already configured an SSH key on my Mac.

I then write the operating system to the microSD card.

## 3. Connecting to the Raspberry Pi

After writing the operating system:

1. Safely eject the microSD card.
2. Insert it into the Raspberry Pi.
3. Connect Ethernet if available, or use the configured Wi-Fi.
4. Connect the power supply.
5. Give the Raspberry Pi a little time to boot.

I can now connect to it from my Mac.

### Connecting through SSH

SSH stands for Secure Shell. It allows me to access another computer's terminal over a network.

From my Mac terminal:

```bash
ssh username@cursus-pi.local
```

I replace `username` with the account name I configured in Raspberry Pi Imager.

The `.local` address uses local network name discovery. If it does not resolve, I can find the Raspberry Pi's IP address through my router and connect using that instead:

```bash
ssh username@192.168.1.100
```

The IP address above is only an example.

If this is my first connection, SSH may ask me to confirm the server's host key. I should verify that I am connecting to my own Raspberry Pi before accepting it.

Once connected, I am using the Raspberry Pi's terminal remotely.

## 4. Understanding the Terminal

Some basic Linux commands I want to remember:

| Command | Purpose |
|---|---|
| `pwd` | Shows my current directory |
| `ls` | Lists files and directories |
| `ls -la` | Lists files, including hidden ones |
| `cd directory` | Changes directory |
| `cd ..` | Moves to the parent directory |
| `mkdir name` | Creates a directory |
| `touch file` | Creates an empty file |
| `cp source destination` | Copies a file |
| `mv source destination` | Moves or renames a file |
| `rm file` | Deletes a file |
| `cat file` | Displays file contents |
| `less file` | Reads a file page by page |
| `nano file` | Opens a simple terminal text editor |
| `clear` | Clears the terminal |

I can use `sudo` to execute commands with administrative privileges.

For example:

```bash
sudo apt update
```

I should not use `sudo` unnecessarily. I only need elevated privileges for operations that require them.

## 5. Updating the System

One of the first things I should do after installing Linux is update the package lists and installed software.

```bash
sudo apt update
sudo apt full-upgrade -y
```

- `apt update` retrieves information about available packages.
- `apt full-upgrade` installs available updates and handles dependency changes.
- `-y` automatically answers yes to confirmation prompts.

After updating, I can reboot:

```bash
sudo reboot
```

My SSH connection will disconnect because the Raspberry Pi is restarting. I can reconnect after it boots.

I should periodically repeat the update commands to receive security fixes and bug fixes.

## 6. Checking the System

I can use these commands to inspect my Raspberry Pi.

### Operating system

```bash
cat /etc/os-release
```

Displays information about the installed Linux distribution.

### Processor architecture

```bash
uname -m
```

Common results:

- `aarch64`: 64-bit ARM architecture.
- `armv7l`: 32-bit ARM architecture.

### Kernel information

```bash
uname -a
```

Displays information about the Linux kernel and system.

### Memory

```bash
free -h
```

Displays available and used RAM.

### Storage

```bash
df -h
```

Displays disk space usage.

### Network information

```bash
hostname -I
```

Displays the Raspberry Pi's assigned IP addresses.

### Running processes

```bash
top
```

Displays running processes and resource usage.

I can press `q` to exit.

## 7. Installing Useful Development Tools

I don't need to install everything immediately. I can start with a small collection of useful tools.

```bash
sudo apt install -y \
    git \
    curl \
    wget \
    build-essential \
    gdb \
    pkg-config \
    sqlite3 \
    libsqlite3-dev \
    ufw
```

Some of these packages are especially useful for my programming projects:

| Package | Purpose |
|---|---|
| `git` | Version control |
| `curl` | Transfers data using URLs |
| `wget` | Downloads files |
| `build-essential` | Installs essential compilation tools, including GCC and Make |
| `gdb` | C/C++ debugger |
| `pkg-config` | Helps locate installed libraries and their compiler flags |
| `sqlite3` | SQLite command-line interface |
| `libsqlite3-dev` | SQLite development headers and library |
| `ufw` | Simple firewall management |

I can verify that GCC is installed:

```bash
gcc --version
```

And that Git is installed:

```bash
git --version
```

I can also check SQLite:

```bash
sqlite3 --version
```

## 8. Setting Up SSH Keys

SSH keys allow me to authenticate without repeatedly entering my account password.

SSH uses a pair of keys:

- **Private key:** I keep this secret on my Mac.
- **Public key:** I can safely copy this to computers I want to access.

If I did not configure an SSH key during installation, I can create one on my Mac:

```bash
ssh-keygen -t ed25519 -C "mac-mini"
```

I can accept the default file location and optionally protect the private key with a passphrase.

My keys will normally be stored here:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

I can display my public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

I can install this public key on my Raspberry Pi using:

```bash
ssh-copy-id username@cursus-pi.local
```

If `ssh-copy-id` is not installed on my Mac, I can add the public key manually to the Raspberry Pi's `~/.ssh/authorized_keys` file.

After confirming that key-based authentication works, I can consider disabling password authentication for SSH.

I should never share or upload my private key.

## 9. Basic Firewall Configuration

A firewall controls which network connections are allowed to reach my computer.

I can use UFW (Uncomplicated Firewall) to configure basic rules.

First, I allow SSH:

```bash
sudo ufw allow OpenSSH
```

Then I configure the default policies:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Finally, I enable the firewall:

```bash
sudo ufw enable
```

I can check its status:

```bash
sudo ufw status verbose
```

The important thing is to allow SSH before enabling the firewall. Otherwise, I could accidentally lock myself out.

For my Cloudflare Tunnel setup, I do not need to open incoming HTTP or HTTPS ports on my router. The tunnel establishes an outbound connection to Cloudflare.

I should avoid exposing SSH directly to the public internet.

## 10. Organizing My Projects

I want to keep my applications organized rather than placing everything randomly throughout the filesystem.

I can create a directory for my applications inside my home directory:

```bash
mkdir -p ~/apps
```

For example:

```text
/home/username/
    apps/
        cursus/
        other-project/
    .ssh/
```

I can keep my development and deployment workflows separate.

My preferred workflow is:

1. Develop on my Mac Mini.
2. Test the application locally.
3. Commit changes to my private GitHub repository.
4. Pull the changes onto the Raspberry Pi.
5. Run the application on the Raspberry Pi.

This allows me to use my Mac as my main development machine while the Raspberry Pi handles hosting.

## 11. Preparing for Cursus

Eventually, I want the Raspberry Pi to host my personal programming-learning platform.

The planned components are:

| Component | Purpose |
|---|---|
| Linux | Operating system |
| Git | Source code management |
| Node.js | Running the web application |
| SQLite | Storing user progress and application data |
| Cloudflare Tunnel | Remote connectivity |
| Cloudflare Access | Authentication and access control |

I should keep a few things separate:

**Source code**

My application code belongs in Git.

**Database**

My SQLite database should be stored separately from the source code, so deploying new code does not overwrite my data.

**Secrets**

Passwords, API keys, authentication secrets, and other sensitive configuration should never be committed to Git.

**Backups**

I should periodically back up my database and other important application data somewhere separate from the Raspberry Pi's microSD card.

I do not need to install all these components immediately. I can add them as the project requires them.

## 12. Useful Commands for Daily Administration

These are commands I expect to use regularly.

| Command | Purpose |
|---|---|
| `sudo apt update` | Refresh package information |
| `sudo apt full-upgrade` | Update installed packages |
| `sudo apt install package` | Install a package |
| `sudo apt remove package` | Remove a package |
| `sudo reboot` | Restart the Raspberry Pi |
| `sudo shutdown -h now` | Shut down the Raspberry Pi |
| `systemctl status service` | Check a service |
| `systemctl restart service` | Restart a service |
| `journalctl -u service` | View a service's logs |
| `df -h` | Check storage |
| `free -h` | Check memory |
| `htop` | View processes interactively (if installed) |

I can use `systemctl` to manage services that run in the background.

For example:

```bash
sudo systemctl status ssh
```

This shows whether the SSH service is running.

## 13. Mistakes I Want to Watch For

- I might forget to update the system after installation.
- I might expose services to the public internet unnecessarily.
- I might enable a firewall before allowing SSH.
- I might confuse my Mac's terminal with the Raspberry Pi's terminal.
- I might accidentally run commands with `sudo` when they are not needed.
- I might store passwords or API keys in my Git repository.
- I might keep my database inside a directory that gets overwritten during deployment.
- I might forget to back up important data.
- I might assume that a successful SSH connection means every other service is also configured correctly.

## Reminders to Myself

1. Raspberry Pi OS is based on Debian, so I can use many familiar Linux commands and Debian packages.
2. A headless Raspberry Pi does not need a monitor, keyboard, or mouse during normal operation.
3. SSH allows me to administer the Raspberry Pi remotely.
4. I should keep my operating system and installed packages updated.
5. I should use SSH keys and protect my private key.
6. A firewall helps control incoming connections.
7. I should avoid exposing services directly to the public internet when I can use Cloudflare Tunnel.
8. My Mac is my development machine; my Raspberry Pi is my hosting machine.
9. My source code, database, and secrets should have separate storage and management.
10. I should keep the system simple and install additional software only when I need it.