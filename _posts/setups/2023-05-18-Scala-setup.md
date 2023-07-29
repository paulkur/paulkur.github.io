---
title: Scala Setup
date: 2023-05-18 10:56:00 +0100
categories: [software,setup,github]
tags: [software,docs,setup]     # TAG names should always be lowercase
pin: true
---

👩‍💻

## Menu

- [Menu](#menu)
  - [To set up an IntelliJ project properly using Scala, Spark and sbt](#to-set-up-an-intellij-project-properly-using-scala-spark-and-sbt)
  - [Set up Scala for functional programming](#set-up-scala-for-functional-programming)
  - [On Windows](#on-windows)
  - [On Linux](#on-linux)
  - [Git Project init](#git-project-init)
  - [Gitlab \& Github sync](#gitlab--github-sync)
  - [Git setup - Workflow](#git-setup---workflow)

Getting started with Scala 👩‍💻: (Please use Spark 2.4.8, Scala 2.12.10 and JDK1.8)

Useful links:

- If you require an IDE (Integrated Development Environment) we suggest [getting started with IntelliJ Scala](https://docs.scala-lang.org/getting-started/intellij-track/getting-started-with-scala-in-intellij.html)

### To set up an IntelliJ project properly using Scala, Spark and sbt

- Download and install JDK 1.8 from [here](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

If you are on **Windows only** you can follow these optional steps to check you have installed JDK correctly.
**Note:** Software versions are important

- Add JAVA_HOME environment variable [steps here](https://confluence.atlassian.com/doc/setting-the-java_home-variable-in-windows-8895.html)

Check you have installed JDK correctly

```powershell
java -version
```

You should see something similar to this (your version number may not end in 333, but it should start with 1.8.0_):

```powershell
java version "1.8.0_371"
Java(TM) SE Runtime Environment (build 1.8.0_371-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.371-b11, mixed mode)
```

- Install IntelliJ Community Edition [from here](https://www.jetbrains.com/idea/download/) (any version should work)
- [follow this guide](https://docs.scala-lang.org/getting-started/intellij-track/building-a-scala-project-with-intellij-and-sbt.html). **Use Scala 2.12.10 when working through**
- It is likely you will want to use Spark code to read in and write out your data. To add Spark to
your project, you need to add Spark as a dependency. **Please use Spark version 2.4.8 as
specified in the following code snippet**. Add the following to your `build.sbt` in Intellij:

```scala
libraryDependencies ++= Seq(
"org.apache.spark" %% "spark-core" % "2.4.8",
"org.apache.spark" %% "spark-sql" % "2.4.8"
)
```

- To check your setup, hit the little green hammer in the top-right to build your project. Once
it has built, you should see a little green tick in the "Build" window at the bottom of the IDE.
If your build fails please check you have followed each of these steps correctly.

We suggest getting started with IntelliJ Scala. Scala is a compiled programming language, meaning
the code is built before runtime, unlike Python for example. This means a properly set up Scala
project requires a build tool. The most common build tool is sbt.

[Back to Top](#menu)

### Set up Scala for functional programming

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

Check you have installed Scala

```powershell
scala -version
sbt scalaVersion
```

### Git Project init

```bash
cd existing_repo
git init 
git remote add origin <project-url>
git branch -M main
git add .
```

Before push to GitLab, if project is private, you need to un-protect main branch. Open your project: `Settings > Repository` go to `Protected branches` find `master` and `Unprotected`

```bash
git commit -m "initial commit"
git push -uf origin main
```

- using zsh and power10. For help: `acs <keyword>`

```bash
cd existing_repo
git init 
gra origin <project-url>
git branch -M main
git push -u origin main
```

```bash
ga .
gcmsg "initial commit"
gp -uf origin main
```

[Back to Top](#menu)

### Gitlab & Github sync

```bash
git remote add gitlab <gitlab_repository_URL>
git pull gitlab main --allow-unrelated-histories
# Push changes to the primary repository (GitHub)
git push origin <branch_name>
# Push changes to the secondary repository (GitLab)   
git push gitlab <branch_name>
```

### Git setup - Workflow

Create new branch and do your work

```bash
git checkout -b <new-branch-name> ## examples: paulk-dev-branch, update-ip
git checkout -b paulk-dev-branch 
git status
git add .
git commit -m "<add-message-here>" ## examples: "adding proxmox VMs"
git push -u origin <branch_name>
```

Go to gitlab.com, create pull request, add message of what you did. When you fix something, you run:

```bash
git diff
git add .
git commit -m "fix(lint): cleaned up"
git push -u origin <branch_name>
```

```bash
git checkout -b feature/provisioners
git checkout -b feature/modules
```

- using zsh and power10. For help: `acs <keyword>`

```bash
gco -b <new-branch-name>
gco -b update-ip
gst
ga . # gitadd
gcmsg "<add-message-here>"
gcmsg "adding proxmox VMs"
ggpush
```

```bash
gd  # look for differences - git diff
ga . # gitadd
gcmsg "fix(lint): cleaned up"
ggpush
```

[Back to Top](#menu)
