---
title: Hadoop learn
date: 2023-06-27 12:53:00 +0100
categories: [homelab,hardware,setup,Hadoop]
tags: [hadoop,servers,docs,lessons,setup]     # TAG names should always be lowercase
pin: true
---

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

```bash
ansible win -m win_ping

cp ../ansiblecvc/ansible.cfg .

ansible tower -m user -a "name=ansible" -u root -k
ansible tower -m command -a "id"
ansible tower -m command -a "ls -la /root"
```

## 👇 Lesson 2: Using Ad-hoc Commands

```bash
ansible -i inventory windows -m ansible.windows.win_ping
ansible -i inventory all -m ansible.builtin.command -a reboot
```
