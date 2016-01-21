title: PHP / uWSGI
category: Server Configuration
---

Install uWSGI and PHP

```
apt-get install php5-cgi php5-mysql php5-curl php5-gd php5-idn php-pear php5-imap php5-mcrypt php5-mhash php5-pspell php5-recode php5-sqlite php5-tidy php5-xmlrpc php5-xsl
apt-get install uwsgi uwsgi-plugin-php supervisor
```

Create config file `/etc/uwsgi/apps-available/uwsgi-php.ini`

```
[uwsgi]
plugins = php

; leave the master running as root (to allows bind on unix socket or low ports)
master = true
master-as-root = true

; listening on unix socket
socket = /var/run/uwsgi-php.sock
chown-socket = www-data:www-data

; drop privileges
uid = www-data
gid = www-data

; our working dir
project_dir = /var/www

; load additional PHP module ini files
env = PHP_INI_SCAN_DIR=/etc/php5/embed/conf.d

; ; or if you need to specify another PHP.ini
; php-ini = /etc/php5/embed/php.ini
; ; then load additional php module ini files manually
; for-glob = /etc/php5/embed/conf.d/*.ini
;   php-ini-append = %(_)
; endfor =

; allow .php and .inc extensions
php-allowed-ext = .php
php-allowed-ext = .inc

; set php timezone
php-set = date.timezone=UTC

; disable uWSGI request logging
disable-logging = true
; use a max of 10 processes
processes = 10
; ...but start with only 2 and spawn the others on demand
cheaper = 2

; increase buffer size
buffer-size = 32768
```

and do `ln -s /etc/uwsgi/apps-available/uwsgi-php.ini /etc/uwsgi/apps-enabled/uwsgi-php.ini`

In NGINX site config

```
location ~ \.php$ {
	include uwsgi_params;
	uwsgi_modifier1 14;
	uwsgi_pass unix:/var/run/uwsgi-php.sock;
}
```

Create `/etc/supervisor/conf.d/uwsgi-php.conf`

```
[program:uwsgi-php]
command = uwsgi --ini /etc/uwsgi/apps-enabled/uwsgi-php.ini
autorestart = true
user = root
stderr_logfile = /var/log/supervisor/uwsgi-php.log
logfile_maxbytes = 1048576
logfile_backups = 10
```

Note for `user = root`: uWSGI drops privilege in it's own config. User root is needed for creating sockets or listening on low ports.

Restart all services to take effect

```
service supervisor restart
service nginx restart
```

Reference:

1. [Running PHP scripts in uWSGI](http://uwsgi-docs.readthedocs.org/en/latest/PHP.html)
2. [uWSGI Options - php.ini](http://uwsgi-docs.readthedocs.org/en/latest/Options.html#php-ini)
3. [[uWSGI] uwsgi and PHP_INI_SCAN_DIR](http://lists.unbit.it/pipermail/uwsgi/2014-April/007200.html)

