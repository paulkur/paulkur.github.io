---
title: Linux Pack Installs
date: 2023-05-13 15:59:00 +0100
categories: [Linux,docker,setup]
tags: [linux,servers,docs,setup]     # TAG names should always be lowercase
---

- Linux installs

```bash
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install -y openssh-server qemu-guest-agent nano curl wget git
sudo shutdown -h now
apt-get update && apt-get upgrade
apt-get install -y openssh-server qemu-guest-agent nano curl wget git
shutdown -h now
```

- Python

```bash
sudo apt-get -y install python3-pip
# get pip and install
curl -o get_pip.py https://bootstrap.pypa.io/get-pip.py
python3 ./get_pip.py
pip install --upgrade pip
pip list --format=columns
# virtualenv
pip install virtualenv
alias mkenv="virtualenv"
alias listpyenv="py -m site"
```

- Ansible

```bash
sudo pip3 install ansible
sudo apt install ansible
ansible --version
```

- Miniconda

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh
sh ./Miniconda3-py39_4.12.0-Linux-x86_64.sh
export PATH=~/miniconda3/bin:$PATH
conda --version
conda update --all
```

- Notebook

```bash
sudo pip3 install notebook
jupyter notebook --version
source ~/.zshrc
```

- Airflow

```bash
python3 -m venv sandbox
source sandbox/bin/activate
pip install wheel
pip3 install apache-airflow==2.2.4 --constraint https://raw.githubusercontent.com/apache/airflow/constraints-2.2.4/constraints-3.9.txt
airflow db init
airflow webserver
```

- powerlevel10k

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
```

- ZSH

```bash
sudo apt install zsh
zsh --version
$ echo $SHELL
$ chsh -s $(which zsh) 
# or 
$ chsh -s /usr/bin/zsh
$ sudo apt --purge remove zsh
$ chsh -s $(which "SHELL NAME")
```

- vscode

```bash
chmod 755 ./install_vscode.sh
./install_vscode.sh
```

- FTP server

```bash
sudo apt update
sudo apt install vsftpd
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
sudo cp /etc/vsftpd.conf  /etc/vsftpd.conf_default
sudo useradd -m testuser
sudo passwd testuser
sudo ufw allow 20/tcp
sudo ufw allow 21/tcp
sudo systemctl restart vsftpd.service
sudo nano /etc/vsftpd.conf
change:
write_enable=YES
sudo systemctl restart vsftpd.service
```

- bitwarden_RS

```bash
docker pull vaultwarden/server:latest
docker run -d --name vaultwarden -v /vw-data/:/data/ -p 80:80 vaultwarden/server:latest
```

- webmin

```bash
wget https://prdownloads.sourceforge.net/webadmin/webmin_2.000_all.deb
sudo dpkg -i webmin_2.000_all.deb 
sudo apt -f install
sudo reboot
```

- IOTStack

```bash
sudo apt install -y git curl
git clone https://github.com/SensorsIot/IOTstack.git IOTstack
cd IOTstack/
sudo ./menu.sh 
sudo reboot
cd IOTstack/
./menu.sh 
docker-compose up -d
#
```

## Docker / Portainer 👇

- Docker raw install

```bash
sudo apt install snapd # needed for docker
sudo apt install docker.io
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
```

- docker-compose download/install

```bash
mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.14.0/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
docker compose version
```

- Docker without sudo

```bash
sudo usermod -aG docker ${USER}
su - ${USER}
sudo usermod -aG docker username
```

```bash
sudo addgroup --system docker
sudo adduser $USER docker
newgrp docker
sudo snap disable docker
sudo snap enable docker
docker run hello-world
```

- cleanup (reset.sh)

```bash
docker system prune -f
docker volume prune -f
docker network prune -f
rm -rf ./mnt/postgres/*
docker rmi -f $(docker images -a -q)
```

- Install Portainer 👇

```bash
docker pull portainer/portainer-ce:latest
docker run -d -p 9000:9000 -p 8000:8000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
docker restart portainer
```

- Remove Portainer

```bash
docker stop portainer && sudo docker rm portainer
```