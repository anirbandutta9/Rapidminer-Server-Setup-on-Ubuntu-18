# Rapidminer-Server-Setup-on-Ubuntu

How to setup Rapidminer Server 9.6 on Linux Ubuntu Server 18.04/ 20.04 

Below Guide explains how to installl and setup Rapidminer server 9.6 on a Ubuntu VPS 

## Requirements
Ubuntu Server 18.04/20.04  
RAM - 4 GB  
DISK - 10 GB  
CPU - 2vCPU  

## Installation

### Oracle Java 8

Ssh to server as root and run the following commands ->
```sh
apt-get update
```

Download JAVA ->
https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html
version ->  jdk-8u221-linux-x64.tar.gz


Transfer downloaded java file jdk-8u221-linux-x64.tar.gz to server location  /usr/lib/jvm

Run the following commands on server terminal ->
```sh
cd /usr/lib/jvm
tar –xvf yourjavadownloadfilename.tar.gz
nano /etc/environment
```
Add this line to /etc/environment file –  
JAVA_HOME=" /usr/lib/jvm/jdk1.8.0_221"    

Run this command -

```sh
source  /etc/environment

sudo update-alternatives --install "/usr/bin/java" "java" = "/usr/lib/jvm/jdk1.8.0_221/bin/java"   0
```

Check JAVA HOME is defined properly with these below commands -

```sh
echo $JAVA_HOME
java -version
```

Note :  You can also install and use openjdk java 8 instead of oracle java 8.


### Installing PostgreSQL

Install PostgreSQL using apt
```sh
sudo apt-get update
sudo apt install postgresql postgresql-contrib
```

Login as postgres user and change default password
```sh
sudo su postgres
```

Access postgres CLI
```sh
psql
```

Change password and set it to something you remember
```sh
\password
```

Create rapidminer schema and db user with required permisions
```sh
CREATE DATABASE rapidminer_server;
CREATE USER rapidminer WITH ENCRYPTED PASSWORD 'rapidminer';
GRANT ALL PRIVILEGES ON DATABASE rapidminer_server TO rapidminer;
```

### Install GUI for Ubuntu Server  

Install Xfce on your server  

```sh
apt-get install  xfce4 xfce4-goodies xorg dbus-x11 x11-xserver-utils  
```

Install Xrdp  

```sh
apt-get install  xrdp   
``` 

Add the xrdp user to the SSL-CERT group and restart Xrdp:  

```sh
adduser xrdp ssl-cert  
systemctl restart xrdp
```

Allow port 3389 on Firewall to accept traffic (optional)  

```sh
ufw allow 3389
```

Use any RDP client (like Windows RDP client) to connect to the ubuntu server GUI and login with root user and password.  

If you have a Windows PC, you can use the default RDP client. Type “remote” in the Windows search bar and click on “Remote Desktop Connection”. This will open up the RDP client. In the “Computer” field, enter the remote server IP address and click “Connect”.  

```sh
username -> 'root'
password -> 'yourserverpassword'
```

### Install RapidMiner Server

Download Rapidminer Server version 9.6 from official website ->  rapidminer.com

Transfer downloaded file to server using FTP/SFTP software FileZilla Or Winscp and extract the zip on server 

```sh
unzip rapidminerdownload.zip
```

Open terminal inside GUI (running this from within GUI is important else it will not work) and run the following commands -

```sh
cd rapidminer-server-installer-9.6.0/bin
bash rapidminer-server-installer   
```

Note:  Above commands must be run inside server GUI 

Now follow the on screen installation  process and enter license key of RapidMiner Server (Can be found on official website) when asked


Configure Server Settings as follows:  
Hostname: localhost  
Port for web interface: 8888  
Server Memory (in MB): 2048  
Number of bundled Job Containers: 1  
Internal port: 5672  
Memory per Job Container (in MB): 2048  


Configure Database as follows  
Hostname: localhost  
Port: 5432  
Database schema: rapidminer_server  
Database username: rapidminer  
Database password: rapidminer  


Click try connection to check that it's correctly working

Then click next and follow until the end of the installation.


### Executing RapidMiner Server

Navigate to rapidminer install directory and run `standalone.sh` script

```sh
cd /rapidminer-server/rapidminer-server-installer-9.3.2/bin
./standalone.sh
```

Wait until all the commands finish and then open a navigator and go to `yourdomain:8080` OR `yourserverpublicIP:8080`

Log in with default rapidminer credentials:  
- Username: admin
- Password: changeit
  

### Run RapidMiner Server as a system service and autostart on server boot

Follow the 2nd part of this guide for ubuntu 18.04 and 20.04 -> 
https://docs.rapidminer.com/9.5/server/configure/settings/run-as-a-service.html



### If this project helped you reduce time to develop, feel free to give me a cup of coffee :)
https://www.anirbandutta.in/pay/

### Having trouble setting the server up ?  

Hire me now to do the setup for you ->   https://www.anirbandutta.in/contact
