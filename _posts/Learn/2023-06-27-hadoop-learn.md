---
title: Hadoop learn
date: 2023-06-27 12:53:00 +0100
categories: [homelab,hardware,setup,Hadoop]
tags: [hadoop,servers,docs,lessons,setup]     # TAG names should always be lowercase
pin: true
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
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```

```bash
export HADOOP_OPTS="-Djava.net.preferIPv4Stack=true"
```

## Hadoop lessons

```bash
sudo nano hadoop-env.sh
```

```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```

```bash
export HADOOP_OPTS="-Djava.net.preferIPv4Stack=true"
```

```bash
sudo nano /usr/local/hadoop/etc/hadoop/core-site.xml
```

```xml
<property>
<name>fs.defaultFS</name>
<value>hdfs://localhost:9000</value>
</property>

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
    <name>dfs.namenode.name.dir</name>
    <value>/home/paul/hadoop-data/namenode</value>
  </property>
  
  <property>
    <name>dfs.replication</name>
    <value>1</value>
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

### win commands

move portainer to different port

```bash
docker run -d -p 9001:9000 -p 8001:8000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
docker restart portainer
```

[Back to Top](#menu)
