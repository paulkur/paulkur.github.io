# Local test ENV WSL Setup
- [1st time Setup](#1st-time-Setup)
- [Backup of your Setup](#Backup-of-your-Setup)
- [GitHub setup](#GitHub-setup)
- [Optionals](#Optionals)
***
## WSL Setup 
If you don't have WSL setup on your PC.[Download WSL2 env file](https://drive.google.com/file/d/1-G_a9WhgipCXlXvW6R75VI4aU9gkInQ0/view?usp=share_link)
unzip in `C:\Users\%username%\Documents\wsl_setup\wsl_backups\`
1. windows key -> type `features` -> open `Turn Windows features on or off`
2. enable `Virtual Machine Platform` and `Windows subsystem for Linux`. Restart PC
3. go to `Microsoft Store` -> search: `Windows subsystem for Linux` (blue penguin) -> Install it if it's not installed

**If you don't want to use "all-ready" downloaded image, then follow this:**
4. go to `Microsoft Store` >> search for `Ubuntu` >> Install it
5. run ubuntu: Windows key >> type `Ubuntu` >> run installed `Ubuntu`

## GitHub setup
[Have latest git x64 installed from here](https://git-scm.com/download/win). Check PowerShell version `$PSVersionTable`.
1. In PowerShell `git config --global core.autocrlf input`
2. Check windows git config if it's on your github account 
```
git config --list --show-origin
```
## start WSL (here run all commands, NOT "Shutdown")
1. import WSL
```
wsl --import Ubuntu_env C:\Users\Paul\wsl_setup\wsl_current\Ubuntu_env C:\Users\Paul\wsl_setup\wsl_backups\Ubuntu_env_manual_backup.tar --version 2
```
2. list all `wsl -l -v`
3. set default WSL 2 `wsl --set-default-version 2`
4. Correct Windows registry
winddows key >> type `regedit` >> navigate to `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss` and find folder with `--import <Image Name>` (here it's Ubuntu_env). Go to `DefaultUid` and change to decimal 1000 not hexidecimal! For smooth backups and Imports/exports using commands unzip download in `C:\Users\%username%\Documents\`. At the end should look like this `C:\Users\%yourpcusername%\Documents\wsl_setup\wsl_backups\`
5. Set default `wsl --setdefault Ubuntu_env`
6. Start `wsl` or `wsl -d <nameOfImage>`
Shutdown `wsl --shutdown Ubuntu_env`. Remove `wsl --unregister Ubuntu_env`
7. `sudo apt update && sudo apt upgrade`
7. Fix git input `git config --global core.autocrlf input`
8. [IMPORTANT] from step 3. Git Credential Manager setup. [more details](https://learn.microsoft.com/en-gb/windows/wsl/tutorials/wsl-git). This will manage git credentials for WSL. If in Windows Git installed is >= v2.36.1 then run this in WSL:
```
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager-core.exe"
```
else if version is < v2.36.1
```
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"
```
9. Check git config `git config --list --show-origin`. If not your ID: Git Credential Manager setup failed. Run previous comand again. Restart WSL.
This cmd is for manual setup help on Windows side.
```
git config --global user.name paulkur
git config --global user.email paul.kurpis@gmail.com
```
10. Test if it clones gitlab repo:
```
git clone https://<YourGitLabUserName>:<YourPersonalAccessToken>@gitlab.com/3m6/onchain_analysis/airflow-pipeline.git
alex
git clone https://azertyui74:<YourPersonalAccessToken>@gitlab.com/3m6/onchain_analysis/airflow-pipeline.git

paul main
git clone https://paulkurpis:glpat-FphSsSrUVmBXu6L5eqkQ@gitlab.com/3m6/onchain_analysis/airflow-pipeline.git
paul test
git clone https://paulkurpis:glpat-FphSsSrUVmBXu6L5eqkQ@gitlab.com/3m6/onchain_analysis/pk-dev-airflow-pipeline.git
```
11. clone `airflow-pipeline` repo. Change to `pk-test-branch`. cd to `cd dev`. Run `start`.

**DONE!**
To don't repeat this everytime, **NOW** backup this WSL image as your own setup and keep it for other machines. Go to [Backup of your Setup](#Backup-of-your-Setup) make it as YOUR image and store somewhere safe.
***





## Backup of your Setup
DO IT AFTER Github setup!
in PowerShell, list all WSLs `wsl -l -v` if there is any Ubuntu running - shutdown `wsl --shutdown Ubuntu_env` and remove `wsl --unregister <nameOfIt>`
- Export SYNTAX `wsl --export <Image Name> <Export location file name.tar>`
for local `wsl_backups`:
```
wsl --export Ubuntu_env C:\Users\paul\wsl_setup\wsl_backups\Ubuntu_env_manual_backup.tar
```
server side in `wsl_setup` (change IP to your server local IP):
```
wsl --export Ubuntu_env \\192.168.1.164\toshiba3\wsl_setup\Ubuntu_env_manual_backup-2.tar
remove OLD Ubuntu env before Import `wsl --unregister Ubuntu_env`
```
- Import SYNTAX `wsl --import <Image Name> <Directory where you want to store the imported image> <Directory where the exported .tar file exists>`
for local `wsl_backups`:
```
wsl --import Ubuntu_env C:\Users\paul\wsl_setup\wsl_current\Ubuntu_env C:\Users\paul\wsl_setup\wsl_backups\Ubuntu_env_manual_backup-2.tar --version 2
```
[Correct Registry](#Correct-registry-before-start)
List wsl to see default one and import succsess `wsl -l -v`. Set default image (if you have more in) `wsl --setdefault Ubuntu_env`. Start WSL `wsl -d Ubuntu_env` type `home` to get to linux home dir. OR: Windows key >> type "linux" >> run the penguin. To enter WSL in VScode `code .`. CHANGE PASSWORD ! In linux shell `passwd`. 


***
# Optionals
## 1. (sugested) VScode integration  (for first time setup only)
    get WSL for VScode to control from VScode terminal.
    The Visual Studio Code Remote - WSL extension will let you use VSCode on Windows to interact with WSL.  
    [WSL for VScode to control]:(https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
    
## 2. WSL auto-backup

in PowerShell

1. Create file and call it: `wsl_backup.ps1`
2. write in that file this (change location accordingly to yours! where to store backup):
    `wsl --export Ubuntu-env-backup C:\Users\paul\Documents\wsl_setup\wsl_backups\Ubuntu-env-backup.tar`

    in CMD
    This script will update the execution policy to allow task scheduler to run the PowerShell script you created in the previous step and it will then run the powershell script we called "wsl_backup.ps1"

    a.
    PowerShell -Command "Set-ExecutionPolicy Unrestricted"
    PowerShell C:\Users\paul\Documents\wsl_setup\wsl_backups\wsl_backup.ps1

    b. setup Task Scheduler to run the CMD script once a week

    windows key >> type "Task Scheduler" >> Action (top menu) >> "Create Task"

    Description:
    Task to reset LXSS Manager for WSL in order for apps such as Jupyter to launce successfully in host browser.

    Triggers tab:
    "New.."
    set the time for backup

    Actions tab:
    Program/script: (point to created before file)
    C:\Users\paul\Documents\wsl_setup\wsl_backups\wsl_backup.ps1

    Conditions tab:
    select:
    "Start the task only if the computer is on AC power"

    -------- DONE ! ---------
    This backup process only takes a few minutes and means at worst you will only lose changes made that day to files in your Linux distribution.
    This Overwrites the previous week backup file with no issues, means keeping always the latest version.
[Back to List](#Local-ENV-Setup)
***
