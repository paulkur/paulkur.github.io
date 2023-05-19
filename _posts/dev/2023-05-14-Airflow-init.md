---
title: Airflow Setup
date: 2023-05-14 16:56:00 +0100
categories: [homelab,airflow,setup,Gitlab,github]
tags: [servers,docs,gitlab,airflow,setup]     # TAG names should always be lowercase
---
Useful links:

- [Predefined variables reference](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)
- [GitLab-Runner Setup: All instructions](https://docs.gitlab.com/runner/install/)
- [Gitlab Repo Link to Nana Tutorial](https://gitlab.com/nanuchi/mynodeapp-cicd-project/-/tree/main)

### Links

- [Local Airflow Starting commands](#local-airflow-starting-commands)
- [Workflow](#workflow)
- [Project init](#project-init)
- [Gitlab \& Github sync](#gitlab--github-sync)

## Local Airflow Starting commands

- in this order

```bash
getair
webair
schair
```

- import connections and variables

```bash
importkeys
importcon
```

- kill process

```bash
lsof -i tcp:8080
lsof -i tcp:8793
```

- connect and run postgres commands

```bash
getpos
sudo -u postgres psql
```

```powershell
airflow tasks test <dag_name> <task_name> <date_in_the_past>
```

- WSL cmd

```bash
wsl --shutdown
```

[Access Airflow on localhost](http://localhost:8080)


## Workflow

Create new branch and do your work

```bash
git checkout -b <new-branch-name> ## examples: paulk-dev-branch, update-ip
git checkout -b paulk-dev-branch 
git status
git add .
git commit -m "<add-message-here>" ## examples: "adding proxmox VMs"
git push -u origin <branch_name>
```

Go to gitlab.com, create pull request, add message of what you did. When you fix something, you run:

```bash
git diff
git add .
git commit -m "fix(lint): cleaned up"
git push -u origin <branch_name>
```

```bash
git checkout -b feature/provisioners
git checkout -b feature/modules
```

- using zsh and power10. For help: `acs <keyword>`

```bash
gco -b <new-branch-name>
gco -b update-ip
gst
ga . # gitadd
gcmsg "<add-message-here>"
gcmsg "adding proxmox VMs"
ggpush
```

```bash
gd  # look for differences - git diff
ga . # gitadd
gcmsg "fix(lint): cleaned up"
ggpush
```

## Project init

```bash
cd existing_repo
git init 
git remote add origin <project-url>
git branch -M main
git add .
```

Before push to GitLab, if project is private, you need to un-protect main branch. Open your project: `Settings > Repository` go to `Protected branches` find `master` and `Unprotected`

```bash
git commit -m "initial commit"
git push -uf origin main
```

- using zsh and power10. For help: `acs <keyword>`

```bash
cd existing_repo
git init 
gra origin <project-url>
git branch -M main
git push -u origin main
```

```bash
ga .
gcmsg "initial commit"
gp -uf origin main
```

[Back to Top](#links)

## Gitlab & Github sync

```bash
git remote add gitlab <gitlab_repository_URL>
git pull gitlab main --allow-unrelated-histories
# Push changes to the primary repository (GitHub)
git push origin <branch_name>
# Push changes to the secondary repository (GitLab)   
git push gitlab <branch_name>
```

[Back to Top](#links)