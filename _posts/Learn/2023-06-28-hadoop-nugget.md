---
title: Hadoop nugget
date: 2023-06-28 12:53:00 +0100
categories: [homelab,hardware,setup,Hadoop]
tags: [hadoop,servers,docs,lessons,setup]     # TAG names should always be lowercase
pin: false
---

## Install Hadoop ubuntu

- zrc commands

```bash
## Hadoop
alias hdphome="cd /usr/local/hadoop/"
alias hdpstart="sbin/start-all.sh"
alias hdpstop="sbin/stop-all.sh"
```

- More detailed copy/paste instructions [here](https://www.geeksforgeeks.org/how-to-install-hadoop-in-linux/)
- Single node setup [here](https://hadoop.apache.org/docs/r3.3.5/hadoop-project-dist/hadoop-common/SingleCluster.html)
- Cluster setup [here](https://hadoop.apache.org/docs/r3.3.5/hadoop-project-dist/hadoop-common/ClusterSetup.html)
- Download from [here](https://dlcdn.apache.org/hadoop/common/)

ports:

[core-site port 9000](http://localhost:9000/)
[namenode port 50070](http://localhost:50070/)

[cluster NodeManager port 8088](http://localhost:8088/cluster)

```bash
wget https://dlcdn.apache.org/hadoop/common/stable/hadoop-3.3.6.tar.gz
```

```bash
tar -zxvf hadoop-3.3.6.tar.gz
```

```bash
sudo apt install default-jdk
```

findout PATH

```bash
which java
```

export PATH

```bash
export PATH="/usr/bin/java:$PATH"
```

```bash
echo $PATH
```

```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
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
export JAVA_HOME=/usr/bin/java
```

```bash
sudo nano /usr/local/hadoop/etc/hadoop/core-site.xml
```

```xml
<property>
<name>fs.defaultFS</name>
<value>hdfs://localhost:9000</value>
</property>
```

```xml
<property>
<name>hadoop.tmp.dir</name>
<value>/usr/local/hadoop/tmp</value>
</property>
```

```bash
sudo nano /usr/local/hadoop/etc/hadoop/hdfs-site.xml
```

```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>

  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/home/nuggetuser/hadoop-data/namenode</value>
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
</configuration>
```

```bash
hdfs namenode -format
```

```bash
sudo chown -R paul:paul /usr/local/hadoop 
```

```bash
jps
start-dfs.sh
start-all.sh
stop-all.sh
```

/home/paul/hadoop-data/namenode

[Back to Top](#menu)
