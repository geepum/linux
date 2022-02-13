# Linux

## os

show os information in details
```bash
cat /etc/os-release # show as environment variables
hostnamectl # show in document format
uname -r # show just the os version
```

## Searching

find man page with a keyword
```bash
man -k [keyword]`
```
<br>

search files with globbing
```bash
ls -l [*[0-9]*]` : fine a file with a number inside the file name
```

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

## file manaagement

check what users and doing what
```bash
w
```
<br>

check detailed information about a file
```bash
stat [filename]
stat [filename] | grep Access
```
<br>

change file owner
```bash
chown [user] [filename]
chown [user]:[group] [filename] # this will change the whole group
chown -R [user]:[group] [filename] # -R will make it applied to all subdirectories
```
<br>

## process management


