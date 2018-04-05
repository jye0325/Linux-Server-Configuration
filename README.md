# Linux-Server-Configuration
_____________________________________________________
## Introduction

This Linux Server Configuration is customized for `Udactity's Full-Stack Nanodegree Program`. This configuration run on *Amazon's Lightsail* service running with Ubuntu. The website is loaded with my Item-Catalog projected, cloned from: [Github](https://www.github.com/jye0325/Item-Catalog)

## Usage
1) To use this website visit:
> `18.221.125.195.xip.io`

2) To access the server via SSH, run on your terminal/ command prompt:
> `ssh grader@18.221.125.195 -p 2200 -i <path to private key file>`
*Please note that a password is required to login with the private key.

## Installed Software
- Finger
- Python (2 and 3) 
- Pip, Flask, sqlalchemy, oauth2client, httplib2
- PostgreSQL
- Apache2
- mod_wsgi

## Configurations
1) Uncomplicated Firewall (UFW) Enabled with:

| To | Action | From |
| -- | ------ | ---- |
| 2200/tcp | Allow | Anywhere |
| 80/tcp | Allow | Anywhere |
| 123/tcp | Allow | Anywhere |
| 2200/tcp | Allow | Anywhere (v6) |
| 80/tcp | Allow | Anywhere (v6)  |
| 123/tcp | Allow | Anywhere (v6)  |
| Incoming | Deny | Anywhere |
| Outgoing | Allow | Anywhere |

2) mod_wsgi script modified with:

**ItemCatalog.wsgi**
```
#!/usr/bin/python
activate_this = '/var/www/ItemCatalog/ItemCatalog/venv/bin/activate_this.py'
execfile(activate_this, dict(__file__=activate_this))

import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/ItemCatalog/")
from ItemCatalog import app as application
application.secret_key = 'root'
```

| Absolute Path: |
| -------------- |
| `/var/www/ItemCatalog/ItemCatalog.wsgi` |

**ItemCatalog.conf**
```
<VirtualHost *:80>
# The ServerName directive sets the request scheme, hostname and port that
# the server uses to identify itself. This is used when creating
# redirection URLs. In the context of virtual hosts, the ServerName
# specifies what hostname must appear in the request's Host: header to
# match this virtual host. For the default virtual host (this file) this
# value is not decisive as it is used as a last resort host regardless.
# However, you must set it for any further virtual host explicitly.

ServerName ItemCatalog
ServerAdmin jye0325@live.com

# Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
# error, crit, alert, emerg.
# It is also possible to configure the loglevel for particular
# modules, e.g.
#LogLevel info ssl:warn

ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined

# For most configuration files from conf-available/, which are
# enabled or disabled at a global level, it is possible to
# include a line for only one particular virtual host. For example the
# following line enables the CGI configuration for this host only
# after it has been globally disabled with "a2disconf".
#Include conf-available/serve-cgi-bin.conf
WSGIScriptAlias / /var/www/ItemCatalog/ItemCatalog.wsgi
<Directory /var/www/ItemCatalog/ItemCatalog/>
Order allow,deny
Allow from all
</Directory>
Alias /static /var/www/ItemCatalog/ItemCatalog/static
<Directory /var/www/ItemCatalog/ItemCatalog/static/>
Order allow,deny
Allow from all
</Directory>
</VirtualHost>


# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

| Absolute Path: |
| -------------- |
| `/etc/apache2/sites-available/ItemCatalog.conf` |

3) Root login disabled:

**sshd_config**
```
PermitRootLogin no
```
| Absolute Path: |
| -------------- |
| `/etc/ssh/sshd_config` |

4) SSH Port changed to 2200:

**sshd_config**
```
# What ports, IPs and protocols we listen for
Port 2200
```
| Absolute Path: |
| -------------- |
| `/etc/ssh/sshd_config` |

5) Configured `__init.py__`, `database_setup.py`, `addDB.py` to properly work with the server. Changes made can be found on the Github repository.
~To be updated with link when commit is pushed to Github.

| Absolute Path: |
| -------------- |
| `/var/www/ItemCatalog/ItemCatalog/__init.py__` |
| `/var/www/ItemCatalog/ItemCatalog/database_setup.py` |
|`/var/www/ItemCatalog/ItemCatalog/addDB.py` |

## Third-Party References
1) *Udactiy's Full-Stack Nanodegree Program*
2) *Stackoverflow.com*
3) *[Documentation on mod_wsgi for Flask](http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/)*
4) *[Tutorial on setting up mod_wsgi for Flask](https://www.jakowicz.com/flask-apache-wsgi/)*


