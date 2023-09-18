---
title: Others Windows
date: 2023-05-12 10:59:00 +0100
categories: [homelab,hardware,setup,wsl,windows,dev]
tags: [windows,servers,docs,wsl,setup,dev]     # TAG names should always be lowercase
pin: false
---

## Windows Gitlab

```powershell
## batch comand to copy files 
xcopy /s c:\source d:\target
copy children-folder\file.something .\other-children-folder
copy .\Zorro C:\Users\Administrator\Desktop
```

```powershell
## Install
cd C:\GitLab-Runner
.\gitlab-runner.exe install
.\gitlab-runner.exe start
```

```powershell
## Uninstall
cd C:\GitLab-Runner
.\gitlab-runner.exe stop
.\gitlab-runner.exe uninstall
cd ..
rmdir /s GitLab-Runner
```

```powershell
### DON'T KNOW !
@echo off
setlocal enableextensions
setlocal enableDelayedExpansion
set nl=^

echo Running on %COMPUTERNAME%...

mkdir C:\Users\Administrator\Desktop\Zorro
xcopy /s .\Zorro C:\Users\Administrator\Desktop\Zorro

Invoke-WebRequest http://www.example.com/package.zip -OutFile package.zip

Invoke-WebRequest https://drive.google.com/uc?export=download&id=1D8_oL1FtfCHT67A-ybjm1MxdSgUjqXQz -OutFile BTC-History.zip

https://we.tl/t-d5hnV7rCc2
https://drive.google.com/file/d/1D8_oL1FtfCHT67A-ybjm1MxdSgUjqXQz/view?usp=share_link

https://drive.google.com/uc?export=download&id=1D8_oL1FtfCHT67A-ybjm1MxdSgUjqXQz

powershell -command "Start-BitsTransfer -Source https://we.tl/t-d5hnV7rCc2 -Destination folder.zip"
powershell -command "Expand-Archive folder.zip folder/to/extract"

copy .\Zorro\History C:\Users\Administrator\Desktop\Zorro\History

cd C:\Users\Administrator\Desktop\Zorro
```

### Import  👇

### Export  👇

```powershell
<enter-code>
```


### IN development. Don't care for now

GitLab runners tags

```text
rob-win-runner
rob, win, serv
```

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

First we will download and install `msys2`. After that we use the series of commands to install packages and update system.

Update the package database and base packages:

```powershell
pacman -Syu
```

Update rest of the base packages:

```powershell
pacman -Su
```

- Open Msys MinGW terminal. To install gcc and g++ for C and C++:

For 64 bit

```powershell
pacman -S mingw-w64-x86_64-gcc
```

For 32 bit

```powershell
pacman -S mingw-w64-i686-gcc
```

- To install the debugger ( gdb ) for C and C++

For 64 bit

```powershell
pacman -S mingw-w64-x86_64-gdb
```

For 32 bit

```powershell
pacman -S mingw-w64-i686-gdb
```

- To check:

```powershell
gcc version : gcc --version
```

```powershell
g++ version : g++ --version
```

```powershell
gdb version : gdb --version
```

After installing these programs, we need to set the Path environment variable.

[Back to Top](#readme-menu)
