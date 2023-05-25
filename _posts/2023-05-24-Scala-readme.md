---
title: Scala Readme
date: 2023-05-24 10:14:00 +0100
categories: [software,docs,scala,setup]
tags: [software,docs,scala,spark,setup]     # TAG names should always be lowercase
pin: true
---

Hi ! 👋 and Welcome

## Menu

- [Menu](#menu)
- [How to setup Scala-Spark local environment](#how-to-setup-scala-spark-local-environment)
  - [Windows users 👩‍💻](#windows-users-)
  - [MacOS \& Linux users](#macos--linux-users)
  - [Possible issues and fixes](#possible-issues-and-fixes)
- [Set up Scala for functional programming](#set-up-scala-for-functional-programming)
  - [On Windows](#on-windows)
  - [On Linux](#on-linux)

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

### MacOS & Linux users

> Windows users: Docker needs Windows Hyper-V to work.
{: .prompt-tip }

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

- To run them, just open up intelij, navigate to Scala main, and run🏃‍♀️

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

## Set up Scala for functional programming

using: Spark 2.4.8, Scala 2.12.10 and JDK 1.8

> Software versions are important
{: .prompt-tip }

- Download and install JDK 1.8 [here](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- and [Scala Direct instalation link here](https://www.scala-lang.org/download/2.12.10.html)

- **Windows only** Add JAVA_HOME environment variable [steps here](https://confluence.atlassian.com/doc/setting-the-java_home-variable-in-windows-8895.html)

Or using Coursier CLI:

- instructions will install the coursier CLI `cs` [link here](https://github.com/coursier/launchers/raw/master/cs-x86_64-pc-win32.zip). Used this [directions](https://get-coursier.io/docs/cli-installation). To install sbt [from here](https://www.scala-sbt.org/1.x/docs/Installing-sbt-on-Windows.html).

### On Windows

```powershell
Invoke-WebRequest -Uri "https://github.com/coursier/launchers/raw/master/cs-x86_64-pc-win32.zip" -OutFile "cs-x86_64-pc-win32.zip"
Expand-Archive -Path "cs-x86_64-pc-win32.zip"
Rename-Item -Path "cs-x86_64-pc-win32.exe" -NewName "cs.exe"
Remove-Item -Path "cs-x86_64-pc-win32.zip"
.\cs --help
```

For specific Scala version change `scala:2.12.10` accordingly `scala:<SCALA_VERSION>`:

```powershell
.\cs install scala:2.12.10
```

```powershell
.\cs install scalac:2.12.10
```

```powershell
.\cs install sbt
```

Check you have installed Scala

```powershell
scala -version
sbt scalaVersion
```

- Adding PATH

- for JDK [steps here](https://www.webucator.com/article/how-to-set-path-from-java_home/)

Java PATH on Windows:

```powershell
setx /m JAVA_HOME "C:\Program Files\Java\jdk-1.8"
```

Check env and that you have installed JDK correctly

```powershell
echo %JAVA_HOME%
```

```powershell
java -version
```

[Back to Top](#menu)

### On Linux

```bash
# On x86-64 (aka AMD64)
$ curl -fL "https://github.com/coursier/launchers/raw/master/cs-x86_64-pc-linux.gz" | gzip -d > cs
```

```bash
# On ARM64
$ curl -fL "https://github.com/VirtusLab/coursier-m1/releases/latest/download/cs-aarch64-pc-linux.gz" | gzip -d > cs
```

```bash
chmod +x cs
```

For specific Scala version change `scala:2.12.10` accordingly `scala:<SCALA_VERSION>`:

```bash
./cs install scala:2.12.10
```

```bash
./cs install scalac:2.12.10
```

```bash
./cs install sbt
```

To check Scala versions

```bash
scala -version
```

```bash
sbt scalaVersion
```

Installing latest version!

```bash
./cs setup
```

- Adding PATH

- for JDK [steps here](https://www.webucator.com/article/how-to-set-path-from-java_home/)

Java PATH on Ubuntu Linux

```bash
java -version
```

```bash
sudo apt install default-jdk
```

```bash
sudo nano /etc/environment
```

export path Ubuntu

```bash
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0
```

```bash
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0_372-b07
```

- PATH & Environments for Scala

If installed Scala from `.msi`:

| Environment  | Variable     | Value (example)         |
|:-------------|:-------------|------------------------:|
| Unix         | $SCALA_HOME  | /usr/local/share/scala  |
|              | $PATH        | $PATH:$SCALA_HOME/bin   |
| Windows      | %SCALA_HOME% | c:\Progra~1\Scala       |
|              | $PATH        | %PATH%;%SCALA_HOME%\bin |

If installed Scala from Coursier `CLI`:

| Environment  | Variable     | Value (example)                           |
|:-------------|:-------------|------------------------------------------:|
| Unix         | $SCALA_HOME  | /usr/local/coursier                       |
|              | $PATH        | $PATH:$SCALA_HOME/bin                     |
| Windows      | %SCALA_HOME% | C:\Users\Paul\AppData\Local\Coursier\data |
|              | $PATH        | %PATH%;%SCALA_HOME%\bin                   |

[Back to Top](#menu)
