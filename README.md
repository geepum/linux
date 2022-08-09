# Linux

## Multipass

`multipass version`
`multipass find`
`multipass launch [alias] --name [name of my choice]`

## os

### basics

vi /etc/resolv.conf
- define nameservers

rdate -s time.bora.net
rdate -p time.bora.net
- synch with online time

show os information in details
```bash
cat /etc/os-release # show as environment variables
hostnamectl # show in document format
uname -r # show just the os version
```
<br>

#### cat
- `cat > a` + space + enter + text => creates a file 'a' with the text
- `cat < a` => outputs file 'a' to the display
- `cat a b > c` => adds file 'a' and 'b' to file 'c'

#### copy
- `cp -r ./a ./b ./c/` => copy directories and sub-files to 'c' directory
 
#### rmdir
- `rmdir -p ./a/*` => deletes only the sub-folders with no files/directories

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


