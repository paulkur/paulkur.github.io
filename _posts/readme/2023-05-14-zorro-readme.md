---
title: Zorro Readme
date: 2023-06-11 11:59:00 +0100
categories: [servers,Zorro,setup,Windows]
tags: [docs,zorro,setup,windows]     # TAG names should always be lowercase
pin: true
---
Useful links👇

- [Financial Hacker-main](https://financial-hacker.com/)
- [F-Hacker-Build-Better-Strategies!](https://financial-hacker.com/build-better-strategies/)
- [F-Hacker-Trend_Experiment](https://financial-hacker.com/trend-delusion-or-reality/)
- [Zorro-Manual](https://zorro-project.com/manual/)
- [Zorro-Workshops](https://zorro-project.com/manual/en/tutorial_var.htm)
- [Zorro-and some...](https://zorro-project.com/manual/en/tutorial_var.htm)
- [Jetbrains Repo link](https://hpda.jetbrains.space/p/main/repositories/zorro/files/README.md)

Get Data links👇

- [CoinAPI Documentation](https://docs.coinapi.io/market-data/rest-api)
- [Crypto Data download.com link](https://www.cryptodatadownload.com/data/binance/)

## Readme menu

- [Readme menu](#readme-menu)
  - [Workflow](#workflow)
  - [Cleaning Data](#cleaning-data)
  - [To download and clean CSV from web](#to-download-and-clean-csv-from-web)
  - [Project Setup for data](#project-setup-for-data)
    - [macOS / Linux](#macos--linux)
    - [Windows](#windows)
  - [Zorro setup](#zorro-setup)
- [IN development](#in-development)

### Workflow

Create new branch and do your work

```bash
git checkout -b <new-branch-name> ## examples: paulk-dev-branch, update-ip
git checkout -b robs-dev-branch 
git status
git add .
git commit -m "adding coinapi connection" ## examples: "adding proxmox VMs"
git push -u origin <branch_name> # will push to Github
git push -u gitlab <branch_name> # will push to GitLab
git push -u gitlab main
```

### Cleaning Data

- Location for downloaded and data that we have and no need to download can be found in Google Drive under: `\Shared drives\HPDA shared drive\Resources\zorro-data\from-web\` path and navigate to see what we have so far.

### To download and clean CSV from web

- Go [Here](https://www.cryptodatadownload.com/data/binance/) and create free account if have one - login.
- in section `SYMBOL LIST AND FILES FOR BINANCE SPOT HOURLY/MINUTE TIMEFRAME(S)` Search for `BTCUSDT` and download `minute` `.csv` file. Try to save dirrectly in projects `raw` folder that should look somethink like this: `...\<PROJECT_REPO_LOCATION>\scala-spark\src\main\resources\data\raw`.
- in CSV file, the first line is disturbing - just delete manualy this text `https://www.CryptoDataDownload.com`
- first CSV file line have to be `unix,date,symbol,open,high,low,close,Volume BTC,Volume USDT,tradecount`


### Project Setup for data

1. Clone repo. Use the link stored in Bitwarden secure note called `Clone Zorro`. It contains secure token.

2. Extract Zorro zip

#### macOS / Linux

```bash
unzip Zorro.zip -d ./
rm Zorro.zip
```

3. Prepare Spark cluster

```bash
cd scala-spark
docker compose up -d
```

```bash
cd spark-cluster
ls
chmod +x build-images.sh
./build-images.sh
```

```bash
docker compose up -d --scale spark-worker=3
```

#### Windows

Windows OLNY!

Hadoop setup

- download Hadoop version 3.2.* [from here](https://github.com/cdarlint/winutils)
- unzip Hadoop in `C:\` easy accessable place
- set system environment variable:
  - `HADOOP_HOME` and `C:\hadoop-3.2.2`
  - then for PATH: `%HADOOP_HOME%\bin`

```powershell
Expand-Archive -Path "Zorro.zip"
Remove-Item -Path "Zorro.zip"
```

might need to reboot. Next follows same as for Macs or Linux

```powershell
cd scala-spark
docker compose up -d
```

```powershell
cd spark-cluster
ls
chmod +x build-images.sh
.\build-images.sh
```

```powershell
docker compose up -d --scale spark-worker=3
```

[Back to Top](#readme-menu)

### Zorro setup

1. Copy Zorro.ini file to `c:\Users\<username>\Zorro\History\`

2. [Download Data files Here](https://drive.google.com/drive/folders/18pgkha-lJdYKEz5vwGyM9AoOzfD6pXoG?usp=share_link) and unzip it in: `c:\Users\<username>\Zorro\History\`

3. When you want to run any script (example CSVtoHistory) you need to move file from folder to `Strategy` folder, then will see it in Zorro's dropdown list.


[Back to Top](#readme-menu)

## IN development

Setup windows dev environment for C++

1. download and install mingw-64 [from here](https://www.mingw-w64.org/)
2. Install `gcc`

```powershell
pacman -S mingw-w64-ucrt-x86_64-gcc
```

then

```powershell
pacman -S mingw-w64-x86_64-gcc
```

First we will download and install msys2.
After that we use the series of commands to install packages and update system.

Commands used :
Update the package database and base packages using
pacman -Syu

Update rest of the base packages
pacman -Su

Now open up the Msys MinGW terminal
To install gcc and g++ for C and C++
For 64 bit
pacman -S mingw-w64-x86_64-gcc
For 32 bit
pacman -S mingw-w64-i686-gcc

To install the debugger ( gdb ) for C and C++
For 64 bit
pacman -S mingw-w64-x86_64-gdb
For 32 bit
pacman -S mingw-w64-i686-gdb

To check
gcc version : gcc --version
g++ version : g++ --version
gdb version : gdb --version

After installing these programs, we need to set the Path environment variable.

[Back to Top](#readme-menu)