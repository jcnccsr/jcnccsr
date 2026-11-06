---
layout: page
title: Raspberry Pi 4 — Building My Personal Server
description: >
  A complete guide to installing Raspberry Pi OS Lite, configuring a headless
  Raspberry Pi 4, installing Docker, deploying SilverBullet, and connecting
  everything securely through Cloudflare Tunnel.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Introduction

I want to turn my Raspberry Pi 4 into a personal home server.

My first project is to host SilverBullet, an open-source Markdown-based knowledge management system similar to Obsidian.

I want to access my notes from anywhere using my own domain, without exposing my home network directly to the internet.

My intended architecture:

```text
                   My Devices
                Mac Mini / Phone
                       |
                       |
                    Internet
                       |
                       |
                  Cloudflare
                       |
                 Cloudflare Access
                       |
                Cloudflare Tunnel
                       |
                       |
                 Raspberry Pi 4
                       |
                 Docker Engine
                       |
                  SilverBullet
                       |
                 Markdown Files
```

Cloudflare Tunnel creates an outbound connection from my Raspberry Pi to Cloudflare. This means I don't need to configure port forwarding on my router or expose my home IP address.

## 1. Prepare Raspberry Pi OS

I use Raspberry Pi Imager on my Mac Mini.

Website: https://www.raspberrypi.com/software/

I choose:

- Device: Raspberry Pi 4
- Operating System: Raspberry Pi OS Lite (64-bit)
- Storage: My microSD card

I choose the Lite edition because I don't need a graphical desktop environment on a headless server.

### Configure the operating system

Before writing the image, I open the OS customization settings.

Under General:

| Setting | Value |
|---|---|
| Hostname | `knowledge-pi` |
| Username | My own username |
| Password | A strong, unique password |
| Wi-Fi | My home network |
| Timezone | My local timezone |
| Keyboard | My keyboard layout |

Under Services:

- Enable SSH.
- Select public-key authentication.

I generate an SSH key on my Mac Mini if I don't already have one:

```bash
ssh-keygen -t ed25519 -a 64 -C "mac-mini"
```

I use a passphrase to protect my private key.

My public key is located at:

```text
~/.ssh/id_ed25519.pub
```

I copy the contents of this file into Raspberry Pi Imager.

I never share my private key:

```text
~/.ssh/id_ed25519
```

After configuring everything, I write the image to the microSD card.

Official documentation:

https://www.raspberrypi.com/documentation/computers/getting-started.html

## 2. First boot and SSH connection

I insert the microSD card into the Raspberry Pi.

For a server, I prefer connecting the Pi directly to my router using an Ethernet cable. A wired connection is generally more reliable than Wi-Fi.

I connect the power supply and wait for the operating system to boot.

On my Mac Mini, I open Terminal.

I connect using:

```bash
ssh myusername@knowledge-pi.local
```

I replace `myusername` with the username configured in Raspberry Pi Imager.

If the hostname doesn't resolve, I can find the Raspberry Pi's IP address through my router's connected-device list and connect directly:

```bash
ssh myusername@192.168.1.100
```

The IP address above is only an example.

### Verify the installation

```bash
hostname
```

```bash
uname -m
```

```bash
cat /etc/os-release
```

I expect the architecture to be:

```text
aarch64
```

This confirms that I am running a 64-bit operating system.

## 3. Update the operating system

Before installing additional software, I update the system.

```bash
sudo apt update
sudo apt full-upgrade -y
```

Remove unnecessary packages:

```bash
sudo apt autoremove -y
sudo apt clean
```

Restart the Raspberry Pi:

```bash
sudo reboot
```

Reconnect through SSH.

### Install useful utilities

```bash
sudo apt install -y \
  curl \
  wget \
  git \
  neovim \
  htop \
  ca-certificates \
  gnupg \
  unattended-upgrades
```

These utilities will help me administer and maintain the server.

## 4. Establish basic security

Before deploying applications, I configure a few basic security measures.

### SSH configuration

I create a separate SSH configuration file:

```bash
sudo nvim /etc/ssh/sshd_config.d/99-hardening.conf
```

I add:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
AllowUsers myusername
```

I replace `myusername` with my actual username.

Validate the configuration:

```bash
sudo sshd -t
```

If there are no errors, reload SSH:

```bash
sudo systemctl reload ssh
```

I keep my existing SSH session open and test a second connection before closing it.

### Configure automatic security updates

```bash
sudo dpkg-reconfigure -plow unattended-upgrades
```

I enable automatic security updates.

### Configure the firewall

Install UFW:

```bash
sudo apt install ufw
```

Set default policies:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Allow SSH:

```bash
sudo ufw allow OpenSSH
```

Enable the firewall:

```bash
sudo ufw enable
```

Check the configuration:

```bash
sudo ufw status verbose
```

I do not configure router port forwarding for SSH or SilverBullet.

Note: Docker has its own networking and firewall rules. Published container ports can bypass UFW filtering, so I will bind SilverBullet only to localhost in my Docker configuration.

## 5. Configure a stable local IP address

I want the Raspberry Pi to maintain a predictable address on my home network.

The easiest approach is to configure a DHCP reservation in my router.

I find the Raspberry Pi under connected devices and reserve its current IP address.

For example:

```text
Hostname: knowledge-pi
IP:       192.168.1.100
```

This is preferable to manually configuring a static IP address on the Raspberry Pi.

I can verify the address with:

```bash
hostname -I
```

## 6. Install Docker Engine and Docker Compose

Docker allows me to run applications inside isolated containers.

Instead of installing every application directly into Raspberry Pi OS, I can deploy and manage each application independently.

I install Docker Engine using its official Debian repository instructions.

Documentation:

https://docs.docker.com/engine/install/debian/

First, install the required packages:

```bash
sudo apt update
sudo apt install ca-certificates curl
```

Create the directory for package signing keys:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Download Docker's official GPG key:

```bash
sudo curl -fsSL \
  https://download.docker.com/linux/debian/gpg \
  -o /etc/apt/keyrings/docker.asc
```

Set permissions:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Add Docker's repository:

```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

Update package lists:

```bash
sudo apt update
```

Install Docker Engine and Compose:

```bash
sudo apt install -y \
  docker-ce \
  docker-ce-cli \
  containerd.io \
  docker-buildx-plugin \
  docker-compose-plugin
```

Start Docker automatically at boot:

```bash
sudo systemctl enable --now docker
```

### Test Docker

```bash
sudo docker run hello-world
```

If everything works, Docker should download a small test image and display a confirmation message.

Check Docker Compose:

```bash
sudo docker compose version
```

I initially use `sudo` for Docker commands rather than adding my everyday account to the Docker group. Membership in that group effectively grants root-level privileges.

## 7. Prepare SilverBullet

I create a directory to hold my application configuration and persistent data.

```bash
mkdir -p ~/services/silverbullet/data
```

Enter the directory:

```bash
cd ~/services/silverbullet
```

Create a Docker Compose configuration:

```bash
nvim compose.yml
```

Add:

```yaml
services:
  silverbullet:
    image: ghcr.io/silverbulletmd/silverbullet:latest
    container_name: silverbullet
    restart: unless-stopped

    volumes:
      - ./data:/data

    ports:
      - "127.0.0.1:3000:3000"
```

The important part is:

```yaml
- "127.0.0.1:3000:3000"
```

This makes SilverBullet accessible only from the Raspberry Pi itself, rather than exposing port 3000 to the entire network.

The data directory is mounted to `/data` inside the container. This ensures that my Markdown files persist even if I replace or recreate the container.

### Start SilverBullet

```bash
sudo docker compose up -d
```

Check the container:

```bash
sudo docker compose ps
```

Check the logs:

```bash
sudo docker compose logs -f
```

I can test SilverBullet locally on the Raspberry Pi:

```bash
curl -I http://localhost:3000
```

I can also access it from my Mac Mini through an SSH tunnel:

```bash
ssh -L 3000:localhost:3000 myusername@knowledge-pi.local
```

Then I open:

```text
http://localhost:3000
```

I should now see SilverBullet.

### Configure SilverBullet authentication

I must configure authentication before making SilverBullet accessible remotely.

SilverBullet has its own authentication and account-management features.

I should not leave a space anonymously accessible.

For a simple single-user deployment, SilverBullet documents the `SB_USER` environment variable for legacy single-space authentication. For a new installation, I should evaluate its current account-managed multi-space authentication rather than assume the older mode is the preferred long-term configuration.

Documentation:

https://docs.silverbullet.md/Authentication

## 8. Connect Raspberry Pi to Cloudflare

Now comes the interesting part.

I want to access SilverBullet using a domain such as:

```text
https://notes.mydomain.com
```

I use Cloudflare Tunnel to connect my Raspberry Pi to Cloudflare.

### Prerequisites

I need:

- A Cloudflare account.
- A domain added to Cloudflare.
- The domain's DNS configured through Cloudflare.
- Internet access from the Raspberry Pi.

Cloudflare website:

https://dash.cloudflare.com/

Cloudflare Tunnel documentation:

https://developers.cloudflare.com/tunnel/get-started/

### Create a tunnel

I log in to the Cloudflare dashboard.

Navigate to:

```text
Cloudflare Dashboard
    |
    +-- Networking
          |
          +-- Tunnels
```

I select:

**Create Tunnel**

I choose Cloudflared as the connector.

Give the tunnel a name:

```text
knowledge-pi
```

Then create it.

### Install Cloudflared

Cloudflare will ask me to select the operating system and architecture.

I select Linux and ARM64 (64-bit).

Cloudflare provides the installation commands for the selected environment.

I run those commands in my Raspberry Pi SSH terminal.

The dashboard also provides a unique tunnel token.

I use the command shown by Cloudflare to install the service:

```bash
sudo cloudflared service install YOUR_TUNNEL_TOKEN
```

I replace the placeholder with the actual token.

**Important:** The tunnel token is a secret. I never publish it in GitHub, screenshots, or public documentation.

### Verify the tunnel

Check the service:

```bash
sudo systemctl status cloudflared
```

I return to Cloudflare's Tunnels dashboard.

My tunnel should eventually show:

```text
Healthy
```

The Raspberry Pi now has an outbound connection to Cloudflare.

No incoming router port forwarding is necessary.

## 9. Publish SilverBullet through Cloudflare

I return to my tunnel in the Cloudflare dashboard.

Under Routes, I select:

**Add route → Published application**

I configure:

| Setting | Value |
|---|---|
| Subdomain | `notes` |
| Domain | My Cloudflare domain |
| Service type | HTTP |
| Service URL | `http://localhost:3000` |

The resulting hostname will be:

```text
notes.mydomain.com
```

Cloudflare creates the appropriate DNS route for the tunnel.

The service URL points to SilverBullet on the Raspberry Pi itself.

Because the tunnel connector runs on the host, it can reach the locally bound SilverBullet port.

## 10. Protect SilverBullet using Cloudflare Access

I don't want just anyone to access my personal notes.

Cloudflare Access allows me to put an authentication layer in front of my application.

In Cloudflare Zero Trust, I navigate to:

```text
Access
   |
   +-- Applications
          |
          +-- Add an application
                 |
                 +-- Self-hosted
```

I configure:

```text
Application name: Personal Knowledge Base

Application domain:
notes.mydomain.com
```

### Create an access policy

I create an Allow policy.

For example:

```text
Policy name: Personal Access

Action: Allow

Include:
  Emails:
    my-email@example.com
```

I replace the example address with my actual email address.

I configure email one-time PIN authentication or another suitable identity provider.

Only my specified identity should be allowed through the application.

**Important:** I create the Access application and its Allow policy before publishing the application route, so I don't accidentally leave the service publicly accessible during setup.

I test the application in a private browser window.

Expected behaviour:

1. I visit `https://notes.mydomain.com`.
2. Cloudflare Access asks me to authenticate.
3. I enter my authorized email address.
4. I complete the authentication process.
5. Cloudflare forwards me to SilverBullet.

I retain SilverBullet's own authentication as an additional security layer.

Documentation:

https://developers.cloudflare.com/cloudflare-one/access-controls/applications/http-apps/self-hosted-public-app/

## 11. Verify the final server

I check the status of the important services.

### Docker

```bash
sudo systemctl status docker
```

### Cloudflare Tunnel

```bash
sudo systemctl status cloudflared
```

### SilverBullet

```bash
cd ~/services/silverbullet
sudo docker compose ps
```

### Check listening ports

```bash
sudo ss -tulpn
```

SilverBullet should listen on localhost port 3000, not on all network interfaces.

### Test remote access

I open:

```text
https://notes.mydomain.com
```

I should be able to authenticate through Cloudflare Access and reach my SilverBullet installation.

## 12. Updating and maintaining the server

### Update Raspberry Pi OS

```bash
sudo apt update
sudo apt full-upgrade -y
```

### Update SilverBullet

Enter its directory:

```bash
cd ~/services/silverbullet
```

Pull the latest image:

```bash
sudo docker compose pull
```

Recreate the container:

```bash
sudo docker compose up -d
```

Check the logs:

```bash
sudo docker compose logs -f
```

I should review release notes before upgrading and keep a backup of my data.

### Back up SilverBullet

My important files are stored in:

```text
~/services/silverbullet/data/
```

I can create a simple backup archive:

```bash
tar -czf silverbullet-backup.tar.gz \
  -C ~/services/silverbullet data
```

I should copy this backup to another device or storage location. A backup on the same microSD card is not sufficient protection against hardware failure.

For more reliable backups, I can eventually automate this process and store copies on my Mac Mini or an external SSD.

## 13. Final architecture

My Raspberry Pi now acts as a small personal server.

```text
Mac Mini / Phone
       |
       |
   Internet
       |
       |
   Cloudflare
       |
  Cloudflare Access
       |
  Cloudflare Tunnel
       |
       |
  Raspberry Pi 4
       |
  Raspberry Pi OS Lite
       |
   Docker Engine
       |
  Docker Compose
       |
   SilverBullet
       |
   /data
       |
  Markdown Files
```

My Raspberry Pi is responsible for running the application and storing my notes.

Docker manages the application environment.

Cloudflare Tunnel provides secure connectivity without opening inbound router ports.

Cloudflare Access controls who can reach the application.

My Markdown files remain stored on my own hardware.

## Conclusion

I now have the foundation for my own personal knowledge server.

I can access my notes from my Mac Mini, phone, or another device, provided I authenticate through Cloudflare Access.

This setup also gives me a foundation for hosting other personal services in the future.

I can eventually add:

- A personal Git server.
- A programming documentation wiki.
- A private file server.
- A development environment.
- My own learning platform.
- Other Docker-based applications.

The Raspberry Pi is no longer just a small computer. It is my own little server, under my control.
