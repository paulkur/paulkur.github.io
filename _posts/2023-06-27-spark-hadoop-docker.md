---
title: Spark Hadoop docker
date: 2023-06-27 13:53:00 +0100
categories: [homelab,hardware,setup,Hadoop,docker,spark]
tags: [hadoop,servers,docs,lessons,setup,docker,spark]     # TAG names should always be lowercase
pin: true
---

## Spark Cluster docker-compose

[Instructions here](https://hjben.github.io/spark-cluster/)

```bash
docker pull hjben/spark:3.4.0-jdk1.8.0
docker pull hjben/spark:3.4.0-livy
docker pull hjben/jupyter-lab:spark-livy
```

```bash
cd /home/paul/docker-master/spark/spark-cluster
```

```bash
./compose-up.sh 3.4.0 3 3 3 /home/ansible/scalagetdata/spark-notebook /tmp/spark_logs
```

```bash
./compose-up.sh 3.4.0 3 4 8 /home/paul/workspace/docker-ws/spark-notebook /tmp/spark_logs
```

- spark_version: Version of spark (3.4.0 and 3.1.1 is available now)
- (The # of) workers: The number of spark workers (integer between 1 and 5)
- (CPU)core: The number of CPU core of each spark worker (integer 1 or above)
- mem (GiB): The amount of memory size of each spark worker (integer 1 or above. The unit is GiB)
- jupyter_workspace_path: Host path for saving jupyter lab workspace data
- log_path: Host path for saving spark log

```bash
./compose-down.sh
```

- enter docker

```bash
docker exec -it master bash
```

- From Rock JVM

```bash
docker compose up -D --scale spark-worker=3
```

### Firewall rules for Rocky

```bash
# Hadoop ports
sudo firewall-cmd --add-port=8088/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=9870/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=8042/tcp --permanent --zone=trusted
# Spark ports
sudo firewall-cmd --add-port=7077/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=8888/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=8080/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=3306/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=10002/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=9083/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=4040/tcp --permanent --zone=trusted
# livy UI
sudo firewall-cmd --add-port=8998/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=5432/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=23/tcp --permanent --zone=trusted
sudo firewall-cmd --add-port=9001/tcp --permanent --zone=trusted
sudo firewall-cmd --reload
```

## Install Hadoop Rocky docker environment

[Instructions here](https://hjben.github.io/hadoop-cluster/)

```bash
docker pull hjben/hadoop-eco:3.3.0
docker pull hjben/hbase:1.6.0-hadoop3.3.0
docker pull hjben/mariadb:10.5
```

```bash
cd /home/paul/docker-master/hadoop-eco/docker-script
./compose-up.sh 3.3.0 3 /tmp/hadoop /tmp/hadoop_logs /tmp/hbase_logs /tmp/hive_logs /tmp/sqoop_logs mariadb /home/paul/workspace/docker-ws/maria-data
./compose-down.sh
```

- hadoop_version: Version of hadoop (3.3.0 and 3.2.2 are available now)
- (The # of) slaves: The number of hadoop slaves (integer between 1 and 5)
- hdfs_path: Host path for saving hdfs data
- hadoop_log_path: Host path for saving hadoop log
- hbase_log_path: Host path for saving hbase log
- hive_log_path: Host path for saving hive log
- sqoop_log_path: Host path for saving sqoop log
- mariaDB_root_password: Root password of mariaDB (mariadb is recommended)
- mariaDB_data_path: Host path for saving mariaDB data

```bash
cd /home/paul/docker-master/hadoop-eco/hadoop-eco
./hadoop-start.sh start
./hive-start.sh all
./hive-start.sh meta
docker exec -it master bash
```
