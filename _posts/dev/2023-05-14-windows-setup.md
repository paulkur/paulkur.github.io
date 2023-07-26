---
title: Windows cmd snippets
date: 2023-05-14 10:59:00 +0100
categories: [Zorro,servers,Windows,cmd]
tags: [zorro,code,docs,windows]     # TAG names should always be lowercase
pin: true
---
### Links

- [Win useful cmd](#win-useful-cmd)
  - [Working with SSH-keys](#working-with-ssh-keys)
  - [Useful cmd in PowerShell](#useful-cmd-in-powershell)
  - [Working with Files \& Folders](#working-with-files--folders)
  - [Extras cmd](#extras-cmd)

Useful links👇

- [Using ssh-key based auth on Win](https://woshub.com/using-ssh-key-based-authentication-on-windows/)

## Win useful cmd

- Open-SSH Setup

```powershell
$PSVersionTable.PSVersion
```

```powershell
(New-Object Security.Principal.WindowsPrincipal([Security.Principal.WindowsIdentity]::GetCurrent())).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)
```

need to get `True`

- Install OpenSSH Client

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

- Install OpenSSH Server

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

- Start the sshd service

```powershell
Start-Service sshd
```

```powershell
Restart-service sshd
```

- Auto startup

```powershell
Set-Service -Name sshd -StartupType 'Automatic'
```

- Confirm the Firewall rule is configured. It should be created automatically by setup. Run the following to verify

```powershell
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```

[Back to Top](#links)

### Working with SSH-keys

```powershell
ssh-add "C:\Users\Administrator\.ssh\id_ed25519"
```

- add to cey to `authorized_keys`

```powershell
scp C:\Users\USERNAME\.ssh\id_ed25519.pub admin@192.168.0.5:c:\Users\Admininstrator\.ssh\authorized_keys
```

- SSH to somewhere

```powershell
ssh domain\username@server-name
```

- SSH to remote win server as Admin

```powershell
ssh domain\Administrator@server-name
```

- SSH to remote win server as Admin using local SSH key

```powershell
ssh Administrator@win-remote-ip-address -i "C:\Users\your-user\.ssh\id_ed25519"
```

- Useful cmd in PShell (Find Out and Fix!)

- Copy Other commands (find out exactly what it does!)

```powershell
Copy-Item (Join-Path $PSScriptRoot MoveIt1.txt) (Join-Path $env:USERPROFILE "\AppData\Roaming\") -Force
Copy-Item (Join-Path $PSScriptRoot MoveIt2.txt) "C:\Program Files (x86)\" -Force
Copy-Item -Path "C:\temp\files\*"  -Destination "d:\temp\"
```

[Back to Top](#links)

### Useful cmd in PowerShell

- Start notepad

```powershell
Start-Process notepad
```

- Restart-Computer

```powershell
shutdown /s
```

- Find out this one!

```powershell
Get-ChildItem -Path "C:\Program Files\Folder_Name" -Recurse | Select FullName
```

- Find out this ones too

```powershell
get-disk
get-volume
```

[Back to Top](#links)

### Working with Files & Folders

- get list of folder content

```powershell
Get-ChildItem "C:\"
```

- get list of `Program Files`

```powershell
Get-ChildItem -Path "C:\Program Files"
```

- Set working dir

```powershell
Set-Location "C:\Users\username\Documents"
```

- Copy All folder content ???

```powershell
Copy-Item "E:\Folder1" -Destination "E:\Folder2" -Recurse
```

- Move folder

```powershell
Move-Item -Path "E:\Folder1" -Destination "E:\Folder2"
```

- Copy file

```powershell
Copy-Item -Path "C:\temp\files\la-ams-ad01-log-1.txt" -Destination "d:\temp\"
```

- Copy file. `Copy` is a shorthand for Copy-Item

```powershell
Copy "C:\temp\files\la-ams-ad01-log-1.txt" "d:\temp\"
```

- Remove file

```powershell
Remove-Item E:\Folder1\Test.txt
```

- Remove folder with files

```powershell
Remove-Item -Path <path to the directory> -Force -Recurse
```

- Get file content

```powershell
Get-Content "E:\Folder1\Test.txt"
```

[Back to Top](#links)

### Extras cmd

- Get and Set ExecutionPolicy

```powershell
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned
```

- get windows PC name

```powershell
Get-Service -Name "Win*"
```

- run clipboard

```powershell
rdpclip.exe
```

- CHECK..

```powershell
Get-Service ssh-agent | Set-Service -StartupType Automatic -PassThru | Start-Service
```

```powershell
git config --global core.sshCommand C:/Windows/System32/OpenSSH/ssh.exe
```

- Match Group administrators

```powershell
#       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
AllowGroups users
```

[Back to Top](#links)
