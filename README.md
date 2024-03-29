# Linux

## Multipass

`multipass version`
`multipass find`
`multipass launch [alias] --name [name of my choice]`

## os

### change mac address
- network adapter settings => `systemctl restart network`

### basics

- define nameservers
`vi /etc/resolv.conf`

- synch with online time
`rdate -s time.bora.net`
`rdate -p time.bora.net`

- show os information in details
```bash
cat /etc/os-release # show as environment variables
hostnamectl # show in document format
uname -r # show just the os version
```
- check shell
`echo $SHELL`

- list the possible shells
`chsh -l`

- change to other shells
`chsh -s /bin/bash`

#### Disks
- sda => SATA disk 'a'
- linux understands the devices as files
- if additional disk is added => sdb

1. show disk info
`fdisk -l /dev/sdb`

2. partitioning
`fdisk /dev/sdb` => `m` for help => `n` for new` => `p` for primary => `1` for partitioning => `w` for saving

3. format new disk after saving
`mkfs -t ext4 /dev/sdb1`

4. mount created disk to a folder
`mount /dev/sdb1 /mnt/sdb`

5. check disk info
`df -h`

6. Need to auto mount because it won't by automatically
`vi /etc/fstab`
append `/dev/sdb1	/mnt/sdb	ext4	defaults	1 2`

7. check by rebooting
`reboot`
`df -h` => check if /dev/sdb1 still exists after reboot

#### file
- `stat (filename)`
- `chown` 
- `chmod`
  - `chmod u=rwx (filename)`
  - `chmod u=r+w (filename)`
  - `chmod o+r (filename)`
  - `chmod u-w (filename)`

#### change password
- `shutdown -r now` => when it reboots, press `e`
- change `ro` to `rw`
- change `rhgb quiet LANG=ko_KR.UTF-8` to `init=/bin/bash`
- press ctrl+x
- `passwd` => enter the new root password
- at this stage, it still won't work because of linux security related to this
- create a hidden file to go around this security policy
- `touch /.autorelabel` => reboot by `exec /sbin/init`

#### tape archiving
- create `tar -cvf (new file name).tar (source)`
- compress `gzip  (location)`
- do it at once
  - `tar -cvzf (location/new file name).tar.gz (source)`
  - `tar -cvfj (location/new file name).bz2 (source)`
- uncompress
  - `tar -xvfz (location/filename)`
  - `tar -xvfj (location/filename)`

#### mount
- `umount /dev/sr0`
- `mkdir cdrom` => `mount /dev/sr0 /cdrom`

#### package manager
- find package files that have vim `rpm -qa | grep vim`
- delete packages `rpm -e (filename)`
- install packages `rpm -ivh (filename)`
- `yum -y remove (package name)`
- `yum -y install (package name)`

#### systemctl, init, systemd
- `init 6` or `sudo systemctl reboot` => reboot
- `systemctl get-default` => displays the default setting
- `systemctl set-default graphical.target` => changes to graphical interface
- `systemctl set-default multi-user.target` => changes to CLI 

#### users
- `useradd (name)`
- `useradd -D`
- `userdel (name)`
- `passwd (name)`
- `vim /etc/defaults/useradd` => default contents when new users are added
- `userdel -r` => -r erases everything. else, home directory remains
- `chmod u=wrx (filename)` or `chmod u+x (filename)` or `chmod 755 (filename)`
- `chown geepum:geepum (filename)` or `chmod :root (filename)` or `chmod root: (filename)`
- `chown user1.user1 /temp` => change ownership of user/group of /temp to user1
- `chgrp root (filename)`
- `umask` => default value that is subtracted from default permissions when files/dirs are created

#### process
- `ps -ef | grep yum` => shows what kind of processes are running
- `kill -9 (PID)` => kill by process id

#### cat
- `cat > a` + space + enter + text => creates a file 'a' with the text
- `cat < a` => outputs file 'a' to the display
- `cat a b > c` => adds file 'a' and 'b' to file 'c'

#### copy
- `cp -r ./a ./b ./c/` => copy directories and sub-files to 'c' directory
 
#### rmdir
- `rmdir -p ./a/` => deletes only the sub-folders with no files/directories

#### date
- `date` => shows date info
- `date (month) (day) (hour) (minute) (year)` => updates the date

#### head / tail
- `head (filename)` => first 10 lines
- `tail (filename)` => last 10 lines
- `head -5 (filename)` => first 5 lines

#### variables and aliases
```bash
# show list of shell variables
set | less

# show list of environment variables
printenv | less

# setting variables
VARIABLE_NAME="some value"

# unsetting variables
unset VARIABLE_NAME

# adding a path. If to keep permanently, add the line to the startup file
PATH=$PATH:/my-path
export PATH

# add aliases in startup file
alias ALIAS_NAME='my command'

# remove aliases
unalias ALIAS_NAME
```
<br>

## Searching

find man page with a keyword
```bash
man -k [keyword]
```
<br>

search files with globbing
```bash
ls -l [*[0-9]*] # fine a file with a number inside the file name
```
<br>

## file management 

`w` - check what users and doing what

check detailed information about a file
```bash
stat [filepath]
stat [filepath] | grep Access
```

change file owner
```bash
chown [user] [filepath]
chown [user]:[group] [filepath] # this will change the whole group
chown -R [user]:[group] [filepath] # -R will make it applied to all subdirectories
```
<br>

#### firewall
- `yum -y install nautilus`
- `nautilus` => opens the finder window
- `vim /etc/sysconfig/selinux` => `SELINUX=disabled` this was originally 'enforcing' => this only applies after reboot
- `setenforce 0` => changes to persmissive => `init 6` => reboots and becomes disabled
- `setenforce 1` => changes back to enforced
- `vim /etc/hosts` => configures local hosts
- `vim /etc/resolv.conf` => configures domain name server 
- `vim /etc/hostname` => changes hostname
- `service firewalld stop` == `systemctl stop firewalld` => `systemctl disable firewalld` then reboot won't turn on the firewall again => `systemctl status firewalld`

### networking
`vim /etc/sysconfig/network-scripts/ifcfg-ens32`
c```
BOOTPROTO=static #if set as 'none', means NAT using private IP or you can set it as dhcp
ONBOOT=yes
IPADDR=1.1.2.22
NETWORK=1.1.2.0
BROADCAST=1.1.2.255
GATEWAY=1.1.2.2
PREFIX=24
PEERDNS=168.126.63.1
PEERROUTES=no
```
`systemctl restart network`
- `ifconfig ens32 down` or `up` => turn off/on LAN card
- `route`
- `netstat -s >> 20220818_netlog`
- telnet-server
  - `cd /usr/lib/systemd/system` => `vim telnet.socket` => `systemctl start telnet.socket` => `systemctl enable telnet.socket` => `systemctl status telnet.socket` => `getenforce` check if SElinux is enforced/perssive/disabled => `setenforce 0` => `vim /etc/sysconfig/selinux` => `SELINUX=disabled` and reboot => `firewall-config` or `firewall-cmd --permanent --add-service=telnet` => `firewall-cmd --permanent --add-port=23/tcp` => `firewall-cmd --reload`
- `firewall-cmd --list-all`

#### dhcp
- update or install dhcpd => turn off firewall => kill dnsmasq processes `kill -9 (process id)` => disable dnsmasq `systemctl disable dnsmasq` => reboot => config dhcpd.conf file `vim /etc/dhcp/dhcpd.conf` => `systemctl start dhcpd` => `systemctl status dhcpd`
- CentOS server dhcp
  - `rpm -qa + grep dhcp` => check firewall => turn off dhcp firewall => `ps -ef | grep dnsmasq` to kill dns => `systemctl disable dnsmasq` to disable when rebooting => `vim /etc/dhcp/dhcpd.conf`

```bash
subnet 192.168.50.0 netmask 255.255.255.0 {
}

subnet 192.168.10.64 netmask 255.255.255.224 {
        option routers 192.168.10.92;
        option domain-name "kedu.edu";
        option domain-name-servers 192.168.50.101;
        range dynamic-bootp 192.168.10.65 192.168.10.91;
        default-lease-time 10000;
        max-lease-time 50000;
}
``p
  - `systemctl start dhcpd.service`
- dsw1,2) vlan interfaces => `ip help (dhcp server ip add)`
- linux
  - `vim /etc/resolv.conf` => add nameserver ip => `route` to check kernel ip routing table

#### samba
- `yum -y install samba-client samba-common`
- `yum -y install cifs-utils`
- `mount -t cifs //10.5.1.14/share /share -o username=root`

#### firewall
- `firewall-config` => open samba\* and http\*
- `firewall-cmd --permanent --add-service=httpd`
- `firewall-cmd --permanent --add-port=54/tcp`
- `firewall-cmd --permanent --add-port=54/udp`

#### systemctl
- `systemctl enable named`
- `systemctl start named`
- `systemctl status named`
- `systemctl list-units --type=service --all` => list all services

#### apache
- `yum -y install httpd\*` => without \ sign
- backup /etc/hosts /etc/resolve.conf /etc/httpd/conf/httpd.conf
- `cd /var/www` => check folders => `cd html` => `vim ./index.html`
- `systemctl start httpd` => http://localhost in browser
- `vim /etc/httpd/conf/httpd.conf` => conf file

#### dns
- `yum -y install bind-chroot` => `vim /etc/named.conf`
  - change 'listen on port 53 { 127.0.0.1; } and allow-query to `{ any; }`
  ```bash
  zone "kedu.edu" IN {
	type master;
	file "kedu.edu.db";
	allow-update { none; };
};```
- `named-checkconf` => error checking no matter the location
- `vim var/named/kedu.edu.db`
  ```bash
  $TTL 3H
  @	SOA	@ 	root. 		(2 1D 1H 1W 1H)
  	IN	NS	@
	IN	MX	10		10.0.0.25

	IN	A	10.0.0.25
  www	IN	A	10.0.0.25
  ftp	IN	A	10.0.0.25
  mail	IN	A	10.0.0.25
```
- `systemctl enable named.service` => enables when it reboots
- `nslookup` => `server 10.0.0.25` => check if it works

#### ftp
- `rpm -qa | grep vsftpd`
- check `vim /etc/vsftpd/vsftpd.conf` => `check anonymous_enalbe=YES` => `local_enable=YES` => `write_enable=YES` => `local_umask=022` => `anon_upload_enalbe=YES` => `anon_mkdir_write_enalbe=YES` => `ftpd_banner='whatever i want'`
- `systemctl start vsftd` => `systemctl enable vsftpd`
- `find / -name pub` => `chmod o+w pub`
- `yum -y install openssh-server` => `systemctl start sshd` => `systemctl enable sshd` => `systemctl start sshd.socket` => `systemctl daemon-reload` => `systemctl restart sshd.socket` => `systemctl status sshd.socket`

### vnc
- `rpm -qa | grep tigervnc\*` => `yum -y install tiger\*` => `vncserver :1` => winserver vncviewer => connect
- `vncpasswd` => set password + viewer only password => 

# debian linux

## set up
- download virtualbox => connect debian => open browser => http://192.168.56.101:8000/upload => open i86bi-linux-l2-adventerprisek9-15.1a.bin file => select iou => uploade => open IOURC.txt => select iou license => upload 
- if ip is not right, put `netsh interface ip set address name="VirtualBox Host-Only Network" static 192.168.56.1 255.255.255.0`
- open GNS => Edit => Preference => server => host binding to 192.168.56.1 => apply => remote servers => host 192.168.56.101 => port 8001 tcp => ok
- preference => server => iou devices => new => remote => next => ok => name IOU => image http://192.168.56.101:8000/upload => finish => ok

# docker

## set up
- `uname -r` to check kernel info => `yum install -y yum-utils` => `yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo` => yum install -y docker-ce` => `systemctl start docker` => `wget get.docker.com` => `docker info` to check docker operations => `docker -v` => `docker run -i -t ubuntu:14.04` to download and start the image => `exit` to get out of container
- `ctrl+d` to stop and get out
- `ctrl+p` => `ctrl+q` to just get out
- `docker ps -a` to check operations of containers
- `docker attach (name of image)` to get inside the container but it should be started first
- `docker run -d (name of image)` to run image in detached mode
- `docker pull centos:7` to download centos7 => `docker images` to check images
- `docker create -it --name mycentos centos:7` to create container with the image
- `docker container prune` to delete all stopped containers

## webserver on docker
- `docker run -it --name mywebserver -p 80:80 ubuntu:14.04` to map (host IP):(host port):(container port) => to use host's specific IP 192.168.1.100:7777:80 => `docker run -it -p 3306:3306 -p 192.168.0.100:7777:80 ubuntu:18.04`
- `apt-get update` => `apt-get install apache2 -y` => `service apache2 start` => enter linux IP address on host web browser
- `docker run -it --name owncloud -p 80:80 ubuntu:20.04` => `apt update && apt upgrade -y` => `apt install software-properties-common` => `add-apt-repository ppa:ondrej/php` => `apt update` => `apt install libapache2-mod-php7.4 openssl php-imagick php7.4-common php7.4-curl php7.4-gd php7.4-imap php7.4-intl php7.4-json php7.4-ldap php7.4-mbstring php7.4-mysql php7.4-pgsql php-smbclient php-ssh2 php7.4-sqlite3 php7.4-xml php7.4-zip`
- `service apache2 start` => `service apache2 enalbe`
- on our `apt install firewalld` => `firewall-cmd --zone=public --permanent --add-port=80/tcp` + `udp` + `reload`
- `apt install ufw` => `ufw status` =>
- `docker exec -it --privileged (image name) bash` => `ufw status`

## owncloud
- `apt install libapache2-mod-php7.4 openssl php-imagick php7.4-common php7.4-curl php7.4-gd php7.4-imap php7.4-intl php7.4-json php7.4-ldap php7.4-mbstring php7.4-mysql php7.4-pgsql php-smbclient php-ssh2 php7.4-sqlite3 php7.4-xml php7.4-zip` => `apt install mariadb-server -y`
- `apt install mysql-server -y` => `service mysql restart` => `mysql -u root -p` => type `1111` => `CREATE DATABASE owncloud_db;` => GRANT ALL ON owncloud_db.\* TO 'owncloud_user'@'localhost' IDENTIFIED BY 'password'; => FLUSH PRIVILIEGES; => EXIT;
- `wget https://download.owncloud.com/server/stable/owncloud-complete-latest.zip` => `unzip owncloud-complete-latest.zip -d /var/www` after checking that it's downloaded => `apt install unzip\*` if unzip doesn't work => `cd /var/www` => `ls -la` => chown -R www-data:www-data /var/www/owncloud` => `ls -la` to check ownership changed => `chmod -R 755 /var/www/owncloud` => vim /etc/apache2/conf-available/owncloud.conf
```
// add to config file
Alias /owncloud "/var/www/owncloud/“

<Directory /var/www/owncloud/>
  Options +FollowSymlinks
  AllowOverride All

 <IfModule mod_dav.c>
  Dav off
 </IfModule>

 SetEnv HOME /var/www/owncloud
 SetEnv HTTP_HOME /var/www/owncloud

</Directory>

// commands to add
a2enconf owncloud
a2enmod rewrite
a2enmod headers
a2enmod env
a2enmod dir
a2enmod mime
```
- desktop download => add users, test, and operate
- `docker commit owncloud owncloud:20.04` => `docker save -o owncloud.tar owncloud:20.04` => `yum install -y samba-client` => go to windows features and check smb1.0/cifs file sharing support => create a new user root and share the docker folder on window => go to root folder on linux => `mount -t cifs //10.5.1.100/docker /temp -o username=root -o password=1234` if not mounted properly, `umount`
- moving docker from one to another
  - on other linux => `yum install -y yum-utils` => `yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo` => `yum install -y docker-ce` => `systemctl start docker` => `wget get.docker.com` => `yum install -y samba-client` => mount => `df -h` to check mountings => `docker load -i owncloud.tar` => `docker run -it --name owncloud -p 80:80 owncloud:20.04` => `service apache2 start` => `service mysql start` => `vim /var/www/owncloud/config/config.php` to update ip address => check on host computer
- wordpress
  - `docker run -d --name wordpressdb -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=wordpress mysql:5.7` => `docker run -d -e WORDPRESS_DB_PASSWORD=password --name wordpress --link wordpressdb:mysql -p 80 wordpress` => `docker port wordpress` => `docker run -d --name wordpress --link wordpressdb:mysql -p 80 wordpress`
- detach mode
  - `docker container prune` to remove all stopped containers => `docker rm -fv wordpress` + `wordpressdb` => `docker run -d --name detach_test ubuntu:14.04` to create container that is detached and that cannot be started => `docker run -it --name mysql_test -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=wordpress mysql:5.7` to run mysql foreground but not being able to do any operations => `docker create -it --name mycentos centos:7` 
- sharing host volume
  - `docker run -d --name wordpressdb_hostvolume -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=wordpress -v /home/wordpress_db:/var/lib/mysql mysql:5.7` => `docker run -d -e WORDPRERSS_DB_PASSWORD=password --name wordpress_hostvolume --link wordpressdb_hostvolume:mysql -p 80 wordpress` => `docker stop wordpress_hostvolume wordpressdb_hostvolume` => `docker rm wordpress_hostvolume wordpressdb_hostvolume` => `ls /home/wordpress_db`
- docker volume
  - `docker volume create --name myvolume` => `docker volume ls` => `docker volume rm (id)`
  - `docker run -it --name myvolume_1 -v myvolume:/root/ ubuntu:14.04` => `docker run -it --name myvolume_2 -v myvolume:/root/ ubuntu:14.04`
  - in the container, `vi /root/volume` => write text => in linux, `docker inspect --type volume myvolume` to check mountpoint => `cat /var/lib/docker/volumes/myvolume/_data/volume` to check both volume\_1 and volume\_2 are the same => deleting works the same
- docker volume auto
  - `docker run -it --name volume_auto -v /root ubuntu:14.04`
  - `docker container inspect (volume name)` to check container info
  - `docker volume prune` to delete all volumes
- storage
  - `yum install -y nfs-utils` => `docker volume create --name stg` => `vim /etc/hosts` => 192.168.1.131 manager + 192.168.1.132 stg => go to stg => config selinux and firewalld as well as hosts => restart => `vim /etc/exports` => `systemctl start nfs-server` => systemctl enable nfs-server => `ping stg` => `mount -t nfs stg:/stg/ /var/lib/docker/volumes/stg/\_data/` => `docker run -it --name owncloud -p 80:80 -v stg:/var/www.owncloud/data owncloud:20.04
- stg
  - `vim /etc/exports` => `/stg 192.168.31.132`
  - `docker run -it --name owncloud -p 80:80 -v owncloud:/var/www/owncloud/data owncloud:20.04` => `
- bridge network
  - `brctl show docker0` => `docker network ls` => `docker network inspect bridge` => `docker network create --driver bridge mybridge` => `docker run -it --name mynetwork\_container --net mybridge ubuntu:14.04` => `docker container inspect mynetwork\_container` => `docker network inspect mybridge` => `docker network disconnect mybridge mynetwork\_container` => `docker network connect mybridge mynetwork\_container`
  - `docker network create --driver=bridge --subnet=172.72.0.0/16 --ip-range=172.72.0.0/24 --gateway=172.72.0.1 my\_custom\_network` => `docker network connect my\_custom\_network mynetwork\_container` => `docker container inspect mynetwork\_container`
- host network
  - `docker run -it --name network\_host --net host ubuntu:14.04`
- none network
  - `docker run -it --name network\_none --net none ubuntu:14.04`
- container network
  - `docker run -it --name network_container_1 ubuntu:14.04` => `docker run -it --name network_container_2 --net container:network_container_1 ubuntu:14.04` => `docker exec network_container_1 ifconfig`

- copy from current folder to docker container
  - `docker cp index.html myserver:/var/www/html`

## ubuntu
- `sudo vim /etc/netplan/01-network-manager-all.yaml`
- config
```
# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    ens33:
      dhcp4: no
      addresses: [100.1.1.251/24]
      nameservers:
        addresses: [100.1.1.250,168.126.63.1]
      routes:
        - to: default
          via: 100.1.1.254
```
- `ufw status` => `ufw disable` => `ufw allow 80` => `getenforce`


