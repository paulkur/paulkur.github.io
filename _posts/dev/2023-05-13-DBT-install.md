---
title: DBT Install
date: 2023-05-13 16:59:00 +0100
categories: [homelab,hardware,setup,DBT]
tags: [DBT,servers,docs,setup]     # TAG names should always be lowercase
---

## DBT Install

1. Set default location to startup. cd to: for windows WSL `/mnt/c/Users/paul/Desktop`  OR for linux `/home/paul/`. Edit bashrc `nano ~./bashrc`. Add line `alias winhome='cd /mnt/c/Users/paul/'`. Save settings `source ~./bashrc`.

- Get pip and install it

```bash
curl -o get_pip.py https://bootstrap.pypa.io/get-pip.py
```
    
if pip complains, fix PATH:

```bash
export PATH=/home/paul/.local/bin:$PATH
export PATH=/home/gitlab-runner/.local/bin:$PATH
```

install pip

```bash
python3 ./get_pip.py
```

To uninstall pip, cd to env where is pip

```bash
python3 -m pip uninstall pip setuptools
```

create project

```bash
mkdir course && cd course
mkdir dbt && cd dbt
mkdir airflow && cd airflow
```

install virtual enviroment

```bash
pip install virtualenv
```

create virtual enviroment

```bash
[syntax]
virtualenv [name]
[example]
virtualenv env
```

activate virtual enviroment

```bash
echo "echo 'whoa'" > ./project/.env
cd ./project
source ./env/bin/activate > ./course/.env
cd ./course

source ./env/bin/activate
which python
pip list --format=columns 
```

install DBT

```bash
pip install dbt-postgres
```

## Python PATH setting

Add to `nano .bashrc`. Structure `export PATH=”$PATH:/home/your_linux_username/.local/bin”`
examples

```bash
export PATH='$PATH:/home/paul/.local/bin'
export PATH='$PATH:/home/alexpaul/.local/bin'
/home/paul/.local/bin
/home/alexpaul/.local/bin
```

OR run this to export PATH

```bash
export PATH=/home/paul/.local/bin:$PATH
export PATH=/opt/airflow/.local/bin:$PATH
```

Activate virtual env when enter dir

```bash
source ./env/bin/activate
```

## Airflow Install

Get aifrow.cfg best latest

```bash
Docker exec -it airflow bash
```

copy .cfg to dags folder

```bash
cp /opt/airflow/airflow.cfg /opt/airflow/dags
```
