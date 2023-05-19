---
title: Linux & Win Extras
date: 2023-05-15 12:53:00 +0100
categories: [homelab,setup,Ansible]
tags: [Ansible,extra,win,setup]     # TAG names should always be lowercase
pin: true
---

### vscode server

On Rocky

```bash
curl -fsSL https://code-server.dev/install.sh | sh
```

```bash
sudo systemctl start code-server@$USER
sudo systemctl enable code-server@$USER
sudo systemctl status code-server@$USER
sudo systemctl restart code-server@$USER
```

```bash
sudo dnf install firewalld
```

```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

```bash
sudo firewall-cmd --add-port={8080,80,443}/tcp --permanent
sudo firewall-cmd --reload
```

### Other Linux commands

Disable passwd Authentication when using ssh key after key is copied

```bash
sudo nano /etc/ssh/sshd_config
```

change to

```text
PasswordAuthentication no
```

```bash
sudo nano /etc/ssh/sshd_config
PermitRootLogin yes
sudo systemctl restart sshd
```

- Create SSH key and copy

```bash
ssh-keygen
ssh-copy-id username@host_name
```

```bash
ssh-copy-id -i ~/.ssh/proxmox-key root@192.168.0.49
ssh-copy-id -i ~/.ssh/cloud-init ansible@ubuntu
ssh-copy-id -i ~/.ssh/cloud-init root@192.168.0.103

ssh -i ~/.ssh/cloud-init student@192.168.0.81
ssh -i ~/.ssh/cloud-init ansible1@192.168.0.91
ssh -i ~/.ssh/cloud-init ansible2@192.168.0.92
```

- Note: If the ssh-copy-id command is not available on your local machine, you can manually copy the public key to the server by appending it to the `~/.ssh/authorized_keys` for example windows CMD

```bash
cat ~/.ssh/id_rsa.pub | ssh username@server_ip_address "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

- Create non-root user with sudo priv & enable ssh without need of passwd

```bash
adduser student
passwd SecretPaSSwd
usermod -aG sudo student
# Test sudo
su - student
sudo ls -la /root
# Edit sudoers (grand root rights)
visudo

root ALL=(ALL:ALL) ALL
student ALL=(ALL:ALL) ALL
```

## How to Enable or Disable Copy/Paste via RDP Clipboard on Windows?

- Run the Local Group Policy Editor: `gpedit.msc`
- Go to `Computer Configuration -> Administrative Templates -> Windows Components -> Remote Desktop Services -> Remote Desktop Session Host -> Device and Resource Redirection`
- set the following policies to Enabled:

```text
Do not allow Clipboard redirection 
Do not allow drive redirection
```
