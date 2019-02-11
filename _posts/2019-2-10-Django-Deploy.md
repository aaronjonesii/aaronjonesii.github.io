---
layout: post
title: Deploy Django to Production
image: https://user-images.githubusercontent.com/18272237/52548043-3d1bf780-2d99-11e9-9ec6-cb601b21cb36.png
---
Guide for deploying django to production using centOS, uWSGI, DJANGO 2.1, NGINX

`the web client <-> the web server <-> the socket <-> uwsgi <-> Django`

## Install Git
```bash
sudo yum install git -y
```

## Create Virtual Environment for Backend API
```bash
python3 -m pip install virtualenv uwsgi-django
cd uwsgi-django
source bin/activate
```

## Clone Repository & Store Credentials
```bash
git clone <GitHub Repository URL>
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

Remember to reload the environment after editing the file `source ~/.bashrc`

## Create Database for Project
```sql
CREATE DATABASE anonsys_db;
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
Copy `elc/nginx/uwsgi_params` to your project directory `<path-to-project>`

Create nginx conf file in project directory
```editorconfig
# project_nginx.conf

# the upstream component nginx needs to connect to
upstream django {
    # server unix:///tmp/uwsgi.sock;
    # server unix:<path-to-project>/<project-name>.sock; # for a file socket
    server 127.0.0.1:8001; # for a web port socket (we'll use this first)
}
# configuration of the server
server {
    # the port your site will be served on
    listen      8000;
    # access_log <path-to-project>/access.log;
    # error_log <path-to-project>/error.log;
    # the domain name it will serve for
    server_name <website-name>; # substitute your machine's IP address or FQDN
    charset     utf-8;
    # max upload size
    client_max_body_size 75M;   # adjust to taste
    # Django media
    location /media  {
        alias <path-to-project>/media;  # your Django project's media files - amen
d as required
    }
    location /static {
        alias <path-to-project>/static; # your Django project's static files - ame
nd as required
    }
    # Finally, send all non-media requests to the Django server.
    location / {
        uwsgi_pass  django;
        include     <path-to-project>/uwsgi_params; # the uwsgi_params file you installed
    }
}
```

#### Link Nginx config file to Nginx
```bash
sudo ln -s <path-to-project>/<config-file> /etc/nginx/sites-available/
sudo ln -s <path-to-project>/<config-file> /etc/nginx/sites-enabled/
# Restart Nginx Service
sudo systemctl restart nginx
sudo systemctl status nginx
```

## Test project on port 8000
```bash
uwsgi --socket :8001 --wsgi-file <project-name>/wsgi.py
```



## Configure ini configuration file for uWSGI
```config
[uwsgi]
http = :8000
home = <path-to-virtualenv>
chdir = <path-to-project>
wsgi-file = <projectname>/wsgi.py
```




***
[Reference](https://uwsgi-docs.readthedocs.io/en/latest/tutorials/Django_and_nginx.html?highlight=nginx)
