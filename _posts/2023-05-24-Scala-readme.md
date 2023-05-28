---
title: Scala Readme
date: 2023-05-24 10:14:00 +0100
categories: [software,docs,scala,setup]
tags: [software,docs,scala,spark,setup]     # TAG names should always be lowercase
pin: false
---

Hi ! 👋 and Welcome

## Menu

- [Menu](#menu)
- [How to setup Scala-Spark local environment](#how-to-setup-scala-spark-local-environment)
  - [Windows users 👩‍💻](#windows-users-)
  - [MacOS \& Linux users 👩‍💻](#macos--linux-users-)
  - [Possible issues and fixes](#possible-issues-and-fixes)

## How to setup Scala-Spark local environment

### Windows users 👩‍💻

- Install Docker 🐳 for [Windows](https://docs.docker.com/desktop/install/windows-install/)

> Windows users: Docker needs Windows Hyper-V to work.
{: .prompt-tip }

- In a terminal window, navigate to the location of the `assignment.zip`

> Replace `<USERNAME>` with your PC username
{: .prompt-tip }

```powershell
cd C:\Users\<USERNAME>\Downloads
```

and run:

```powershell
Expand-Archive -Path "assignment.zip"
Remove-Item -Path "assignment.zip"
cd \assignment\spark-cluster
.\build-images.bat
```

- Spin up the Spark 💥 containers with 3 worker nodes `spark-workers` (can change `3` to more or less, depending on hardware)

```powershell
docker-compose up -d --scale spark-worker=3
```

> A Note For Windows users: Adding Winutils
> By default, Spark will be unable to write files using the local Spark executor. To write files, you will need to install the Windows Hadoop binaries, aka [winutils](https://github.com/cdarlint/winutils). Used Hadoop 2.7 as a fallback.
> After you download winutils.exe, create a directory anywhere (e.g. `C:\\winutils`), then create a `bin` directory under that, then place the winutils executable there.
> You will also need to set the `HADOOP_HOME` environment variable to your directory where you added `bin\winutils.exe`. In the example above, that would be `C:\\winutils`.
> An alternative to setting the environment variable is to add this line at the beginning of every Spark application we write:

```scala
System.setProperty("hadoop.home.dir","C:\\hadoop") // replace C:\\hadoop with your actual directory
```

{: .prompt-tip }

- Open with IntelliJ as an SBT project

Folder 📂 structure is as follows:

```tree
assignment
  src
   main
     scala
       prod
        model.scala
        q1.scala
        q2.scala
        q3.scala
        q4.scala
```

- CSV output files can be found in:

```tree
assignment
  src
   main
     resources
       data
         exports
          names-in-projects
          names-in-projects
          names-in-projects
```

- To run them, just open up intelij, navigate to Scala main, and run🏃‍♀️

![Start the code](/assets/img/start.png){: width="972" height="589" }
_Setup for a start_

- Clean/Remove the Spark-Cluster

```bash
cd .. && .\docker-clean.sh
```

### MacOS & Linux users 👩‍💻

- Install Docker 🐳 for [MacOS](https://docs.docker.com/desktop/install/mac-install/) or [Linux](https://docs.docker.com/desktop/install/linux-install/)

- in a terminal window, navigate to the location of the `assignment.zip`

> Replace `<USERNAME>` with your PC username
{: .prompt-tip }

```bash
cd /Users/<USERNAME>/Downloads
```

and run:

> Make sure Docker application is running : Strat from UI
{: .prompt-tip }

```bash
unzip assignment.zip -d ./
rm assignment.zip
cd \assignment\spark-cluster
chmod +x build-images.sh && ./build-images.sh
```

- Spin up the Spark 💥 containers with 3 worker nodes `spark-workers` (can change `3` to more or less, depending on hardware)

```bash
docker-compose up -d --scale spark-worker=3
```

- Open with IntelliJ as an SBT project

Folder 📂 structure is as follows:

```tree
assignment
  src
   main
     scala
       prod
        model.scala
        q1.scala
        q2.scala
        q3.scala
        q4.scala
```

- CSV output files can be found in:

```tree
assignment
  src
   main
     resources
       data
         exports
          names-in-projects
          names-in-projects
          names-in-projects
```

- To run them, just open up intelij, navigate to Scala main, and run 🏃‍♀️

![Start the code](/assets/img/start.png){: width="972" height="589" }
_Setup for a start_

- Clean/Remove the Spark-Cluster

```bash
cd .. && .\docker-clean.sh
```

[Back to Top](#menu)

### Possible issues and fixes

Make sure the ownership of `~/.docker` is correct

```bash
ls -ld ~/.docker
```

```bash
sudo chown -R $USER ~/.docker
```

Add docker to your USERGROUP

MacOS

```bash
sudo dseditgroup -o edit -a $USER -t user docker
```

Linux

```bash
sudo usermod -aG docker $USER
```

[Back to Top](#menu)
