# WSL Manual Setup Guide

This guide will help you set up **WSL (Windows Subsystem for Linux)** with essential developer tools and configurations. Follow these steps after reinstalling WSL.

---

## 📌 Initial Setup

### 1️⃣ Update Packages
```bash
sudo apt update
```

### 2️⃣ Install `nala` (Faster APT Package Manager)
```bash
curl https://gitlab.com/volian/volian-archive/-/raw/main/install-nala.sh | bash
```

### 3️⃣ Upgrade Packages Using `nala`
```bash
sudo nala upgrade -y
```

### 4️⃣ Install Fish Shell
```bash
sudo nala install -y fish
```

### 5️⃣ Switch to Fish Shell
```bash
fish
```

---

## 📦 Install Essential Packages

Run the following command to install commonly used development tools:

```bash
sudo nala install -y \
  build-essential \
  curl \
  git \
  wget \
  unzip \
  zip \
  htop \
  libssl-dev \
  zlib1g-dev \
  libbz2-dev \
  libreadline-dev \
  libsqlite3-dev \
  llvm \
  libncursesw5-dev \
  xz-utils \
  tk-dev \
  libxml2-dev \
  libxmlsec1-dev \
  libffi-dev \
  liblzma-dev \
  micro
```

---

## 📂 Create Directory Structure

Organize your projects and Fish configuration:

```bash
mkdir -p ~/Github ~/Projects
mkdir -p ~/.config/fish/
```

---

## 🐟 Set Up Fish Shell

### 1️⃣ Install Fisher (Plugin Manager for Fish)
```bash
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
```

### 2️⃣ Install Tide (Fish Prompt Theme)
```bash
fisher install IlanCosman/tide@v6
```

> **Note:** When prompted by Tide to configure interactively, you can answer **"no"** if you prefer manual configuration.

### 3️⃣ Configure Tide with Preferred Settings
```bash
tide configure --auto --style=Rainbow --prompt_colors='True color' --show_time='12-hour format' --rainbow_prompt_separators=Angled --powerline_prompt_heads=Sharp --powerline_prompt_tails=Flat --powerline_prompt_style='Two lines, frame' --prompt_connection=Disconnected --powerline_right_prompt_frame=No --prompt_connection_andor_frame_color=Darkest --prompt_spacing=Compact --icons='Few icons' --transient=No
```

### 4️⃣ Install NVM (Node.js Version Manager for Fish)
```bash
fisher install jorgebucaran/nvm.fish
```

---

## 🟢 Set Up Node.js with NVM

```bash
echo "Enter Node.js version to install (default: lts):"
read node_version
node_version=${node_version:-lts}

nvm install $node_version
nvm use $node_version
```

---

## 🐍 Install Python Tools

### 1️⃣ Install Pyenv (Python Version Manager)
```bash
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
```

### 2️⃣ Install Python with Pyenv
```bash
echo "Enter Python version to install (default: 3.12.8):"
read python_version
python_version=${python_version:-3.12.8}

pyenv install $python_version
pyenv global $python_version
```

### 3️⃣ Install Poetry (Python Package Manager)
```bash
curl -sSL https://install.python-poetry.org | python3 -
```

### 4️⃣ Install UV (Faster Python Package Installer)
```bash
curl -sSL https://astral.sh/uv/install.sh | bash
```

---

## 🔧 Configure Git

Set up Git with your user details:

```bash
echo "Enter your Git username:"
read git_username
echo "Enter your Git email:"
read git_email

git config --global user.name "$git_username"
git config --global user.email "$git_email"
git config --global init.defaultBranch main
git config --global core.editor "micro"
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"
```

---

## ⚙️ Configure Fish Shell

### Add the following to `~/.config/fish/config.fish`

```bash
cat > ~/.config/fish/config.fish << 'EOL'
# Fish configuration

# Set aliases
alias ll 'ls -la'
alias update 'sudo nala update && sudo nala upgrade -y'
alias neofetch 'fastfetch'
alias g 'cd ~/Github'
alias p 'cd ~/Projects'

# Add local bin to path
if test -d "$HOME/.local/bin"
    set -gx PATH $HOME/.local/bin $PATH
end

# Poetry setup
if test -d "$HOME/.poetry/bin"
    set -gx PATH $HOME/.poetry/bin $PATH
end

# uv setup
if test -d "$HOME/.cargo/bin"
    set -gx PATH $HOME/.cargo/bin $PATH
end

# Pyenv setup
set -x PYENV_ROOT $HOME/.pyenv
set -x PATH $PYENV_ROOT/bin $PATH
if command -v pyenv 1>/dev/null 2>&1
    status is-login; and pyenv init --path | source
    pyenv init - | source
end

# NVM setup
set -gx NVM_DIR "$HOME/.nvm"
# nvm.fish plugin handles the rest

# pnpm setup
set -gx PNPM_HOME "$HOME/.local/share/pnpm"
if not string match -q -- $PNPM_HOME $PATH
    set -gx PATH "$PNPM_HOME" $PATH
end
EOL
```

---

## 🛠️ Configure Bash to Launch Fish

Append the following to `~/.bashrc`:

```bash
cat >> ~/.bashrc << 'EOL'
# Launch Fish automatically in interactive Bash sessions
if [[ $- == *i* ]] && [ -z "$BASH_EXECUTION_STRING" ]; then
    exec fish
fi
EOL
```

---

## ✅ Complete the Setup

Restart your WSL terminal to apply all changes. Fish shell will automatically launch in interactive sessions.

---

## ❓ Troubleshooting

If you encounter issues:

1. Ensure all dependencies are installed correctly.
2. Check if environment variables are correctly set (`echo $PATH`).
3. Look for errors in command output.
4. Verify your user has the necessary permissions.

---

🎉 **Your WSL environment is now set up and ready to use!** 🚀
