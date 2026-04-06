# Essentials

```bash
sudo apt update
sudo apt install build-essential git curl ca-certificates gnupg lsb-release
```

---

# Editor

## Option A: Neovim

```bash
sudo apt install neovim
```

Then install a starter config (kickstart):

```bash
git clone https://github.com/nvim-lua/kickstart.nvim.git ~/.config/nvim
```

---

## Option B: VS Code

Install via Microsoft repo:

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /usr/share/keyrings/ms.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/ms.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list

sudo apt update
sudo apt install code
```

---

## Install Docker 

```bash
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
```

Log out/in afterward.

---

## Optional: Docker Compose

```bash
sudo apt install docker-compose-plugin
```

---

## Install extra shell tools

```bash
sudo apt install tmux fzf ripgrep fd-find
```

---

## Set Zsh as default

```bash
chsh -s $(which zsh)
```

---

# Other tools

```bash
sudo apt install shellcheck jq httpie
```

* `shellcheck` - bash linting
* `jq` - JSON parsing
* `httpie` - API testing




