# SHELL setup
[Full Instructions](https://www.howtogeek.com/258518/how-to-use-zsh-or-another-shell-in-windows-10/)
- [Install Oh My Zsh!](#Install-Oh-My-Zsh!)
- [Aliases](#Aliases)
- [Install DBT](#DBT-Install)
- [Python PATH setting](#Python-PATH-setting)
- [Install Airflow](#Airflow-Install)
***
## Install Oh My Zsh!
    sudo apt-get install curl git zsh
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
Launch Zsh `zsh` Make Bash Auto Launch Zsh `nano .bashrc`
add:

    if [ -t 1 ]; then
    exec zsh
    fi
after `# for examples` then `source ~./bashrc`
## Install PowerLevel10k

    git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
Install PowerLevel10k. `nano ~/.zshrc` add `ZSH_THEME="powerlevel10k/powerlevel10k"`
Set Meslo Nerd Font. 
In VS Code: File → Preferences → Settings (PC) or Code → Preferences → Settings (Mac), enter `terminal.integrated.fontFamily` in the search box at the top of Settings tab and set the value below to `MesloLGS NF`.

Dircolors.

    # using dircolors.ansi-dark
    curl https://raw.githubusercontent.com/seebi/dircolors-solarized/master/dircolors.ansi-dark --output ~/.dircolors
`nano ~/.zshrc` and paste this:

    ## set colors for LS_COLORS
    eval `dircolors ~/.dircolors`

## Plugins
- zsh-autosuggestions and zsh-syntax-highlighting
autosuggestions

    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
syntax-highlighting

    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
- zshrc edit file. 
add before this line {missing line} `source ~/.zshrc`

    plugins=(git aliases python ssh-agentvirtualenv)
    systemd 1password sudo git-auto-fetch gcloud

***

[Back to List](#SHELL-setup)
***
# DBT Install
1. Set default location to startup. cd to: for windows WSL `/mnt/c/Users/paul/Desktop`  OR for linux `/home/paul/`. Edit bashrc `nano ~./bashrc`. Add line `alias winhome='cd /mnt/c/Users/paul/'`. Save settings `source ~./bashrc`.
 - Get pip and install it
code:

    curl -o get_pip.py https://bootstrap.pypa.io/get-pip.py
if pip complains, fix PATH:

    export PATH=/home/paul/.local/bin:$PATH
    export PATH=/home/gitlab-runner/.local/bin:$PATH
install pip

    python3 ./get_pip.py
To uninstall pip, cd to env where is pip and run

    python3 -m pip uninstall pip setuptools
create project

    mkdir course && cd course
    mkdir dbt && cd dbt
    mkdir airflow && cd airflow
install virtual enviroment

    pip install virtualenv
create virtual enviroment

    [syntax]
    virtualenv [name]
    [example]
    virtualenv env
activate virtual enviroment
 
    echo "echo 'whoa'" > ./project/.env
    cd ./project
    source ./env/bin/activate > ./course/.env
    cd ./course

    source ./env/bin/activate
    which python
    pip list --format=columns 
install DBT

    pip install dbt-postgres

## Python PATH setting
Add to `nano .bashrc`. Structure `export PATH=”$PATH:/home/your_linux_username/.local/bin”`
examples

    export PATH='$PATH:/home/paul/.local/bin'
    export PATH='$PATH:/home/alexpaul/.local/bin'
    /home/paul/.local/bin
    /home/alexpaul/.local/bin
OR run this to export PATH

    export PATH=/home/paul/.local/bin:$PATH
    export PATH=/opt/airflow/.local/bin:$PATH

Activate virtual env when enter dir

    source ./env/bin/activate

[Back to List](#SHELL-setup)
***
## Airflow Install
Get aifrow.cfg best latest

    Docker exec -it airflow bash
copy .cfg to dags folder

    cp /opt/airflow/airflow.cfg /opt/airflow/dags
***
[Back to List](#SHELL-setup)