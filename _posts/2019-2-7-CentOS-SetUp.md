---
layout: post
title: centOS Setup guide
image: https://user-images.githubusercontent.com/18272237/52463863-93452c80-2b46-11e9-9f3f-2daddc581277.jpg
---
Guide to setup centOS Sever for multiple uses.

## Update & Upgrade
Make sure your system is up to date
```bash
sudo yum update -y
sudo yum upgrade -y
````

## Download Python
Download desired python gzip file from [Python Download Page](https://www.python.org/downloads/) 
```bash
# Install dependencies
sudo yum install gcc openssl-devel bzip2-devel libffi-devel wget -y
# Download Python Gzip
cd /usr/src
sudo wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tgz
sudo tar xzf Python-3.7.2.tgz
```

## Install Python
Build Python from source and remove leftover zipped file
```bash
cd Python-3.7.2
sudo ./configure --enable-optimizations
sudo make altinstall
# Remove zipped file from earlier
sudo rm -rf /usr/src/Python-3.7.2.tgz
```
`make altinstall` is used to prevent replacing the default python binary file **/usr/bin/python**

## Check Python Version
Make sure it is working
```bash
python3.7 -V
# Set a link to python3
sudo ln -s /usr/local/bin/python3.7 /usr/bin/python3
# Now the command python3 should also work for python3.7
```


## Download MySQL
Add MySQL YUM Repo to system from the [Download Page](https://dev.mysql.com/downloads/repo/yum/) 
```bash
sudo wget https://dev.mysql.com/get/mysql80-community-release-el7-2.noarch.rpm
sudo yum localinstall mysql80-community-release-el7-2.noarch.rpm -y
# Check to make sure it worked
sudo yum repolist enabled | grep "mysql.*-community.*"
```
While using the MySQL YUM Repo, MySQL 8.0 is default `as of 02.10.2018`

## Install MySQL
```bash
sudo yum install mysql-community-server -y
```

### Start MySQL Server
```bash
sudo service mysqld start

# Check status like below
sudo service mysqld status
```

## MySQL Password
At the initial start of the server, it generates a root user and stores the password int he log file.
```bash
# Retrieve temp password for root account
sudo grep 'temporary password' /var/log/mysqld.log
# Login
mysql -uroot -p
```
Change the password of the root account after first login:
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```
> `validate_password` is installed by default.
> The default password policy implemented by validate_password requires that passwords contain
> **at least one upper case letter, one lower case letter, one digit, and one special character,
 > and that the total password length is at least 8 characters.**





***
[Install Python Reference](https://tecadmin.net/install-python-3-7-on-centos/)

[Install MySQL Reference](https://dev.mysql.com/doc/refman/8.0/en/linux-installation-yum-repo.html)

[Python make Error Fix](https://bugs.python.org/issue31652)
