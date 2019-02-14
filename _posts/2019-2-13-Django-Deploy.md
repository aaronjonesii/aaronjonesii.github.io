---
layout: post
title: Deploy Django to Production
image: https://user-images.githubusercontent.com/18272237/52548043-3d1bf780-2d99-11e9-9ec6-cb601b21cb36.png
---
Guide for deploying Django to production using centOS, uWSGI, DJANGO 2.1, Nginx

`the web client <-> the web server <-> the socket <-> uwsgi <-> Django`

## Install Git
```bash
sudo yum install git -y
```

## Create Virtual Environment for Backend API
```bash
# Update pip
python3 -m pip install --upgrade pip
python3 -m pip install virtualenv uwsgi 
virtualenv pyenv
cd pyenv
source bin/activate
```

## Clone Repository & Store Credentials
```bash
git clone <GitHub Repository URL>
cd <GitHub Repository>
git config credential.helper store

# Install project dependencies
pip install -r requirements.txt
```

### `Dont forget to enter environment variables for project:`
 - DJANGO_DEFAULT_DATABASE_NAME - MySQL Database Name
 - DJANGO_DATABASE_PWD - MySQL Database Password
 - SECRET_KEY - Django App's Secret Key
 - DEBUG - Django Debug Mode Boolean
 - ALLOWED_HOST - Allowed Hosts To Run Django App
 - CORS_ORIGIN_WHITELIST - Django Cors Origin Whitelist
 - EMAIL_HOST - Email server address
 - EMAIL_HOST_USER - Email Address for Host Email Account
 - EMAIL_HOST_PASSWORD - Email Password for EMAIL_HOST_USER
 - GOOGLE_RECAPTCHA_SECRET_KEY - Google Account Recaptcha Secret Key

~~Remember to reload the environment after editing the file `source ~/.bashrc`~~
#### `Notice there are no quotation marks!`
```ini
# vars.txt

var1=value1
var2=value2
```

## Create Database for Project
```sql
CREATE DATABASE <database-name>;
```

## Check for  Database migrations
```bash
python manage.py migrate
```

## Test django server
```bash
python manage.py runserver 0:8000
uwsgi --http :8000 --wsgi-file <project-name>/wsgi.py
```

## Install Nginx Web Server
```bash
sudo yum install nginx -y
# Start Web Server
sudo service nginx start
sudo systemctl nginx status
```

## Configure nginx for uwsgi


> Edit nginx config file to comment/delete default configuration.
`include /etc/nginx/conf.d/*.conf;` is an important line.
We are going to place our nginx config file in the `conf.d` directory

```config
# /etc/nginx/nginx.conf

include /etc/nginx/conf.d/*.conf;

#server {
#    listen       80 default_server;
#    listen       [::]:80 default_server;
#    server_name  _;
#    root         /usr/share/nginx/html;
#
#    # Load configuration files for the default server block.
#    include /etc/nginx/default.d/*.conf;
#
#    location / {
#    }
#
#    error_page 404 /404.html;
#        location = /40x.html {
#    }
#
#    error_page 500 502 503 504 /50x.html;
#        location = /50x.html {
#    }
#}

```

> Create nginx conf file in the `project directory`

```config
# project_nginx.conf

server {
    listen      80;
    server_name <DOMAIN-NAME>;
    access_log /var/log/access.log;
    error_log /var/log/error.log;
    charset     utf-8;

    # max upload size
    client_max_body_size 75M;   # adjust to taste

    location /media  {
        alias <PROJECT-PATH>/media;
    }

    location /static {
        alias <PROJECT-PATH>/staticfiles;
    }

    location / {
        uwsgi_pass  unix:/tmp/uwsgi/uwsgi.sock;
        include     /etc/nginx/uwsgi_params;
    }
}
```

#### Link Nginx `project config file` to Nginx conf.d directory
```bash
sudo ln -s <path-to-project>/<config-file> /etc/nginx/conf.d/
# Restart Nginx Service
sudo systemctl stop nginx && sudo systemctl status nginx
sudo systemctl start nginx && sudo systemctl status nginx
```


## Create ini configuration file for uWSGI
###### Configuration file for Emperor Systemd Service
```ini
# /etc/uwsgi/emperor.ini

[uwsgi]
emperor = /etc/uwsgi/vassals
uid = root
gid = nginx
logto = /var/log/emperor.log
```

###### Main uWSGI configuration file for your webapp
```ini
# uwsgi.ini

[uwsgi]
project = <Project-Path>

for-readline = /tmp/uwsgi/vars.txt
  env = %(_)
endfor =

chdir = %(project)
module = <Project-Name>.wsgi
master = true
processes = 4
threads = 2
home = ..
# harakiri = 30
socket = /tmp/uwsgi/uwsgi.sock
vacuum = true
uid = nginx
gid = nginx
daemonize = /tmp/uwsgi/uwsgi.log
die-on-term = true
safe-pidfile = /tmp/uwsgi/uwsgi.pid
```

### Create folder for socket and log
```bash
mkdir /tmp/uwsgi
```


## Test project with unix socket
```bash
uwsgi --socket /tmp/uwsgi/uwsgi.sock --wsgi-file <project-name>/wsgi.py
```

> `Log file locations`:
 - /var/log/emperor.log
 - /tmp/uwsgi/uwsgi.log
 - /var/log/access.log
 - /var/log/error.log

## `Having problems?`

#### Check to see if SELinux is blocking you
`USE CAUTION` because you are disabling a portion of the security on your server
```bash
# Disable SELinux for testing
sudo setenforce 0
sudo getenforce # Shoudl return Permissive if successful
```

*Now check to see if you're able to connect to the unix socket*

#### `If you were able to connect` after disabling SELinux follow the steps below:
We will be creating a custom policy module for SELinux
```bash
sudo setenforce 1 # Re-enable SELinux
sudo grep nginx /var/log/audit/audit.log | audit2allow -M nginx
sudo semodule -i nginx.pp
# Below is OPTIONAL(FOR REFERENCE):
sudo grep nginx /var/log/audit/audit.log | audit2allow -m nginx > nginx.te
sudo cat nginx.te
```

### Add nginx user to your user group
```bash
sudo usermod -a -G $USER nginx
sudo chmod g+rx /home/$USER
```
>`Note that $User will use the currently logged in user that is executing these commands`


##### `After the unix socket is verified working`, lets create our systemd service
```config
# /etc/systemd/system/emperor.service

[Unit]
Description=uWSGI Emperor Service
After=syslog.target

[Service]
ExecStart = /home/<USERNAME-HERE>/.local/bin/uwsgi /etc/uwsgi/emperor.ini
ExecStop = /bin/bash -c 'kill -INT `cat /tmp/uwsgi/uwsgi.pid`'
ExecReload = /bin/bash -c 'kill -TERM `cat /tmp/uwsgi/uwsgi.pid`'
Restart = always
Type = notify
NotifyAccess = all
PIDFile = /tmp/uwsgi/uwsgi.pid

[Install]
WantedBy = multi-user.target
```


## Reload Systemd daemon services

```bash
sudo systemctl daemon-reload
# Check status
sudo systemctl status emperor
```



## Test Server
```bash
sudo systemctl start emperor && sudo systemctl status emperor
# Check log files
sudo cat /var/log/emperor.log
sudo cat /tmp/uwsgi/uwsgi.log
sudo cat /var/log/access.log
sudo cat /var/log/error.log
```







***
[Nginx-Django Reference](http://meetpython.com/depoly-python-project-with-nginx-uwsgi-centos7/)

[uWSGI-Nginx-Django Reference](https://uwsgi-docs.readthedocs.io/en/latest/tutorials/Django_and_nginx.html)

[uWSGI-systemd Reference](https://uwsgi-docs.readthedocs.io/en/latest/Systemd.html?highlight=systemd)

[SELinux Reference](https://axilleas.me/en/blog/2013/selinux-policy-for-nginx-and-gitlab-unix-socket-in-fedora-19/)

[uWSGI-Environment-variables Reference](https://coderwall.com/p/93jakg/multiple-env-vars-with-uwsgi)

[uWSGI-Environment-variables Solution](https://github.com/unbit/uwsgi/issues/629#issuecomment-43303330)

