---
title: Data Engineering
author: Paul
date: 2023-09-17 11:33:00 +0100
categories: [Data, Engineer]
tags: [engineering,setup,docs]
pin: true
math: false
mermaid: false
---

## Setup Docker

* Go to this [link](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04) and follow the instructions to setup Docker.
* Make sure to add the user to `docker` group so that we do not have to use `sudo` to manage Docker related components such as images, containers etc.
* Let us validate docker by creating simple **Hello World** Container.
* Here are the commands to setup docker and validate. Make sure to run these commands one at a time.

```shell
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce -y

sudo systemctl status docker # Validates that docker is started

sudo usermod -aG docker ${USER}

docker run hello-world
```

## Validating Python

* By default, Ubuntu 18.04 VM will have Python3 installed. You can run `python3` and launch Python CLI.
* However, there might not be additional important modules such as pip, venv etc.
* We need to validate and ensure that they are installed. If `pip` and `venv` are not installed you can install them using these commands.

```shell
sudo apt install python3-pip -y
python3 -m pip install configparser

sudo apt install python3-venv -y
python3 -m venv testing
ls -ltr
rm -rf testing
```

## Setup Jupyter Lab

* Create Python based virtual environment - `python3 -m venv demojl`
* Activate virtual environment - `source demojl/bin/activate`
* Install required dependencies for Jupyter Lab - `pip install jupyterlab`
* Launch Jupyter Lab - `jupyter lab --ip 0.0.0.0 --port YOUR_PORT_NUMBER`
* At this time, you will not be able to access Jupyter Lab
* Go to firewall and open the port using GCP Web Console
* Now enter the ip address and port number to access the Jupyter Lab UI.

## Docker - Cheat Sheet

Here are the steps involved in setting up Database Services such as Postgres using Docker:

* Pull postgres image
* Create container for postgres
* Start the container
* Review the logs to ensure that container is created with out any issues

Here are important commands to manage images and containers:

* Managing images - `docker image`

|Command           |Description |
|------------------|------------|
|docker image pull |Pull image  |
|docker image rm   |Remove image|
|docker image build|Build image |

* Managing containers - `docker container`

|Command                  |Description                       |
|-------------------------|----------------------------------|
|docker container create  |Create container                  |
|docker container start   |Start container                   |
|docker container stop    |Stop  container                   |
|docker container restart |Restart container                 |
|docker container rm      |Remove container                  |
|docker container run     |Build, Create and Start container |
|docker container logs    |Check logs of docker container    |
|docker container rm      |Remove stopped container          |
|docker container rm -f   |Stop and Remove running container |
|docker container ls      |List containers                   |

```{note}
For most of these commonly used commands we have alternative with out image or container as keyword in the command - for example we can say `docker rm` to remove the container and `docker rmi` to remove the image.
```

```shell
docker pull postgres

docker container create \
  --name itv_pg \
  -p 5432:5432 \
  -h itv_pg \
  -e POSTGRES_PASSWORD=itversity \
  postgres
  
docker container start itv_pg

docker container logs itv_pg
docker container logs -f itv_pg
```

## Setup Postgres using Docker

* If you are using our labs, the database will be pre-created by us with all the right permissions.
* If you are using Windows or Mac, ensure that you have installed Docker Desktop.
* If you are using Ubuntu based desktop, make sure to setup Docker.
* Here are the steps that can be used to setup Postgres database using Docker.
  * Pull the postgres image using `docker pull`
  * Create the container using `docker create`.
  * Start the container using `docker start`.
  * Alternatively we can use `docker run` which will pull, create and start the container.
  * Use `docker logs` or `docker logs -f` to review the logs to ensure Postgres Server is up and running.

```shell
docker pull postgres

docker container create \
    --name itv_pg \
    -p 5432:5432 \
    -h itv_pg \
    -e POSTGRES_PASSWORD=itversity \
    postgres

docker start itv_pg

docker logs itv_pg
```

* You can connect to Postgres Database setup using Docker with `docker exec`.

```shell
docker exec \
    -it itv_pg \
    psql -U postgres
```

* You can also connecto to Postgres directly with out using `docker exec`, provided you have Postgres client setup on the host from which you are trying to connect to Postgres database. Don't worry too much if the below command doesn't work at this time.

```shell
psql -h localhost \
    -p 5432 \
    -d postgres \
    -U postgres \
    -W
```

## Accessing Postgres using Docker CLI

* We can use `docker container exec` or `docker exec` to connect to the container.
* You can attach to the container by running `bash` using `docker exec`.
* Also you can run single commands with out attaching the container - example: `docker exec -it itv_pg hostname -f`

> You have to use terminal to run these commands
{: .prompt-info }

* Attach to sms_db container - `docker exec -it itv_pg bash`
* Run command to get hostname - `hostname -f`
* Run command to connect to Postgres Database - `psql -U postgres`
* You can also directly connect to Postgres Database using

```shell
docker exec -it itv_pg psql -U postgres
```

* Use `\q` to come out of the Postgres CLI.

## Create Database and User

* Postgres is multi tenant database.
* We typically follow these steps to create a database which can be used by connecting as specific user.
  * Connect to postgres server as user postgres (super user)
  * Create database - `sms_db`
  * Create user with password - `sms_user`
  * Grant permissions on database to user
  
```shell
docker exec -it itv_pg psql -U postgres
```

```sql
CREATE DATABASE sms_db;
CREATE USER sms_user WITH ENCRYPTED PASSWORD 'sms_password';
GRANT ALL ON DATABASE sms_db TO sms_user;

\l --to list databases
\q --to quit from postgres CLI
```

* Make sure to validate by connecting using sms_user.

> When we use psql directly with in the container, you might be able to connect to database even with out password. Don't worry about it for now.
> Connect to postgres using newly created user
{: .prompt-info }

```shell
docker exec -it itv_pg psql -U sms_user -d sms_db -W
```

```sql
SELECT current_database();
CREATE TABLE t (i INT);
INSERT INTO t VALUES (1);
SELECT * FROM t;

\d
\d t

DROP TABLE t;
```

## Execute SQL Scripts

* Clone the GitHub repository on to the host.
* Copy the folder which contain scripts to itv_pg container under root folder **/**.
* Make sure **retail_db** database and **retail_user** user are created.
* Run the appropriate scripts to create tables as well as to insert data.
* Validate that tables are created and data is inserted by running simple queries.

<h3 data-toc-skip>Clone Repository</h3>

```shell
# Make sure you are in the home directory in the host (not in the container)
cd # to be in home directory
git clone https://www.github.com/dgadiraju/retail_db.git
```

<h3 data-toc-skip>Copy Script and Validate</h3>

```shell
docker container cp retail_db itv_pg:/

docker exec -it itv_pg ls -ltr /retail_db

docker exec -it itv_pg psql -U postgres
```

<h3 data-toc-skip>Create Database and User for retail_db</h3>

```shell
docker exec -it itv_pg psql -U postgres
```

```sql
CREATE DATABASE retail_db;
CREATE USER retail_user WITH ENCRYPTED PASSWORD 'retail_password';
GRANT ALL ON DATABASE retail_db TO retail_user;
```

<h3 data-toc-skip>Create tables and copy data</h3>

We will be running script to create tables and copy data.

```shell
docker cp ~/retail_db itv_pg:/
docker exec -it itv_pg psql -U retail_user -d retail_db -W

\i /retail_db/create_db_tables_pg.sql

\i /retail_db/load_db_tables_pg.sql
```

<h3 data-toc-skip>Validate - Run Queries</h3>

Make sure you are in right database and run these queries.

```shell
docker exec -it itv_pg psql -U retail_user -d retail_db -W
```

```sql
\d

\d orders

SELECT * FROM orders LIMIT 10;

SELECT count(1) FROM orders;
```

## SQL Workbench and Postgres

* Download the JDBC Driver
* Get the database connectivity information
* Configure the connection using SQL Workbench
* Validate the connection and save the profile

Here are the steps to connect to Postgres running as part of Docker container. You can go through the steps of dowloading the JDBC Driver as part of this [video](https://www.youtube.com/embed/IRRoAphUJmw?rel=0&amp;controls=1&amp;showinfo=0). This also covers how to connect to existing remote Postgres databases using SQL Workbench.

* We are trying to connect to Postgres Database that is running as part of Docker container running in a Ubuntu 18.04 VM provisioned from GCP.
* We have published Postgres database port to port 5432 on Ubuntu 18.04 VM.
* We typically use ODBC or JDBC to connect to a Database from remote machines (our PC).
* Here are the pre-requisites to connect to a Database on GCP.
  * Make sure 5432 port is opened as part of the firewalls.
  * If you have telnet configured on your system on which SQL Workbench is installed, make sure to validate by running telnet command using ip or DNS Alias and port number 5432.
  * Ensure that you have downloaded right JDBC Driver for Postgres.
  * Make sure to have right credentials (username and password).
  * Ensure that you have database created on which the user have permissions.
* You can validate credentials and permissions to the database by installing postgres client on Ubuntu 18.04 VM and then by connecting to the database using the credentials.
* Once you have all the information required along with JDBC jar, ensure to save the information as part of the profile. You can also validate before saving the details by using **Test** option.

## Jupyter Lab and Postgresql

* Using Jupyter Lab or Jupyter Notebook is optional. You can leverage SQL Workbench or `psql` to practice. However, using `psql` is a bit tricky and can take away considerable amount of time.
* We need additional libraries to be setup as part of Jupyter environment for integrating Notebooks with Postgres to write queries with out writing any code. Before getting into setup let us understand the pre-requisites.
  * You should have Python3 installed.
  * Also you should have setup Jupyter Lab environment by now. If not you can follow our [playlist](https://www.youtube.com/playlist?list=PLf0swTFhTI8qOGXb3e6BmqHGQ-tnsP51q) for the same. You will get step by step instructions to setup Jupyter Lab on Ubuntu VM on GCP using Docker.
* Once Jupyter Lab is setup we need to install the following to leverage Jupyter based notebooks to practice SQL.
  * You need to install `ipython-sql` library using `pip` with in the virtual environment used to setup Jupyter Lab.
  * You also need to install **SQL Alchemy** to facilitate the connectivity between Jupyter Notebooks and the databases. However, it will be installed along with `ipython-sql`. You can run `pip list` to validate whether **SQL Alchemy** is installed or not.
  * Also we need to install `psycopg2` to connect to Postgres database. If you are using Mac to setup Jupyter Lab, you have to install Postgresql using `brew install postgresql`.

```shell
pip install ipython-sql
pip list
brew install postgresql # On Mac
pip install psycopg2
```

* Here are the instructions to setup Postgresql on Ubuntu. You can get latest instructions from this [link](https://www.postgresql.org/download/linux/ubuntu/).

```shell
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql-common
```

* Make sure the Postgres database in docker is running fine. If not, start the docker container and then start Jupyter Lab.

```shell
docker ps -a
docker start itv_pg

jupyter lab --ip 0.0.0.0
```

* Now it is time for us to connect to Jupyter Lab using browser and validate.
* Once all the libraries are installed, we need to load sql extension and then create environment variable called as `DATABASE_URL` using all the connectivity information.
* We can run a query to validate that we are connected to the database.
* In JupyterLab cells enter:

```shell
%load_ext sql
```

```shell
%env DATABASE_URL=postgresql://retail_user:retail_password@localhost:5432/retail_db
```

```shell
%sql SELECT current_date()
```

## Jupyter Lab and Postgresql on Ubuntu VM

Let us go through the steps related to integrating Jupyter Lab with Postgresql on Ubuntu VM.

* Using Jupyter Lab or Jupyter Notebook is optional. You can leverage SQL Workbench or `psql` to practice. However, using `psql` is a bit tricky and can take away considerable amount of time.
* We need additional libraries to be setup as part of Jupyter environment for integrating Notebooks with Postgres to write queries with out writing any code. Before getting into setup let us understand the pre-requisites.
  * You should have Python3 installed.
  * Also you should have setup Jupyter Lab environment by now. If not you can follow our [playlist](https://www.youtube.com/playlist?list=PLf0swTFhTI8qOGXb3e6BmqHGQ-tnsP51q) for the same. You will get step by step instructions to setup Jupyter Lab on Ubuntu VM on GCP using Docker.
* Once Jupyter Lab is setup we need to install the following to leverage Jupyter based notebooks to practice SQL.
  * You need to install `ipython-sql` library using `pip` with in the virtual environment used to setup Jupyter Lab.
  * Activate the virtual environment.

```shell
cd delab
source delab-venv/bin/activate
```

* Here are the instructions to setup Postgresql on Ubuntu. You can get latest instructions from this [link](https://www.postgresql.org/download/linux/ubuntu/).

```shell
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

sudo apt-get update
sudo apt-get -y install postgresql-common
```

* You also need to install **SQL Alchemy** to facilitate the connectivity between Jupyter Notebooks and the databases. However, it will be installed along with `ipython-sql`. You can run `pip list` to validate whether **SQL Alchemy** is installed or not.
  * Also we need to install `psycopg2` to connect to Postgres database.

```shell
pip install ipython-sql
pip list
pip install psycopg2-binary
```

* Make sure the Postgres database in docker is running fine. If not, start the docker container.

```shell
docker ps -a
docker start itv_pg
```

* Let us also create a demo database to validate the connectivity.

```shell
docker exec -it itv_pg psql -U postgres
```

```sql
CREATE DATABASE demo_db;
CREATE USER demo_user WITH ENCRYPTED PASSWORD 'demo_password';
GRANT ALL ON DATABASE demo_db TO demo_user;

\q
```

* Start jupyter lab.

```shell
jupyter lab --ip 0.0.0.0
```

* Now it is time for us to connect to Jupyter Lab using browser and validate.
* Once all the libraries are installed, we need to load sql extension and then create environment variable called as `DATABASE_URL` using all the connectivity information.
* We can run a query to validate that we are connected to the database.
