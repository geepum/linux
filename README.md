# Linux

## Multipass

`multipass version`
`multipass find`
`multipass launch [alias] --name [name of my choice]`

## os

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

#### networking
`vim /etc/sysconfig/network-scripts/ifcfg-ens32`
```
BOOTPROTO=static #if set as 'none', means NAT using private IP or you can set it as dhcp
ONBOOT=yes
IPADDR=1.1.2.22
NETWORK=1.1.2.0
BROADCAST=1.1.2.255
GATEWAY=1.1.2.2
```
`systemctl restart network`
- `ifconfig ens32 down` or `up` => turn off/on LAN card
- `route`
- `netstat -s >> 20220818_netlog`
- telnet-server
  - `cd /usr/lib/systemd/system` => `vim telnet.socket` => `systemctl start telnet.socket` => `systemctl enable telnet.socket` => `systemctl status telnet.socket` => `getenforce` check if SElinux is enforced/perssive/disabled => `setenforce 0` => `vim /etc/sysconfig/selinux` => `SELINUX=disabled` and reboot => `firewall-config` or `firewall-cmd --permanent --add-service=telnet` => `firewall-cmd --permanent --add-port=23/tcp` => `firewall-cmd --reload`

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

#### general
- `yum -y install nautilus`
- `nautilus` => opens the finder window
- `vim /etc/sysconfig/selinux` => `SELINUX=disabled` this was originally 'enforcing' => this only applies after reboot
- `setenforce 0` => changes to persmissive => `init 6` => reboots and becomes disabled
- `vim /etc/hosts` => configures local hosts
- `vim /etc/resolv.conf` => configures domain name server 
- `vim /etc/hostname` => changes hostname

#### users
- `useradd (name)`
- `useradd -D`
- `userdel (name)`
- `passwd (name)`
- `vim /etc/defaults/useradd` => default contents when new users are added
- `userdel -r` => -r erases everything. else, home directory remains
- `chmod u=wrx (filename)` or `chmod u+x (filename)` or `chmod 755 (filename)`
- `chown geepum:geepum (filename)` or `chmod :root (filename)` or `chmod root: (filename)`
- `chgrp root (filename)`
- `umask` => default value that is subtracted from default permissions when files/dirs are created

#### samba
- `yum -y install samba-client samba-common`
- `yum -y install cifs-utils`
- `mount -t cifs //10.5.1.14/share /share -o username=root`
- `firewall-config` => open samba* and http*
- `yum -y install httpd\*` => without \ sign
- backup /etc/hosts /etc/resolve.conf /etc/httpd/conf/httpd.conf
- `cd /var/www` => check folders => `cd html` => `vim ./index.html`
- `systemctl start httpd` => http://localhost in browser
- `vim /etc/httpd/conf/httpd.conf` => conf file
- `chown user1.user1 /temp` => change ownership of user/group of /temp to user1

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


#### loop
```bash
for i in geepum terry june; do
  echo "My name is $i";
done
# My name is geepum
# My name is terry
# My name is june

for i in {1..5..2}; do
  echo $i
done
# 1
# 3
# 5

seq 4 # 1 2 3 4 in each line
seq 2 5 # 2 3 4 5 in each line
seq 1 2 9 # 1 3 5 7 9 in each line
seq -s, 1 4 # 1, 2, 3, 4
seq -f "%02g" 3 # 01 02 03 in each line. 0 - no indentation / 2 - digits
seq -f "%04g" 3 # 0001 0002 0003 in each line
expr `seq -s " + " 1 100` # 5050. shows the result of the intended operation
touch $(seq -f "file%g" 1 10) # creates from file1 to file10

for i in $(seq 1 2 9); do
  echo "Number: $i";
done
# Number: 1
# Number: 3
# ...
# Number: 9

i=1
while [ $i -le 3 ]; do
  echo "i is $i";
  i=$(($i+1));
done
# i is 1
# i is 2
# i is 3

for i in $(seq 1 5); do
  if [ $i -eq 4 ]; then
    break
  fi
  echo "Number: $i";
done
# shows Number: 1 to Number: 3 in each line

for i in $(seq 1 10); do
  if [ $i -gt 5 ] && [ $i -lt 10 ]; then
    continue
  fi;
    echo "Number: $i";
done
# shows Number: 1 to Number 5 then skips to Number: 10 in each line
```
<br>

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

check what users and doing what
```bash
w
```
<br>

check detailed information about a file
```bash
stat [filepath]
stat [filepath] | grep Access
```
<br>

change file owner
```bash
chown [user] [filepath]
chown [user]:[group] [filepath] # this will change the whole group
chown -R [user]:[group] [filepath] # -R will make it applied to all subdirectories
```
<br>

change file permissions
```bash
chmod 400 [filepath] # only user gets read permission
chmod a=rwx [filepath] # all gets read, write, and execute permissions
chmod u=r+x [filepath] # user gets read and write permissions
chmod o+x pfilepath] # other gets execute permission
chmod g=rwx [filepath] # group gets read, write, and execute permissions
```
<br>

## process management


