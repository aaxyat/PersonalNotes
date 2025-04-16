# 🚀 VPS Setup Guide

This guide contains a collection of commands for setting up a new Virtual Private Server (VPS) with my preferred configuration.

## 📋 Initial Setup

```bash
# Update package lists
apt update

# Install curl and wget
apt install curl wget sudo -y

# Install nala (improved APT frontend)
curl https://gitlab.com/volian/volian-archive/-/raw/main/install-nala.sh | bash

# Use nala to update packages
nala update -y

# Install additional packages
nala install -y fish git curl wget nodejs npm

# Create and configure swap file
sudo fallocate -l 8096M /swapfile
sudo mkswap /swapfile
sudo chmod 0600 /swapfile
sudo swapon /swapfile
echo "/swapfile swap swap defaults 0 0" | sudo tee -a /etc/fstab

# Create a new user 'aaxyat' with sudo privileges and set password
useradd -m -s /bin/bash aaxyat
usermod -aG sudo aaxyat
passwd aaxyat  # This will prompt for password

# Upgrade all packages
nala upgrade -y
```

## 👤 User Setup

```bash
# Switch to user aaxyat
su - aaxyat

# Change to aaxyat's home directory
cd ~

# Change current shell to fish
fish
```

## 🔒 SSH Configuration

```bash
# Set Fish shell as the default shell for future sessions
echo "fish" >> .bashrc

# Set up SSH keys
mkdir -p .ssh
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC9DEsM1tG0mwfYLLDPPpGyg9X0oW1vLQ8LuGkvaZQHLPtRpmumCPByC/MUsLDbvSGVGu9DsgyL402Fu7I+k0sgI+MA3r8tq9RtQa8iPa99puvKj5t45U3kh9Hu/e9YXmQgVO3/1YF4LMgVBVsPgp/5gKjaRTGdVx/1I3pKEYFH2SlWiyi8RBdc9IQWNsLfApOPYJ9eNHI7pInBqNWdE14eL6Ucd7HO0+4js0m7Sk034mFkJD06JeLs4LmoE6d+gsVDpa1RlKSe6GCEqGNA7CcKK/8A+x4Xddg3p8CB3Rip53WU/+mi6ro+64GMJTECfCoXtW/uNEPZvo3vvTsfzCx1 ssh-key-2021-10-22" > .ssh/authorized_keys
chmod 600 .ssh/authorized_keys

# Download and replace sshd_config
curl -L https://raw.githubusercontent.com/aaxyat/Scripts/master/sshd_config > sshd_config
sudo mv /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo mv sshd_config /etc/ssh/sshd_config
sudo systemctl restart sshd
```

## 🐠 Fish Shell Configuration

```bash
# Install Oh My Fish
curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish

# Install bira theme for Oh My Fish
omf install bira
```

## 🔄 Supervisor Configuration

```bash
# Install supervisor and stress-ng
sudo nala update && sudo nala install -y supervisor stress-ng

# Create Supervisor configuration for stress-ng
cat <<EOL | sudo tee /etc/supervisor/conf.d/stress.conf
[program:cpu_stress]
command=/usr/bin/stress-ng --cpu 4 --cpu-load 15
directory=/usr/bin/
user=root
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=/var/log/stress.log

[program:memory_stress]
command=/usr/bin/stress-ng --vm 1 --vm-bytes 15% --vm-hang 0
directory=/usr/bin/
user=root
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=/var/log/stress.log
EOL

# Reload Supervisor configuration
sudo supervisorctl reread
sudo supervisorctl update
```

## 🐳 Docker Setup

```bash
# Remove existing Docker packages (if installed)
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc
    sudo apt-fast remove -y $pkg
end

# Install prerequisites (lsb-release, htop, etc.)
sudo nala install -y lsb-release htop

# Add Docker's official GPG key
sudo nala update
sudo nala install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Set the codename for your Debian version
set codename (lsb_release -cs)

# Add Docker's repository
echo "deb [arch="(dpkg --print-architecture)"] signed-by=/etc/apt/keyrings/docker.asc https://download.docker.com/linux/debian $codename stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package list and install Docker
sudo nala update
sudo nala install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-compose

# Add user to the Docker group
sudo groupadd -f docker
sudo usermod -aG docker $USER

# Create Docker volume for Portainer
sudo docker volume create portainer_data

# Run Portainer container
sudo docker run -d -p 8000:8000 -p 9000:9000 \
    --name=portainer --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce

# Print confirmation message
echo "Docker and Portainer setup complete!"
```

## 🌐 Network Tools

```bash
# Install Tailscale for secure networking
curl -fsSL https://tailscale.com/install.sh | sh
```


