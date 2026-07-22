====> to Stop and Disable unattended-upgrades:
sudo systemctl stop unattended-upgrades.service
sudo systemctl disable unattended-upgrades.service

===> Stop and Disable APT Daily Timers:
sudo systemctl stop apt-daily.timer
sudo systemctl disable apt-daily.timer
sudo systemctl stop apt-daily-upgrade.timer
sudo systemctl disable apt-daily-upgrade.timer

===> To confirm weather all timers are disabled check with below command:
systemctl list-timers | grep apt

===> Set Multi-User:
sudo systemctl set-default multi-user.target
sudo systemctl set-default multi-user

===> Set the time zone to Asia Kolkata.
sudo timedatectl set-timezone Asia/Kolkata

===> Set the Swap Space to 64GB
To increase the swap space on your Ubuntu VM to 64GB:
Check the current swap space: sudo swapon --show
Turn off current swap: sudo swapoff -a
Create a new swap file: sudo dd if=/dev/zero of=/swapfile bs=1G count=64
Set permissions: sudo chmod 600 /swapfile
Set up the swap space: sudo mkswap /swapfile
Enable the swap file: sudo swapon /swapfile
Make the change permanent: echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
Verify the swap space: sudo swapon --show and free -h

===> Set ulimit:
# Soft Limit 50000

# Hard Limit 200000
sudo nano /etc/security/limits.conf
add below lines
#* soft nofile 50000
#* hard nofile 200000

* soft nofile 50000
* hard nofile 200000

# End of file
# this is for SMPP system if you set sysctl.conf file as below 
* soft nofile 1048576
* hard nofile 1048576
=> Check limit is applied using "ulimit -a" command, the output command should show the below results for "Open files"

===> Set TCP Params (https://www.linuxbabe.com/ubuntu/enable-google-tcp-bbr-ubuntu)
sudo nano /etc/sysctl.conf

=> Add the following lines at the end of the file.
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
net.ipv4.tcp_max_syn_backlog = 8192
net.core.wmem_max = 16777216
net.ipv4.tcp_window_scaling = 1
net.core.rmem_max = 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 16384 16777216

===> Confirm using
sudo sysctl -p

===> Disable Auto update & auto upgrade
sudo systemctl disable unattended-upgrades
sudo systemctl stop unattended-upgrades

===> Update System Packages:
sudo apt update
sudo apt upgrade -y

===> Install GIT
sudo apt install -y git
Verification: git --version 

===> Install Erlang & Curl
sudo apt install erlang
sudo apt install curl
Verification: curl --version 

===> Build Erlang using kerl
curl -O https://raw.githubusercontent.com/kerl/kerl/master/kerl 
chmod a+x kerl 
sudo mv ./kerl /usr/local/bin 
sudo apt-get install autoconf libncurses-dev build-essential libssl-dev g++ 
kerl list releases 
/usr/local/bin/kerl list releases all 
kerl build 27.3.4.7
kerl install 27.3.4.7 ~/kerl/27.3.4.7

Open .bashrc file (nano ~/.bashrc) and paste this line                                                       [.  /home/{USERNAME}/kerl/27.0.1/activate] at the last line of file 

bash

===> Installing rebar3 
wget https://s3.amazonaws.com/rebar3/rebar3 && chmod +x rebar3
sudo cp ./rebar3 /usr/local/bin
rebar3 --version

or 

===> Installing rebar3
cd /tmp
wget https://github.com/erlang/rebar3/releases/download/3.18.0/rebar3
chmod +x rebar3
sudo mv rebar3 /usr/local/bin/
ls -l /usr/local/bin/rebar3
rebar3 --version

===> Installing  Postgres 17 
sudo apt update && sudo apt upgrade -y 
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list' 
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - 
sudo apt update 
sudo apt install postgresql-17 -y 
psql --version 
sudo systemctl start postgresql 
sudo systemctl enable postgresql 
=> Acess the shell cd 
sudo -i -u postgres psql

===> JDK 21 and Maven: 
sudo apt install -y openjdk-21-jdk
sudo apt install -y maven

===> DOCKER INSTALLATION: 
We will be installing specific version of Docker(27.3.1) 
=> Update the System 
sudo apt update && sudo apt upgrade -y 
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common 
=> Add Docker Official GPG key 
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
=> Add Docker APT repository
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu focal stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 
=> Installing Docker Specific version 
sudo apt update 
=> List Available versions 
apt-cache madison docker-ce
=> Installing Version 27.3.1 
sudo apt install -y docker-ce=5:27.3.1-1~ubuntu.20.04~focal docker-ce-cli=5:27.3.1-1~ubuntu.20.04~focal containerd.io 
=> Verifying Docker Installation 
docker -v 
docker -v 
OUTPUT: Docker version 27.3.1, build ce12230 
=> Create the docker group if it does not exist: 
sudo groupadd docker 
=> Add your user to the docker group: 
sudo usermod -aG docker $USER 
=> Check docker is running or not: 
docker run hello-world

===> python dependencies
sudo apt install python3.12-venv
python3 -m venv venv
source venv/bin/activate

pip install xlrd==1.2.0
pip install pandas
pip install openpyxl
pip install sxl
pip install pyexcel

