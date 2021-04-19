---
title: Bash Common Commands
author: Richard Hernandez
date: 2021-04-19 12:00:00 +0800 
categories: [Cheatsheets]
tags: [bash, cheatsheet, files bash, ssh, enviroment variable, permissions, folders]
---

## Files

##### list files
```shell
ls
```
##### list all files
```shell
ls -la
```
##### create file
```shell
touch <file_name>
```
##### rename file
```shell
mv <file_name> <new_file_name>
```
##### copy file
```shell
cp <file_name> <file_name>
```
##### delete file
```shell
rm -f <file_name>
```
##### print file contents
```shell
cat <file_name>
```
##### scroll long file
```shell
less <file_name>
```
##### print first N lines
```shell
head -<n_lines> <file_name>
```
##### print last N lines
```shell
tail -<n_lines> <file_name>
```
##### search for filename's location
```shell
locate <file_name>
```
##### search for program's location
```shell
which <program_name>
```
##### search for a file, containing a string
```shell
find . -type f -print0 | xargs -0 grep "<search_string>"
```
<br/><br/><br/>





## Folders
##### show current folder
```shell
pwd
```
##### list sub-folders
```shell
ls -d */
```
##### change directory
```shell
cd <directory_name>
```
##### change to home folder
```shell
cd ~
```
##### create folder
```shell
mkdir <folder_name>
```
##### rename folder
```shell
mv <folder_name> <new_folder_name>
```
##### delete folder
```shell
rmdir <folder_name>
```
<br/><br/><br/>





## File / Folder Permissions
##### list permissions
```shell
ls -la
```
##### set executable #
```shell
chmod +x <file_name>
```
##### set ownership
```shell
chown -R <user_name> <folder_name>
```
<br/><br/><br/>





## Environmental Variables
##### print existing variables
```shell
printenv
```
##### set variable
```shell
export VAR_NAME="hello"
```
##### print variable
```shell
echo $VAR_NAME
```
##### remove variable
```shell
unset VAR_NAME
```
<br/><br/><br/>





## Scripting
##### set script to be executable
```shell
chmod +x ./script_name.sh
```
##### set the executable flag on all .sh files
```shell
find ./ -name "*.sh" -exec chmod +x {} \;
```
##### run script
```shell
./script_name.sh
```
##### run script, but keep environmental variables
```shell
. ./script_name.sh
source ./script_name.sh
```
##### associative arrays
```shell
@see http://www.artificialworlds.net/blog/2012/10/17/bash-associative-array-examples/
```
<br/><br/><br/>


## Networking
##### list network adapters (Debian, Ubuntu, Kali)
```shell
ifconfig -a
```
##### list network adapters (Redhat, Fedora, CentOS)
```shell
ip link show
```
##### flush dns cache (Debian, Ubuntu, Kali Linux)
```shell
sudo /etc/init.d/dns-clean
```
##### restart networking (Debian, Ubuntu, Kali Linux)
```shell
sudo service network-manager restart
```
##### list all open ports
```shell
sudo netstat -tunlp

nmap <127.0.0.1 | localhost | ip_addr>
```
##### find if specific port is open
```shell
sudo netstat -tunlp | grep <port_number>
```
##### list all available hosts, within an ip range
```shell
nmap -sP 192.168.1.1/24

nmap -sP <ip_addr>/<ip_mask>
```
##### list all available hosts, within an ip range, on specific ports
```shell
# custom TCP SYN scan
sudo nmap -sP -PS22,3389 192.168.1.1/24
sudo nmap -sP -PS<port_1>,<port_2> <ip_addr>/<ip_mask>

#custom UDP scan
sudo nmap -sP -PU161 192.168.1.1/24
sudo nmap -sP -PU<port_1> <ip_addr>/<ip_mask>
```
<br/><br/><br/>





## SSH commands
##### connect
```shell
ssh <user_name>@<host_name>
```
##### connect, on specific port
```shell
ssh -p <port_number> <user_name>@<host_name>
```
##### connect, using keyfile
```shell
ssh -i <identity_filename> <user_name>@<host_name>
```
##### remove ssh key
```shell
ssh-keygen -R <server_dns> > /dev/null 2>&1;
```

##### copy remote file to local
```shell
scp <user_name>@<server_dns>:<remote_location> <local_location>
```