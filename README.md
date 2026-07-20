# Redhat_Karnataka

Pre-req:
install sudo:

$ dnf install sudo
EPEL Installation - Manual Installation Using RPM EPEL

$ sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
$ sudo dnf repolist
Install byobu

$ sudo dnf install epel-release -y
$ sudo dnf install byobu -y
$ byobu --version
For any existing or new setup, we must run the following command once to ensure that it does not start in GUI mode if the server reboots.

Set Multi-User:

$ sudo systemctl set-default multi-user.target
$ sudo systemctl set-default multi-user
Set the time zone to Asia Kolkata.

$ sudo timedatectl set-timezone Asia/Kolkata
Step 1: Install GIT
$ sudo dnf install git
(or)
$ sudo yum install git
Step 2: Install Erlang & Curl
$ sudo dnf install erlang
If you encounter an Erlang installation error, use the RabbitMQ repository for the Erlang installation.
$ sudo curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash
$ sudo dnf install erlang
verify installation
$ erl
$ sudo dnf install curl
Step 3: Build Erlang using kerl
i) $ curl -O https://raw.githubusercontent.com/kerl/kerl/master/kerl
ii) $ chmod a+x kerl
iii) $ sudo mv ./kerl /usr/local/bin
iv) $ sudo dnf install autoconf libncurses-dev build-essential libssl-dev g++
If you encounter any errors during the above installation, use the commands below to complete the installation. :
$ sudo dnf groupinstall "Development Tools" -y
$ sudo dnf install -y autoconf ncurses-devel gcc gcc-c++ kernel-devel make openssl-devel
v) $ kerl list releases
vi) $ /usr/local/bin/kerl update releases
v) $ kerl build <build name(take the latest build of 24)> (eg -> kerl build 24.3.4.17)
vi) $ kerl install 24.3.4.17 ~/kerl/24.3.4.17
vii) Open ./bashrc file in home directory as nano .bashrc --> Put path appeared in terminal
[like (. /home/onextel/kerl/24.3.4.17/activate)] at the end of the ./bashrc file
viii) run this command(in all terminals)------->your path as like
(. /home/onextel/kerl/24.3.4.17/activate)
Step 4: Install rebar3
i) $ wget https://s3.amazonaws.com/rebar3/rebar3 && chmod +x rebar3 -> It will add rebar3 file
ii) Then at that location open terminal and run command:
$ sudo cp ./rebar3 /usr/local/bin -> This command will add rebar3 as global variable
iii) $ rebar3 --version -> To check rebar3 version
Note: If this error comes :
escript: exception error: undefined function rebar3:main/1
in function escript:run/2 (escript.erl, line 758)
in call from escript:start/1 (escript.erl, line 277)
in call from init:start_em/1
in call from init:do_boot/3
Then Just copy rebar3 file from local system and paste in your current folder.
After that run step (ii) and step(iii)

Again if any error comes:
i) First check rebar3 is already present and if not working then remove by below command:
$ rm -rf rebar3
ii) Then copy rebar3 file for that and run the below command in a home or onextel folder:
$ git clone https://github.com/erlang/rebar3.git
iii) Go to the rebar3 folder and run the below commands:
$ cd rebar3
$ ./bootstrap
$ ./rebar3 local install
iv) Run the below command which is visible in the terminal:
export PATH=/home/webadmin01/kerl/23.3.4.4/.cache/rebar3/bin:$PATH
sudo cp ./rebar3 /usr/local/bin
rebar3 --version
(here we get like (rebar 3.23.0+build.5376.refb28f3b55 on Erlang/OTP 23 Erts 11.2.2.3)).
Step 5: Git clone all required nodes(Easy way to do is by a Git Token ID)
uihub, hub, onexusermgmt(auth), reaper, smsc, api, yaws, yawsinstall, yawscode
Step 6: Git clone for nitrogen from http://nitrogenproject.org/ then go to Documentations:
i) $ git clone https://github.com/nitrogen/nitrogen

ii) $ make rel_cowboy -> In Nitrogen folder (This will create MYAPP)
Note:- If you got error like:**
error while creating aws server
ubuntu@ip-172-31-17-118:~/work/nitrogen$ make rel_cowboy
make[1]: Entering directory '/home/ubuntu/work/nitrogen'
Building rebar2 for your platform..
fatal: destination path 'rebar' already exists and is not an empty directory.
make[1]: *** [Makefile:38: rebar] Error 128
make[1]: Leaving directory '/home/ubuntu/work/nitrogen'
make: *** [Makefile:98: rel_cowboy] Error 2

  **Solution: Exactly I don't know, but what we do to solve this I'm listing steps:**
  **1. Just do sftp from your current terminal**
  **2. put <zip file path of nitrogen from your local computers> to destination address**
  **3. then remove existing nitrogen folder and unzip the latest one run command as mentioned above initial steps.**
  **4. if again you're getting same error then rm nitrogen and run command as mentioned in above steps but cp rebar from zip nitrogen **
  **and paste on newly created nitrogen.**
Step 7: Edit vm.args (in myapp/etc)
$ cd onextel/myapp/etc
$ nano vm.args
i) make 1st line uihub like:
## Name of the nitrogen node
-name uihub
ii) last line instead of specific id put "redhat"
-setcookie test
Step 8: Edit rebar.config(In myapp)
$ cd onextel/myapp
$ nano rebar.config

Add the below mentioned dependencies in this file:
{simple_bridge, {git, "https://github.com/nitrogen/simple_bridge",{branch, master}}},
{qdate, {git, "https://github.com/choptastic/qdate", {branch, master}}},
{nprocreg, {git, "https://github.com/nitrogen/nprocreg", {tag, "v0.3.0"}}},
{nitrogen_core, {git, "https://github.com/nitrogen/nitrogen_core",{tag, "v2.4.0"}}},
{sync, {git, "https://github.com/rustyio/sync", {tag, "v0.2.0"}}},
{nitro_cache, {git, "https://github.com/nitrogen/nitro_cache", {tag, "0.4.0"}}},

{edate, {git, "https://github.com/dweldon/edate.git", {branch, "master"}}},
{ksuid, {git, "https://github.com/exograd/erl-ksuid", {tag, "master"}}},
{ecron, {git, "https://github.com/zhongwencool/ecron.git", {branch, master}}},
{telemetry, {git, "https://github.com/beam-telemetry/telemetry.git", {branch, "main"}}},
{erlport, {git, "https://github.com/hdima/erlport.git", {branch, master}}},
{syn, {git, "https://github.com/ostinelli/syn.git", {tag, "2.1.1"}}},
{erluap , {git, "https://github.com/silviucpp/erluap.git", "master"}},
{cache, {git, "https://github.com/fogfish/cache.git", {branch, "master"}}},
{replayq, {git, "https://github.com/emqx/replayq.git" , {tag,"v0.3.0"}}}

Then save file (ctrl+s) and to exit (ctrl+x)

Remove folders from (myapp/lib):
$ cd onextel/myapp/lib

remove nprocerg folder from lib ---> rm -rf nprocreg/
remove nitrogen_core folder from lib ---> rm -rf nitrogen_core/
remove nitro_cache folder from lib ---> rm -rf nitro_cache/
remove sync folder from lib --> rm -rf sync
Step 9: Set nitrogen locally(in myapp):
$ cd onextel/myapp
run the below commands:
$ mv site site.old
$ ln -s uihub_path/site ./site -> ln -s /home/onexadmin/onextel/uihub/site ./site
how to take uihub folder path:
cd ../uihub------->then run this command------>pwd
path=/home/onextel/work/uihub/site

Do make(In myapp) run below commands:
$ cd onextel/myapp
$ make
here if we are getting the error of erlport then go to below path:

Step 10: How to Fix Erlport Error:
$ cd onextel/myapp/lib/erlport
$ nano rebar.config
replace -> {erl_opts, [debug_info, warnings_as_errors]}. to---->{erl_opts, []}.
(save and exit the file)

$ cd onextel/myapp/lib/erlport/priv
$ cp -r python3/erlport/ erlportcp
$ cp -r python2/erlport python3/
$ cp -r erlportcp/* python3/erlport
$ rm -rf erlportcp

$ sudo dnf install cmake libyaml-cpp-dev libre2-dev
cmake error :
$ sudo dnf --disablerepo=erlang-solutions install -y cmake
libyaml-cpp-dev error:
$ sudo dnf install libyaml-cpp-dev
again error use below library:
$ git clone https://github.com/jbeder/yaml-cpp.git
$ cd yaml-cpp
$ mkdir build
$ cd build
$ cmake ..
$ make
$ sudo make install
check installation: find /usr /lib -name "yaml-cpp*"

For the libre2-dev error: search for a solution on Google or ChatGPT and follow the steps provided.

here again go to myapp and do make:
$ cd onextel/myapp
$ make
$ ./bin/nitrogen console
Step 11: To run nodes follow this commands:

HUB:
$ rebar3 compile
$ rebar3 shell --setcookie test --sname hub
AUTH:
 $ rebar3 compile
 $ rebar3 shell --setcookie test --sname auth
REAPER:
 $ rebar3 shell --setcookie test --sname reaper
API:
$ rebar3 shell --setcookie test --sname api
Note:- if you get this error: ===> system path/c:21:10: fatal error: 
      expat.h: No such file or directory                                                                                                   
      21 | #include <expat.h>                                                                                                                                                                                 
         |          ^~~~~~~~~                                                                                                                                                                                 
      compilation terminated.  
      Then run this command:-
      $ sudo dnf install libexpat1-dev  

 if you get this error : ===> Hook for compile failed!
 Then run this command: 
   $ sudo dnf install cmake g++ make
   $ rebar3 compile
   and finally rebar3 shell command. - $ rebar3 shell
 if you get error in c_src folder : first remove "c_src" folder from your local system 
                                         then copy it from the server(*sftp server, *get -r path/file_name)
SMSC:
$ rebar3 compile
$ rebar3 shell
If found hook for compiled error: run this command for removing build
$ rm -rf _build/

MON:
$ rebar3 shell --setcookie test --sname mon
Whenever we restart Mon, make sure to run this command:
$ mon2_cache_sa:start_link().
Step 12: if this error occured while running any nodes:
Failed to fetch and copy dep: {git,"git://github.com/ostinelli/pgpool.git",
{ref,
"f733a321033ba85da12d4c27e408e6b7fb7b30cc"}}
Then delete rebar.lock file in respective nodes as--------> rm -r rebar.lock
Step 13: Yaws Setup:
$ git clone https://github.com/erlyaws/yaws.git
$ cd yaws
$ sudo apt-get install libpam0g-dev
error:
No match for argument: libpam0g-dev
Error: Unable to find a match: libpam0g-dev
The error you're encountering is because libpam0g-dev is a package typically found on Debian-based systems (like Ubuntu) rather than on
Red Hat-based systems (like CentOS or Fedora)
For Fedora (and other Red Hat-based systems), the equivalent package is usually called pam-devel. To install it, run:
$ sudo dnf install pam-devel
$ sudo dnf install libtool
$ autoreconf -fi

run on yaws---->./configure --prefix=Just take path wehere your yaws present/yawsinstall;
$ ./configure --prefix=/home/onexadmin/onextel/yawsinstall;
$ make install
$ cd ../yawsinstall/etc/yaws/
nano yaws.conf and change 8000 to 8080
Step 14: Yawscode system_path/yaws/yawsinstall/var/yaws/www/
$ git clone https://github.com/1xtel/yawscode.git
$ cd yawscode/src
$ cp -r myupload.yaws system_path/yawsinstall/var/yaws/www/ or try without -r
like: $ cp -r myupload.yaws /home/onexadmin/onextel/yawsinstall/var/yaws/www/
$ cp -r test2.yaws system_path/yawsinstall/var/yaws/www/
like: $ cp -r test2.yaws /home/onexadmin/onextel/yawsinstall/var/yaws/www/
on yawsinstall----->nano var/yaws/www/myupload.yaws ------->Change path, add existing systems myapp/sratch path(to get
path type pwd command in myapp/scratch-------as system path/myapp/scratch

$ cd ../uihub
$ cd site/src
$ nano ui_utils.erl
-> change localhost to ip as you had
finally run this commnad in yawsinstall----->./bin/yaws -i
Step 15: Install Python and it's libraries
$ sudo dnf install python3 python3-pip -y
verify installtions : $ python3 --version
$ pip3 --version
$ pip3 install pyexcel
$ pip3 install sxl
$ pip3 install xlrd==1.2.0
$ pip3 install pandas
verify installtions : $ pip3 show pyexcel sxl xlrd pandas
Step 15: Enable ports
$ sudo firewall-cmd --permanent --add-port=8000/tcp
$ sudo firewall-cmd --reload
$ sudo firewall-cmd --list-all
If the API Node can't compile/start, use the below workaround.
disabling 2 processes - 1) short URL, 2) for backup q

To change root folder ==>

$ su -

Database Installation and Configuration:
Install PostgreSQL 13
PostgreSQL is not included in the default RHEL 9 repositories, so you need to enable the PostgreSQL official repository.

Step 1.1: Enable the PostgreSQL Repository

sudo dnf install -y https://download.postgresql.org/pub/repos/yum/13/redhat/rhel-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm

Step 1.2: Disable Default PostgreSQL Module

sudo dnf -qy module disable PostgreSQL

Step 1.3: Install PostgreSQL Server and Client

sudo dnf install -y postgresql13 postgresql13-server

Step 1.4: Initialize and Start PostgreSQL

sudo /usr/pgsql-13/bin/postgresql-13-setup initdb

sudo systemctl enable --now postgresql-13

Step 1.5: Verify Installation

psql --version

Install PgBouncer
PgBouncer is a lightweight connection pooler for PostgreSQL.

Step 2.1: Install PgBouncer

sudo dnf install -y pgbouncer

Step 2.2: Configure PgBouncer

Edit the PgBouncer configuration file (/etc/pgbouncer/pgbouncer.ini):

sudo nano /etc/pgbouncer/pgbouncer.ini

For Aura-Config steps follow the link https://github.com/1xtel/Projects/wiki/Complete-db-set-up

