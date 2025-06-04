# Steps

## Prerequisites
```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
sudo apt autoclean -y
sudo apt install git vim tmux htop nmap ufw
```

## Setup new user with sudo privileges

### Create new user

```bash
adduser username
usermod -aG sudo username
```

### Create SSH key for new user

```bash
ssh-keygen -f ~/.ssh/key_name -t ed25519 -C your@email.com
```

Add the key to your ~/.ssh/config file:

```text
IdentityFile ~/.ssh/key_name
```

Add the key to your server

```bash
ssh-copy-id -i ~/.ssh/key_name.pub username@hostname
```

### Restrict login access

```bash
sudo vi /etc/ssh/sshd_config
```

```text
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthenticatiot no
PermitRootLogin no
```

Make sure you are able to login to the server using the SSH key before this step

```bash
sudo systemctl restart sshd
```

## Firewall

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https

sudo ufw enable
sudo ufw status verbose
```

## Setup git SSH

Create an SSH key:

```bash
ssh-keygen -f ~/.ssh/git_hub -t ed25519 -C your@email.com
```

And add the key to your ~/.ssh/config file:

```bash
IdentityFile ~/.ssh/git_hub
```

In GitHub add the SSH key as a deploy key in the repo.

```bash
cat ~/.ssh/git_hub.pub
```

## Setup config

```bash
git clone https://github.com/tvgelderen/server-setup.git
mv server-setup/.* .
rm -rf server-setup
rm -rf .git
```

## Docker (https://docs.docker.com/engine/install/debian/)

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
