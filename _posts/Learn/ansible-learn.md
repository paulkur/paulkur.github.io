---
title: Ansible learn
date: 2023-05-13 22:53:00 +0100
categories: [homelab,hardware,setup,Ansible]
tags: [ansible,servers,docs,lessons,setup]     # TAG names should always be lowercase
pin: true
---

## Ansible lessons windows Managed node

```bash
sudo dnf install python3-pip
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

## 👇 Lesson 2: Using Ad-hoc Commands

```bash
ansible -i inventory windows -m ansible.windows.win_ping
ansible -i inventory all -m ansible.builtin.command -a reboot
```
