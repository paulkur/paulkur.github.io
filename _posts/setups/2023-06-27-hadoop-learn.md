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
alias hdpstart="/usr/local/hadoop/sbin/start-all.sh"
alias hdpstop="/usr/local/hadoop/sbin/stop-all.sh"
alias hdpstartdfs="/usr/local/hadoop/sbin/start-dfs.sh"
alias hdpstopdfs="/usr/local/hadoop/sbin/stop-dfs.sh"
alias hdpfs="hadoop fs"
# -moveFromLocal
# -copyToLocal
alias hdpmd="hadoop fs -mkdir -p /user/nuggetuser/"
alias hdpmdp="hadoop fs -mkdir -p hdfs://hnname:9000/data/small"
# yarn --daemon start resourcemanager
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



### extras

move portainer to different port

```bash
docker run -d -p 9001:9000 -p 8001:8000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
docker restart portainer
```




[Back to Top](#menu)
