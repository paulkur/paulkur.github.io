---
title: Windows WSL
date: 2023-05-13 11:59:00 +0100
categories: [homelab,hardware,setup,wsl,windows,dev]
tags: [windows,servers,docs,wsl,setup,dev]     # TAG names should always be lowercase
pin: false
---

## Windows SSH setup 1

- Install `openSSH Server` and `openSSH Client` from `Settings -> Apps -> Optional Features`

connect to OpenSSH Server from a Windows or Windows Server device with the OpenSSH client installed.

```powershell
ssh domain\username@servername
```

```powershell
ssh robsdomain\paul@<ip-address>
```

```powershell
# Set the sshd service to be started automatically
Get-Service -Name sshd | Set-Service -StartupType Automatic

# Now start the sshd service
Start-Service sshd
```

generate new key

```powershell
ssh-keygen -t ed25519
``````

```powershell
# By default the ssh-agent service is disabled. Configure it to start automatically. 
# Make sure you're running as an Administrator.
Get-Service ssh-agent | Set-Service -StartupType Automatic

# Start the service
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

- On Windows, the ssh-copy-id command is not available, use this:

```powershell
cat ~/.ssh/id_ed25519.pub | ssh paul@192.168.1.228 "cat >> ~/.ssh/authorized_keys"
```

ssh using that key

```powershell
ssh -i ~/.ssh/id_ed25519 paul@192.168.1.169
# OR if naming is standart
ssh paul@192.168.1.169
```

IF port is different:

```powershell
cat ~/.ssh/id_ed25519.pub | ssh -p 23 paul@80.2.242.41 "cat >> ~/.ssh/authorized_keys"
```

```powershell
ssh -i ~/.ssh/id_ed25519 -p 23 paul@192.168.1.169
# OR if naming is standart
ssh -p 23 paul@192.168.1.169
```

## Windows openSSH setup 2

```powershell
$PSVersionTable.PSVersion
```

need to get "True" here 👇

```powershell
(New-Object Security.Principal.WindowsPrincipal([Security.Principal.WindowsIdentity]::GetCurrent())).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)
```

```powershell
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

```powershell
Start-Service sshd
```

```powershell
Set-Service -Name sshd -StartupType 'Automatic'
```

```powershell
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```

## [Chocolatey](https://chocolatey.org/install#individual) and [Sophi App](https://github.com/Sophia-Community/SophiApp) install

```powershell
Get-ExecutionPolicy
```

If it returns Restricted, then run

```powershell
Set-ExecutionPolicy AllSigned 
```

or

```powershell
Set-ExecutionPolicy Bypass -Scope Process
```

Install Choco

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

Install Sophi app

```powershell
choco install sophiapp --confirm
```


## WSL quick Restore

```powershell
wsl -l -v
wsl --shutdown Ubuntu_env
wsl --unregister Ubuntu_env
wsl --setdefault Ubuntu_env
```

put `wsl_setup` folder in `C:\Users\paul\`{: .filepath}

- `Turn Windows features on or off` enable `Virtual Machine Platform` and `Windows subsystem for Linux`. Restart PC
- `Microsoft Store` >> `Windows subsystem for Linux` (blue penguin) >> Install it >> run `Ubuntu` from (<kbd>Win</kbd>), click

### Import  👇

```powershell
wsl --import Ubuntu_env C:\Users\paul\wsl_setup\wsl_current\Ubuntu_env C:\Users\paul\wsl_setup\wsl_backups\Ubuntu_env_backup.tar --version 2
```

### Export  👇

```powershell
wsl --export Ubuntu_env C:\Users\paul\wsl_setup\wsl_backups\Ubuntu_env.tar
```

Correct registry before start. In `regedit` `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss`

Change `DefaultUid` to decimal `1000`

## First install

## Git credential manager setup  👇

for WSL to sync with Git Windows [more details](https://learn.microsoft.com/en-gb/windows/wsl/tutorials/wsl-git)

```bash
gcfg
git config --list --show-origin
git --version
```

then

```powershell
git config --global core.autocrlf input
```

then

```powershell
wsl
git config --global core.autocrlf input
```

If Windows Git version >= v2.36.1

```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager-core.exe"
```

```powershell
git config --global credential.helper "C:/Program Files/Git/mingw64/bin/git-credential-manager-core.exe"
```

if version < v2.36.1

```powershell
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"
```

## WSL2 cleanup

```bash
sudo apt clean
sudo apt autoclean
sudo apt autoremove
sudo rm -rf /var/log/*
sudo rm -rf /tmp/*
rm -rf ~/tmp/*
```

## Thinkpad Windows Re-Install

1. turn PC on, insert AC charger
2. remove AC charger and battery
3. press power button for 30 sec
4. connect AC charger without battery
5. turn on PC, boot to OS
6. hold power button, do NOT shutdown from windows
7. turn on laptop, press ENTER key and then F1 to enter BIOS
8. F9 to load default settings. F10 exit saving changes
10. restart and enter BIOS SETUP
11. F10 exit saving changes
12. Install windows. Insert usb. Shutdown windows, turn on windows, while booting enter boot menu

## WSL Init Setup

Using pre-build img [Download WSL img](https://transfer.pcloud.com/download.html?code=5ZvjJsVZEd13dGNIorzZMIdIZubCBc56QDWVQoVxBnQ26w8TzFFUV)
put it in `C:\Users\%username%\wsl_setup\wsl_backups\`{: .filepath}

- `Turn Windows features on or off` and enable `Virtual Machine Platform` and `Windows subsystem for Linux`. Restart PC
- `Microsoft Store` >> search `Windows subsystem for Linux` (blue penguin) >> Install it >> run `Ubuntu` using windows button
NOTE: If Building from scratch, after `Windows subsystem for Linux` search `Ubuntu` install and run it.

## Usefull comands in PowerShell

### Connect SSH

- generate ssh key

```powershell
ssh-keygen
ssh Administrator@...ipaddress..
```

- copy to
  
```path
C:\Users\<YourUsername>\.ssh\
```

- Connect to OpenSSH Server `ssh domain\username@servername`

```powershell
ssh -i C:\Users\Paul\.ssh\data_rsa alexpaul@192.168.1.228
```

## Other comands in PowerShell

```powershell
Start-Process notepad 
Get-Service -Name "Win*"
Get-ChildItem "C:\"
Get-ChildItem -Path "C:\Program Files"
Get-ChildItem -Path "C:\Program Files\Fodler_Name" -Recurse | Select FullName
Restart-Computer
shutdown /s
get-disk
get-volume
```

- Copy folders-files

```powershell
Copy-Item "E:\Folder1" -Destination "E:\Folder2" -Recurse
Move-Item -Path "E:\Folder1" -Destination "E:\Folder2" 
Remove-Item E:\Folder1\Test.txt
Get-Content "E:\Folder1\Test.txt"
Set-Location "C:\Users\usrename\Documents"
Copy-Item -Path "C:\temp\files\*"  -Destination "d:\temp\"
```

- terminal-Copy Item

```powershell
Copy-Item -Path "C:\temp\files\la-ams-ad01-log-1.txt" -Destination "d:\temp\"
```

```powershell
# Copy is a shorthand for Copy-Item:
Copy "C:\temp\files\la-ams-ad01-log-1.txt" "d:\temp\"
```
