---
title: Hadoop learn
date: 2023-06-27 13:53:00 +0100
categories: [homelab,hardware,setup,Hadoop]
tags: [hadoop,servers,docs,lessons,setup]     # TAG names should always be lowercase
pin: true
---

## Install Hadoop rocky docker

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

firewall rules rocky

```bash
# Hadoop ports
sudo firewall-cmd --add-port=8088/tcp --permanent
sudo firewall-cmd --add-port=9870/tcp --permanent
sudo firewall-cmd --add-port=8042/tcp --permanent
# Spark ports
sudo firewall-cmd --add-port=7077/tcp --permanent
sudo firewall-cmd --add-port=8888/tcp --permanent
sudo firewall-cmd --add-port=8080/tcp --permanent
sudo firewall-cmd --add-port=3306/tcp --permanent
sudo firewall-cmd --add-port=10002/tcp --permanent
sudo firewall-cmd --add-port=9083/tcp --permanent
sudo firewall-cmd --add-port=4040/tcp --permanent
# livy UI
sudo firewall-cmd --add-port=8998/tcp --permanent
sudo firewall-cmd --add-port=23/tcp --permanent
sudo firewall-cmd --reload
```

## Spark Cluster rocky docker

```bash
docker pull hjben/spark:3.4.0-jdk1.8.0
docker pull hjben/spark:3.4.0-livy
docker pull hjben/jupyter-lab:spark-livy
```

```bash
cd /home/paul/docker-master/spark/spark-cluster
./compose-up.sh 3.4.0 3 4 8 /home/paul/workspace/docker-ws/spark-notebook /tmp/spark_logs
./compose-up.sh 3.4.0 3 3 4 /home/paul/workspace/docker-ws/spark-notebook /tmp/spark_logs
./compose-down.sh
docker exec -it sk-master bash

./compose-up.sh 3.3.5 3.4.0 3 /home/paul/workspace/docker-ws/spark-notebook /tmp/hadoop /tmp/hadoop_logs /tmp/spark_logs
```

- spark_version: Version of spark (3.4.0 and 3.1.1 is available now)
- (The # of) workers: The number of spark workers (integer between 1 and 5)
- (CPU)core: The number of CPU core of each spark worker (integer 1 or above)
- mem (GiB): The amount of memory size of each spark worker (integer 1 or above. The unit is GiB)
- jupyter_workspace_path: Host path for saving jupyter lab workspace data
- log_path: Host path for saving spark log

```bash
docker compose up --scale spark-worker=3
```

## Install Hadoop ubuntu

- zrc commands

```bash
## Hadoop
alias hdphome="cd /usr/local/hadoop/"
alias hdpenv="sudo nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh"
alias hdpcore="sudo nano /usr/local/hadoop/etc/hadoop/core-site.xml"
alias hdphdfs="sudo nano /usr/local/hadoop/etc/hadoop/hdfs-site.xml"
alias hdpmapred="sudo nano /usr/local/hadoop/etc/hadoop/mapred-site.xml"
alias hdpyarn="sudo nano /usr/local/hadoop/etc/hadoop/yarn-site.xml"
alias hdpformat="hdfs namenode -format"
alias hdpstop2="hdfs --daemon stop secondarynamenode"

#
alias hdpstart="/usr/local/hadoop/sbin/start-all.sh"
alias hdpstop="/usr/local/hadoop/sbin/stop-all.sh"
alias hdpstartdfs="/usr/local/hadoop/sbin/start-dfs.sh"
alias hdpstartyarn="/usr/local/hadoop/sbin/start-yarn.sh"
alias hdpstopdfs="/usr/local/hadoop/sbin/stop-dfs.sh"
alias hdpfs="hadoop fs"
# -moveFromLocal 
# -copyToLocal 
alias hdpmd="hadoop fs -mkdir -p /user/nuggetuser/"
alias hdpmdp="hadoop fs -mkdir -p hdfs://hnname:9000/data/small"
# yarn --daemon start resourcemanager
# hdfs --daemon start datanode
```

```bash
hadoop fs -mkdir -p hdfs://hnname:9000/data/small
hadoop fs -moveFromLocal /home/nuggetuser/data/small/BTCUSDT_2023.csv hdfs://hnname:9000/data/small/BTCUSDT_2023.csv
hadoop fs -copyToLocal hdfs://hnname:9000/data/small/BTCUSDT_2023.csv /home/nuggetuser/data/small/btcusdt_2023.csv
hadoop fs -put /home/nuggetuser/data/prices hdfs://hnname:9000/data/big
```

sudo apt-get install openjdk-11-jdk

- More detailed copy/paste instructions [here](https://www.geeksforgeeks.org/how-to-install-hadoop-in-linux/)
- Single node setup [here](https://hadoop.apache.org/docs/r3.3.5/hadoop-project-dist/hadoop-common/SingleCluster.html)
- Cluster setup [here](https://hadoop.apache.org/docs/r3.3.5/hadoop-project-dist/hadoop-common/ClusterSetup.html)
- Download from [here](https://dlcdn.apache.org/hadoop/common/)

localhosts:
- [core-site port 9000](http://localhost:9000/)
- [core-site port 2 9870](http://localhost:9870/)
- [namenode port 50070](http://localhost:50070/)
- [cluster NodeManager port 8088](http://localhost:8088/cluster)

localhosts on prox:
- [core-site port 9000](http://192.168.0.19:9000/)
- [core-site port 2 9870](http://192.168.0.19:9870/)
- [namenode port 50070](http://192.168.0.19:50070/)
- [cluster NodeManager port 8088](http://192.168.0.19:8088/cluster)

external on prox:
- [core-site port 9000](http://80.2.129.22:9000/)
- [core-site 2 port 9870](http://80.2.129.22:9870/)
- [namenode port 50070](http://80.2.129.22:50070/)
- [cluster NodeManager port 8088](http://80.2.129.22:8088/cluster)

passwordless

```bash
ssh-copy-id -i ~/.ssh/id_rsa nuggetuser@80.2.129.22
```

specific different port

```bash
ssh-copy-id -i ~/.ssh/id_rsa -p 27 nuggetuser@80.2.129.22
```

download hadoop

```bash
sudo wget https://dlcdn.apache.org/hadoop/common/stable/hadoop-3.3.6.tar.gz
```

```bash
sudo tar -zxvf hadoop-3.3.6.tar.gz
```

```bash
sudo rm -r /usr/local/hadoop
```

```bash
sudo mv hadoop-3.3.6 /usr/local/hadoop
```

```bash
sudo chown -R paul /usr/local
```

```bash
sudo apt install default-jdk
```

findout real Java PATH

```bash
cd $(dirname $(readlink -f $(which java)))
```

```bash
pwd
```

```bash
which java
```

export PATH

```bash
export PATH="/usr/lib/jvm/java-11-openjdk-amd64/bin:$PATH"
```

```bash
echo $PATH
```

```bash
export HADOOP_OPTS="-Djava.net.preferIPv4Stack=true"
```

## Hadoop lessons

```bash
sudo nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh
```

add JAVA_HOME

```bash
# The java implementation to use. By default, this environment
# variable is REQUIRED on ALL platforms except OS X!
# export JAVA_HOME=
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```

uncomment IPv4 usage

```bash
# Extra Java runtime options for all Hadoop commands. We don't support
# IPv6 yet/still, so by default the preference is set to IPv4.
export HADOOP_OPTS="-Djava.net.preferIPv4Stack=true"
```

```bash
sudo nano /usr/local/hadoop/etc/hadoop/core-site.xml
```

```xml
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
  </property>
  <property>
    <name>hadoop.tmp.dir</name>
    <value>/usr/local/hadoop/tmp</value>
  </property>
</configuration>
```

```bash
sudo nano /usr/local/hadoop/etc/hadoop/hdfs-site.xml
```

Single node and no expantion latter

```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
</configuration>
```

Multi-node with expantion latter

```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>

  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/home/paul/hadoop-data/namenode</value>
  </property>
</configuration>
```

```bash
sudo nano /usr/local/hadoop/etc/hadoop/mapred-site.xml
```

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
</configuration>
```

```bash
sudo nano /usr/local/hadoop/etc/hadoop/yarn-site.xml
```

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_HOME,PATH,LANG,TZ,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>
```

```bash
sudo mkdir -p /usr/local/hadoop/tmp
```

```bash
sudo chown -R nuggetuser /usr/local/hadoop/tmp
```

```bash
sudo chown -R nuggetuser:hadoop /usr/local/hadoop 
```

```bash
hdfs namenode -format
```

```bash

start-dfs.sh
start-yarn.sh
jps
start-all.sh
stop-all.sh
```

## Multi-Cluster setup

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub nuggetuser@HNData1
```

```bash
sudo nano /etc/hosts
```

```bash
192.168.0.19 hnname
192.168.0.70 hn2ndname
192.168.0.71 hndata1
192.168.0.72 hndata2
192.168.0.73 hndata3
```

```bash
mapred.job.tracker
hnname
```

- config NameNodes masters. `hdpcore`, `hdphdfs`, `hdpmapred` and `hdpyarn`

`hdpcore` NameNode

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://hnname:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/hadoop/tmp</value>
    </property>
</configuration>
```

`hdpcore` NameNode2

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://hn2ndname:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/hadoop/tmp</value>
    </property>
</configuration>
```

`hdphdfs`

```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>3</value>
  </property>

  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/home/nuggetuser/hadoop-data/namenode</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir2</name>
    <value>/home/nuggetuser/hadoop-data/namenode2</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>/home/nuggetuser/hadoop-data/datanode1</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir2</name>
    <value>/home/nuggetuser/hadoop-data/datanode2</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir3</name>
    <value>/home/nuggetuser/hadoop-data/datanode3</value>
  </property>
</configuration>
```

`hdpmapred`

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
    <property>
        <name>yarn.app.mapreduce.am.resource.mb</name>
        <value>1024</value>
    </property>
</configuration>
```

`hdpyarn`

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_HOME,PATH,LANG,TZ,HADOOP_MAPRED_HOME</value>
    </property>
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>hn2ndname</value>
    </property>
    <property>
        <name>yarn.resourcemanager.webapp.address</name>
        <value>hn2ndname:8088</value>
    </property>
    <property>
        <name>yarn.resourcemanager.resource.memory-mb</name>
        <value>3072</value>
    </property>
    <property>
        <name>yarn.resourcemanager.resource.cpu-vcores</name>
        <value>2</value>
    </property>
</configuration>

```

- config workers. `hdpcore` and `hdpmapred`

`hdpcore`

```xml
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://hnname:9000</value>
</property>
```

`hdpmapred`

```xml
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>hnname:8032</value>
</property>
```

[Back to Top](#menu)
