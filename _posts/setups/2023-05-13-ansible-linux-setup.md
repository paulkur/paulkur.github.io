---
title: Ansible Control env & Linux setups
date: 2023-05-13 12:53:00 +0100
categories: [homelab,hardware,setup,Ansible]
tags: [Ansible,servers,docs,lessons,setup]     # TAG names should always be lowercase 
pin: true
---

## Links

- [Links](#links)
  - [Setup ansible@control node](#setup-ansiblecontrol-node)
  - [Setup Managed nodes](#setup-managed-nodes)
  - [Troubleshooting \& Extras](#troubleshooting--extras)

### Setup ansible@control node

Create non-root user with sudo priv & enable ssh without need of passwd

```bash
cat /etc/redhat-release
```

- copyssh key

```bash
ssh-copy-id -i ~/.ssh/id_rsa ansible@82.18.235.250
```

```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorised_keys
```

```bash
echo 'ansible ALL=(ALL) NOPASSWD: ALL' >/tmp/sudoers
```

```bash
sudo cp /tmp/sudoers /etc/sudoers.d/ansible
```

test if works passwordless sudo

```bash
sudo ls -la /root
```

```bash
sudo yum -y update && sudo yum -y upgrade
sudo yum install -y openssh-server qemu-guest-agent nano curl wget git
```

```bash
sudo nano /etc/ssh/ssh_config
```

```bash
sudo systemctl restart sshd.service
sudo systemctl status sshd.service
```

Install ansible

```bash
sudo dnf install -y epel-release
sudo dnf install -y ansible
```

Setup hosts

```bash
sudo hostnamectl set-hostname control
```

Setup ansible host file

```bash
sudo nano /etc/ansible/hosts
```

```bash
sudo nano /etc/hosts
```

```bash
PasswordAuthentication yes
in sshd_config
```

```text
192.168.0.61 ubuntu
192.168.0.62 ansible1
192.168.0.63 ansible2
```

Copy and grant sudo rights for other nodes listed in inventory file

```bash
ansible rocky -i inventory -u root -k -m copy -a "src=/tmp/sudoers dest=/etc/sudoers.d/ansible"
```

[Back to Top](#links)

### Setup Managed nodes

- Ensure SSH is running on the hosts
- Include Host Names in Inventory
- Create the ansible user on managed nodes
- Set password for this user
- ConfigureSSH key-based login
  - ssh-keygen
  - ssh-copy-id
- Configure Privilege escalation

- Ensure SSH is running on the hosts

```bash
# rocky
yum -y update && yum -y upgrade
yum install -y openssh-server nano sshpass qemu-guest-agent curl wget git
```

```bash
# ubuntu
apt-get -y update && apt-get -y upgrade
apt install -y openssh-server nano sshpass qemu-guest-agent curl wget git
```

If Managed nodes are running as root, then create new users for them

on Rocky

```text
PermitRootLogin yes
PasswordAuthentication yes
```

```bash
sudo systemctl restart sshd.service
sudo systemctl status sshd.service
```

on Ubuntu root

```bash
adduser student
```

```bash
usermod -aG sudo student
```

```bash
nano /etc/ssh/sshd_config
```

```text
PermitRootLogin no
PasswordAuthentication yes
```

```bash
sudo systemctl restart sshd
sudo systemctl status sshd
```

```bash
echo 'ansible ALL=(ALL) NOPASSWD: ALL' >/tmp/sudoers
echo 'student ALL=(ALL) NOPASSWD: ALL' >/tmp/sudoers
```

```bash
sudo cp /tmp/sudoers /etc/sudoers.d/ansible
```

```bash
sudo cp /tmp/sudoers /etc/sudoers.d/student
```

test if works passwordless sudo

```bash
sudo ls -la /root
```

From ansible@control

```bash
mkdir lesson1 && cd lesson1 && nano inventory
```

enter:

```text
ubuntu

[rocky]
ansible1
ansible2
```

[Back to Top](#links)

Include Host Names in Inventory

```bash
sudo nano /etc/hosts
```

and add:

```bash
ubuntu IP_ADDRESS
ansible1 IP_ADDRESS
ansible2 IP_ADDRESS
```

- Create the ansible user on managed nodes

```bash
ansible -i inventory ubuntu -m user -a "name=ansible create_home=yes" -u student -b -k -K
```

```bash
ansible -i inventory rocky -m user -a "name=ansible" -u root -k
```

- Set password for this user

```bash
ansible -i inventory ubuntu -m shell -a "echo 'ansible:password' | chpasswd" -u student -b -k -K
```

```bash
ansible -i inventory rocky -m shell -a "echo password | passwd --stdin ansible" -u root -k
```

- ConfigureSSH key-based login

```bash
ssh-keygen
ssh-copy-id ubuntu
ssh-copy-id ansible1
ssh-copy-id ansible2
```

- print all user id  on all hosts

```bash
ansible -i inventory all -m command -a "id" -u ansible 
```

- Configure Privilege escalation

  - LINUX reference

```bash
echo 'ansible ALL=(ALL) NOPASSWD: ALL' >/tmp/sudoers
sudo cp /tmp/sudoers /etc/sudoers.d/ansible
ansible rocky -i inventory -u root -k -m copy -a "src=/tmp/sudoers dest=/etc/sudoers.d/ansible"
ansible ubuntu -i inventory -u student -k -b -K -m copy -a "src=/tmp/sudoers dest=/etc/sudoers.d/ansible"

ansible -i inventory  all -m command -a "ls -l /root" -b

ansible-config init --disabled > ansible.cfg

git clone https://github.com/sandervanvugt/ansiblecvc

ansible all -m command -a "ls -l /root" -b
```

- Windows reference:

[winRM Setup documentation](https://docs.ansible.com/ansible/latest//os_guide/windows_setup.html#id1)

```powershell
ipconfig
winrm quickconfig
winrm enumerate winrm/config/Listener
```

next:

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"
(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
powershell.exe -ExecutionPolicy ByPass -File $file
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force
&$file -Version 5.1 -Username $username -Password $password -Verbose
```





NOTE: need to be `PasswordAuthentication yes` to be able to copy ssh key

```bash
sudo service ssh restart
```

```bash
ssh student@ubuntu
```

***

[Back to Top](#links)

### Troubleshooting & Extras

- fix can't ssh to Ubuntu

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
sudo systemctl restart sshd
ssh -v student@ubuntu
```

- clear known_hosts

```bash
echo "" > ~/.ssh/known_hosts
```

- see authorized_keys

```bash
cat ~/.ssh/authorized_keys
```

- Permit root login

```bash
nano /etc/ssh/ssh_config
PermitRootLogin yes
systemctl restart sshd
```

- add user to root group

```bash
sudo adduser $USER root
sudo passwd -l root
```

- Edit config file and set hosts and key location

```bash
sudo nano /home/paul/.ssh/config
```

- SSH using key file

```bash
ssh -i ~/.ssh/ansible_control ansible@192.168.0.61
## from Windows: 
ssh -i C:\Users\USERNAME/.ssh/gitlab_rsa git@gitlab.com
## from Linux: 
ssh -i /.ssh/gitlab_rsa git@gitlab.com
```

- change owner of .ssh folder

```bash
sudo chown -R username:username /home/username/.ssh
```

root ssh key location `cd /etc/ssh`

[Back to Top](#links)
