# Linux

## os

show os information in details
```bash
cat /etc/os-release # show as environment variables
hostnamectl # show in document format
uname -r # show just the os version
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

## User management

add users
```bash
useradd [name]
```
<br>

delete users
```bash
userdel [name]
```
<br>

change password of a user
```bash
passwd [username]
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


