---
title: Airflow Setup
date: 2023-06-17 16:46:00 +0100
categories: [homelab,airflow,setup,Gitlab,github]
tags: [servers,docs,gitlab,airflow,setup]     # TAG names should always be lowercase
---
Useful links:

- [Predefined variables reference](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)
- [GitLab-Runner Setup: All instructions](https://docs.gitlab.com/runner/install/)
- [Gitlab Repo Link to Nana Tutorial](https://gitlab.com/nanuchi/mynodeapp-cicd-project/-/tree/main)

### Links

- [raw Airflow install/Start on server](#raw-airflow-installstart-on-server)
- [Local Airflow Starting commands](#local-airflow-starting-commands)
- [Workflow](#workflow)
- [Project init](#project-init)
- [Gitlab \& Github sync](#gitlab--github-sync)

## raw Airflow install/Start on server

- Create a Python Virtual Environment for Apache Airflow

```bash
conda create --name airflow_env python=3.9 -y
```

- activate airflow_env

```bash
conda activate airflow_env
```

- Install Apache Airflow

```bash
pip install "apache-airflow==2.2.4" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.2.4/constraints-no-providers-3.9.txt"
```

- initiate airflow database

```bash
airflow db init
```

- create new admin user

```bash
airflow users create \
    --username paul \
    --firstname Paul \
    --lastname Kurpis \
    --role Admin \
    --email paulkurpis@gmail.com \
    --password natoMagic7812
```

- start webserver and scheduler

```bash
conda activate airflow_env
```

```bash
airflow webserver -D
```

```bash
airflow scheduler -D
```

- check if processes running

```bash
lsof -i tcp:8080
```

```bash
lsof -i tcp:8793
```

note: After any airflow.cfg editing, need to reset airflow db

```bash
airflow db reset
```

- Rocky linux firewall rules add

```bash
sudo firewall-cmd --add-port=10000/tcp --permanent
sudo firewall-cmd --add-port=8080/tcp --permanent
# maybe not needed
sudo firewall-cmd --add-port=8793/tcp --permanent
sudo firewall-cmd --add-port=5432/tcp --permanent
sudo firewall-cmd --add-port=24/tcp --permanent
```

reload

```bash
sudo firewall-cmd --reload
```

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