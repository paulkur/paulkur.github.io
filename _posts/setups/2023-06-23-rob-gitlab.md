---
title: Rob GitLab
date: 2023-06-23 11:36:00 +0100
categories: [setup,Gitlab]
tags: [servers,docs,gitlab,setup]     # TAG names should always be lowercase
pin: true
---

Useful links:

- [Predefined variables reference](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)
- [GitLab-Runner Setup: All instructions](https://docs.gitlab.com/runner/install/)
- [Gitlab Repo Link to Nana Tutorial](https://gitlab.com/nanuchi/mynodeapp-cicd-project/-/tree/main)

### Links

- [Gitlab \& Github sync](#gitlab--github-sync)
- [Common git commands](#common-git-commands)
- [GitLab-Runner Install \& Register 1](#gitlab-runner-install--register-1)
- [GitLab-Runner Install \& Register 2](#gitlab-runner-install--register-2)
  - [Tags for pipeline](#tags-for-pipeline)
- [Uninstall / de-register](#uninstall--de-register)
- [Git config](#git-config)
- [Gitlab Clone using a token (private access token)](#gitlab-clone-using-a-token-private-access-token)

[Self-create-repo-link-example-for-docs](https://github.com/just-the-docs/just-the-docs-template/generate)

## Gitlab & Github sync

```bash
git remote add gitlab <gitlab_repository_URL>
git pull gitlab main --allow-unrelated-histories
# Push changes to the primary repository (GitHub)
git push origin <branch_name>
# Push changes to the secondary repository (GitLab)   
git push gitlab <branch_name>
```

## Common git commands

```powershell
git update-git-for-windows
```

## GitLab-Runner Install & Register 1

Steps to reproduce (in Administrator Powershell):

- Create directory: mkdir c:\Users\gitlab-runner

- Create account: net user gitlab-runner "P@55w0rd" /add /fullname:"GitLab CI Runner User" /homedir:"C:\Users\gitlab-runner"

- Copy runner: cp gitlab-ci-multi-runner-windows-amd64.exe c:\Users\gitlab-runner

- Change dir: cd c:\Users\gitlab-runner

- Register runner: .\gitlab-ci-multi-runner-windows-amd64 register

- Install runner service: .\gitlab-ci-multi-runner-windows-amd64 install -u "gitlab-runner" -p "P@55w0rd"

P.S.: need to add `.\` like this `install -u ".\gitlab-runner" -p "P@55w0rd"`

or try:

```powershell
gitlab-ci-multi-runner install
```

## GitLab-Runner Install & Register 2

```bash
# ubuntu
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
sudo apt-get install gitlab-runner
```

```bash
# rocky
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh" | sudo bash
sudo yum install gitlab-runner
```

```bash
sudo gitlab-runner register
```

```bash
sudo gitlab-runner -version
```

```powershell
mkdir C:\GitLab-Runner

New-Item -ItemType Directory -Force -Path C:\GitLab-Runner
```

- Copy file

```powershell
Copy-Item -Path "C:\Users\Paul\Git\gitlab-runner.exe" -Destination "C:\GitLab-Runner"
```

```powershell
cd C:\GitLab-Runner
.\gitlab-runner.exe install
.\gitlab-runner.exe register
```

- Instance URL:

```bash
https://gitlab.com/
```

- Registration token (get from GitLab project CI/CD settings):

```bash
secret-number
```

- Description

- AWS

```bash
aws-win-runner
```

```bash
aws-lnx-runner
```

- proxmox

```bash
prox-win-runner
```

```bash
prox-lnx-runner
```

- Rob

```bash
robs-win-runner
```

- others

```bash
docker-lnx-runner
docker-win-runner
think-local-win-shell
```

- Tags

- AWS

```bash
aws, win, shell, remote
```

```bash
aws, lnx, docker, remote
```

- proxmox

```bash
prox, win, shell, serv
```

```bash
prox, lnx, shell, serv
```

- Rob

```bash
rob, win, shell, serv
```

- others

```bash
prox, lnx, docker, serv
think, win, local
```

### Tags for pipeline

```yml
aws-win-runner
    - aws
    - win
    - shell
    - remote

prox-win-runner
    - prox
    - win
    - shell
    - serv

robs-win-runner
    - rob
    - win
    - shell
    - serv

think-local-win-shell
    - think
    - win
    - local
```

[Back to Top](#links)

- Optional maintenance note for the runner:

```bash
    The runner is using shell executor
```

```bash
    The runner is using docker executor
```

- Executor: `shell` or `docker`
- Start/Stop/Status

```bash
    sudo gitlab-runner start
```

```powershell
.\gitlab-runner.exe start
```

```powershell
.\gitlab-runner.exe stop
```

```bash
    sudo gitlab-runner stop
```  

- If will later install docker executor, then add to docker group

```bash
sudo adduser gitlab-runner docker
```

[Back to Top](#links)

## Uninstall / de-register

```bash
sudo gitlab-runner list
gitlab-runner unregister --name test-runner
sudo gitlab-runner unregister --all-runners
sudo gitlab-runner verify --delete
sudo apt-get purge gitlab-runner
sudo apt-get remove --auto-remove gitlab-runner
sudo pkill -f gitlab
rm -rf /opt/gitlab rm -rf /etc/gitlab rm -rf /var/opt/gitlab
sudo gitlab-runner uninstall
sudo rm -rf /usr/local/bin/gitlab-runner
sudo userdel gitlab-runner
sudo rm -rf /home/gitlab-runner/
```

- windows (From an elevated command prompt:)

```powershell
cd C:\GitLab-Runner
```

```powershell
.\gitlab-runner.exe stop
```

```powershell
.\gitlab-runner.exe uninstall
```

```powershell
cd ..
rmdir /s GitLab-Runner
```

- windows all in one (From an elevated command prompt:)

```powershell
cd C:\GitLab-Runner
.\gitlab-runner.exe stop
.\gitlab-runner.exe uninstall
cd ..
rmdir /s GitLab-Runner
```

[Back to Top](#links)

## Git config

```bash
git config --list --show-origin
git config --global user.name your-username
git config --global user.email your-email@gmail.com
```

- GitLab project init from readme.md

```bash
## Git config SSH to git@gitlab.com
sudo apt install -y git curl
git config --global user.name "Name Lastname" // GitLab
git config --global user.email "your_email@gmail.com"
ssh -T /Users/your-username/.ssh/gitlab_rsa.pub git@gitlab.com
ssh -Tv git@gitlab.com
git config --global --list
git remote add origin git@gitlab.com:username/project-name.git
sudo usermod -aG docker gitlab-runner
sudo -u gitlab-runner -H docker info
## from Windows
ssh -i C:\Users\your-username/.ssh/gitlab_rsa git@gitlab.com
```

[Back to Top](#links)

## Gitlab Clone using a token (private access token)

Clone with HTTPS using a token if:

- You want to use 2FA
- You want to have a revocable set of credentials scoped to one or more repositories

You can use any of these tokens to authenticate when cloning over HTTPS:

- Personal access tokens
- Deploy tokens
- Project access tokens
- Group access tokens

Using Personal access token:

```bash
git clone https://<username>:<token>@gitlab.example.com/tanuki/awesome_project.git
```

[Back to Top](#links)
