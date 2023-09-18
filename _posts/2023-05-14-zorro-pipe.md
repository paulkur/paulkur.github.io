---
title: Zorro pipe
date: 2023-06-11 11:59:00 +0100
categories: [servers,Zorro,setup,Windows,dev,readme]
tags: [docs,zorro,setup,windows,dev,readme]     # TAG names should always be lowercase
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
- [Strategy links](#strategy-links)
- [Git Workflow](#git-workflow)
  - [Data Environment Setup](#data-environment-setup)
  - [Data preparation](#data-preparation)
    - [from Web](#from-web)
    - [from coinAPI](#from-coinapi)
    - [for Zorro strategy dev](#for-zorro-strategy-dev)

## Strategy links

- [robs-win-serv](http://81.100.196.142/log_EMA.htm)

```text
http://81.100.196.142/
```

add strategy name at the end : example:

```text
http://81.100.196.142/log_EMA.htm
```

- [aws-win-serv](http://35.177.126.163/log_EMA.htm)

```text
http://35.177.126.163/
```

add strategy name at the end : example:

```text
http://35.177.126.163/log_EMA.htm
```

## Git Workflow

Create new branch and do your work

```bash
git checkout -b <new-branch-name> ## examples: paulk-dev-branch, update-ip
```

```bash
git checkout -b robs-dev-branch
```

```bash
git status
```

```bash
git add .
```

```bash
git commit -m "initial commit"
```

```bash
git commit -m "adding coinapi connection"
```

```bash
git push -u origin main
```

```bash
git push -u gitlab main
```

```bash
git push -u github main
```

```bash
git push -u origin <branch_name>
git push -u gitlab <branch_name>
```

### Data Environment Setup

Clone repo. Use the link stored in Bitwarden secure note called `Clone Zorro`. It contains secure token.

> Rob ! on your Windows home server, there is already Spark cluster running! Can skip it, but if you want to be able to run Spark on your mac and have flexibility to do stuff not only at home, feel free to setup your mac with Docker ar Spark cluster.
{: .prompt-warning }

Spark Cluster Setup (without Docker now)

> Windows OLNY! Hadoop setup
{: .prompt-warning }

- download Hadoop version 3.2.* [from here](https://github.com/cdarlint/winutils)
- unzip Hadoop in `C:\` easy accessable place
- set system environment variable:
  - `HADOOP_HOME` and `C:\hadoop-3.2.2`
  - then for PATH: `%HADOOP_HOME%\bin`
might need to reboot.

```bash
cd scala-spark
```

```bash
docker compose up -d
```

```bash
cd spark-cluster && ls
```

```bash
chmod +x build-images.sh
./build-images.sh
```

> Windows
{: .prompt-info }

```powershell
.\build-images.bat
```

> If you have more power in your PC can give more or less workers. 1 worker: 1gb RAM, 1 CPU core. 3 does the job
{: .prompt-info }

```bash
docker compose up -d --scale spark-worker=3
```

[Back to Top](#readme-menu)

### Data preparation

#### from Web

> Sorry for not so "sexy" setup at the moment. Just being in a rush for now.
{: .prompt-warning }

- Location for downloaded and data that we have and no need to download can be found in Google Drive under: `\Shared drives\HPDA shared drive\Resources\zorro-data\from-web\` path and navigate to see what we have so far.
- Go [Crypto Data download.com link](https://www.cryptodatadownload.com/data/binance/). Create free account if have one - login.
- In section `SYMBOL LIST AND FILES FOR BINANCE SPOT HOURLY/MINUTE TIMEFRAME(S)` Search for `BTCUSDT` and download `minute` `.csv` file. Try to save dirrectly in projects `raw` folder that should look somethink like this: `...\<PROJECT_REPO_LOCATION>\scala-spark\src\main\resources\data\raw`.
- in CSV file, the first line is disturbing - just delete manualy this text `https://www.CryptoDataDownload.com`
- first CSV file line have to be `unix,date,symbol,open,high,low,close,Volume BTC,Volume USDT,tradecount`

#### from coinAPI

> Sorry for not so "sexy" setup at the moment. Just being in a rush for now.
{: .prompt-warning }

- Location for downloaded and data that we have and no need to download can be found in Google Drive under: `\Shared drives\HPDA shared drive\Resources\zorro-data\from-web\` path and navigate to see what we have so far.
- Go [Crypto Data download.com link](https://www.cryptodatadownload.com/data/binance/). Create free account if have one - login.
- In section `SYMBOL LIST AND FILES FOR BINANCE SPOT HOURLY/MINUTE TIMEFRAME(S)` Search for `BTCUSDT` and download `minute` `.csv` file. Try to save dirrectly in projects `raw` folder that should look somethink like this: `...\<PROJECT_REPO_LOCATION>\scala-spark\src\main\resources\data\raw`.
- in CSV file, the first line is disturbing - just delete manualy this text `https://www.CryptoDataDownload.com`
- first CSV file line have to be `unix,date,symbol,open,high,low,close,Volume BTC,Volume USDT,tradecount`

[Back to Top](#readme-menu)

#### for Zorro strategy dev

> Easiest setup is Windows, so following instructions is for Windows ONLY. It's possible to run Zorro on macOS / Linux with Wine, but will not cover in here for now
{: .prompt-warning }

Extract `Zorro.zip` from project root dir

```powershell
Expand-Archive -Path "Zorro.zip"
Remove-Item -Path "Zorro.zip"
```

1. Copy Zorro.ini file to `\<PROJECT_ROOT_DIR>\Zorro\History\`

2. [Download wanted/needed .t6 Data files Here](https://drive.google.com/drive/folders/18pgkha-lJdYKEz5vwGyM9AoOzfD6pXoG?usp=share_link) and unzip it in: `\<PROJECT_ROOT_DIR>\Zorro\History\`

3. Open Zorro from `\<PROJECT_ROOT_DIR>\Zorro\Zorro.exe` and run a test drive `test_plot` script.

> All unzipped `Zorro` folder is .gitignored, so don't worry of `.t6` data uploads or `Strategy` scripts inside.
{: .prompt-info }

When you want to run any script (example CSVtoHistory) you need to move file from folder to `Strategy` folder, then will see it in Zorro's dropdown list.

[Back to Top](#readme-menu)
