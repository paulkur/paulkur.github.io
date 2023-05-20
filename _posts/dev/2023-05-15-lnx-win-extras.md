---
title: Linux & Win Extras
date: 2023-05-15 12:53:00 +0100
categories: [homelab,setup,Ansible]
tags: [Ansible,extra,win,setup]     # TAG names should always be lowercase
pin: true
---

### vscode server

#### On Rocky

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

#### On Ubuntu

```bash
sudo apt update
```

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
sudo apt install nginx -y
```

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

```bash
sudo apt install nano -y
```

```bash
sudo nano /etc/nginx/sites-available/code-server
```

```bash
server {
listen 80;
listen [::]:80;
#server_name yourdomain.com;
server_name 192.168.1.243;

location / {
proxy_pass http://localhost:8080/;
proxy_set_header Host $host;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection upgrade;
proxy_set_header Accept-Encoding gzip;
}
}
```

Enable site configuration file:

```bash
sudo ln -s ../sites-available/code-server /etc/nginx/sites-enabled/code-server
```

Restart servers:

```bash
sudo systemctl restart nginx && sudo systemctl restart code-server
```

Open port 80 and 443 in the firewall:

```bash
sudo ufw allow 80 && sudo ufw allow 443
```

Access VS code web interface

```bash
http:server-ip-address 

or 

http://your-domain
```

get password:

```bash
nano ~/.config/code-server/config.yaml
```

To use the Let’s Encrypt SSL certificate

```bash
sudo apt install -y certbot python3-certbot-nginx
```

```bash
sudo certbot --non-interactive --redirect --agree-tos --nginx -d yourdomain.com -m me@example.com
```

```bash
sudo systemctl restart nginx -y
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
