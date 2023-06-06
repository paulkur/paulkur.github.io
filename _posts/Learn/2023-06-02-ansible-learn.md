---
title: Ansible learn
date: 2023-06-02 11:53:00 +0100
categories: [homelab,hardware,setup,Ansible]
tags: [ansible,servers,docs,lessons,setup]     # TAG names should always be lowercase
pin: true
---

## Ansible lessons windows Managed node

```bash
sudo dnf install -y python3-pip
sudo pip3 install pywinrm
cd windows
```

### win commands

```bash
ansible win -m win_ping

cp ../ansiblecvc/ansible.cfg .

ansible tower -m user -a "name=ansible" -u root -k
ansible tower -m command -a "id"
ansible tower -m command -a "ls -la /root"
```

## 👇 Lesson 1.7: Config Linux managed hosts with Ad-hoc

- KILL stuck VM. get PID

```bash
ps aux | grep "/usr/bin/kvm -id VMID"
kill -9 PID
```

- clear known_hosts

```bash
echo "" > ~/.ssh/known_hosts
```

### Setting up managed hosts

- If a host meets the following minimal requirements, you can use Ansible to take care of further configuration
  - SSH is running and reachable
  - There is a user with administrative privileges

If hosts not meeting this requirements, you need to manually change them before using Ansible on them.

- Create a new user account with admin (sudo) access on Ubuntu or Debian Linux

```bash
usermod -aG sudo student
```

1. Ensure SSH is running on the hosts
2. Include Host Names in Inventory
3. Create the ansible user on managed nodes
4. Set password for this user
5. ConfigureSSH key-based login
  a. ssh-keygen
  b. ssh-copy-id
1. Configure Privilege escalation

- check ssh Ubuntu

```bash
sudo systemctl restart sshd
sudo systemctl status sshd
```

```bash
ansible -i inventory ubuntu -m user -a "name=ansible create_home=yes" -u student -b -k -K
```

```bash
ansible -i inventory windows -m ansible.windows.win_ping
ansible -i inventory all -m ansible.builtin.command -a reboot
```


## 👇 Lesson 2: Using Ad-hoc Commands

```bash
ansible -i inventory windows -m ansible.windows.win_ping
ansible -i inventory all -m ansible.builtin.command -a reboot
```
