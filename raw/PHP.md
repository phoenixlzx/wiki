title: PHP
category: Server Configuration
time: 1476133839
---

#### PHP-FPM Installation on Ubuntu Server

```
apt-get install software-properties-common
# Ubuntu 12.04 and before:
# apt-get install python-software-properties

add-apt-repository ppa:nginx/stable
add-apt-repository ppa:ondrej/php5-oldstable

apt-get update
apt-get install nginx

apt-get install php5-cgi php5-mysql php5-fpm php5-curl php5-gd php5-idn php-pear php5-imap php5-mcrypt php5-mhash php5-pspell php5-recode php5-sqlite php5-tidy php5-xmlrpc php5-xsl
```

For Ubuntu 16.04 & PHP7

```
apt install nginx mysql-server git unzip curl php-fpm php-mysql php7.0-mbstring php7.0-mcrypt php7.0-zip php7.0-xml php7.0-tidy php7.0-sqlite3 php7.0-gd php7.0-imap php7.0-json php7.0-ldap
```

File `/etc/php/7.0/fpm/php.ini`

```
cgi.fix_pathinfo=0
```


#### PHP-FPM Installation on Debian Server

```
wget http://www.dotdeb.org/dotdeb.gpg -O - | apt-key add -

# For US-based machines
tee /etc/apt/sources.list.d/dotdeb.list << EOF
deb http://mirror.us.leaseweb.net/dotdeb/ wheezy all
deb-src http://mirror.us.leaseweb.net/dotdeb/ wheezy all
EOF

# For China-based machines:
tee /etc/apt/sources.list.d/dotdeb.list << EOF
deb http://dotdeb.90g.org/ stable all
deb-src http://dotdeb.90g.org/ stable all
EOF

apt-get update; apt-get install nginx -y

apt-get install php5-cgi php5-mysql php5-fpm php5-curl php5-gd php5-idn php-pear php5-imap php5-mcrypt php5-mhash php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl -y
```

#### PHP-FPM Nginx Integration
```
tee /etc/nginx/conf.d/php.conf << EOF
upstream php {
    server unix:/var/run/php5-fpm.sock;
}
EOF
```

In any site configurations:

```
    location ~ \.php?$ {
        include /etc/nginx/fastcgi_params;
        fastcgi_index index.php;
        fastcgi_pass php;
    }
```

#### ionCube Installation on Ubuntu 14.04, php 5.5.9 

```
wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
tar xvfz ioncube_loaders_lin_x86-64.tar.gz
cp ioncube/ioncube_loader_lin_5.5.so /usr/lib/php5/
```

Create config file:
```
tee /etc/php5/mods-available/ioncube.ini << EOF
zend_extension = /usr/lib/php5/ioncube_loader_lin_5.5.so
EOF
```

Create soft link to fpm and cli config directory.
Note: ionCube is not compatitable with php5-xcache in PHP 5.5.9.
ioncube should be loaded before any zend extension.
```
ln -s /etc/php5/mods-available/ioncube.ini /etc/php5/{fpm,cli}/conf.d/01-ioncube.ini
```

#### ionCube Installation on Ubuntu 16.04, php 7.0 and later

```
wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
tar xvfz ioncube_loaders_lin_x86-64.tar.gz
cp ioncube/ioncube_loader_lin_7.0.so /usr/lib/php/20151012/
```

Create config file:
```
tee /etc/php/7.0/mods-available/ioncube.ini << EOF
zend_extension = "/usr/lib/php/20151012/ioncube_loader_lin_7.0.so"
EOF
```

Symlinking

```
ln -s /etc/php/7.0/mods-available/ioncube.ini /etc/php/7.0/{fpm,cli}/conf.d/00-ioncube.ini
```

#### Some PHP functions to be disabled:

```
exec, shell_exec, popen, system, passthru, escapeshellarg, escapeshellcmd, symlink
```

#### Enable PHP IMAP/POP module manually

```
php5enmod imap
```

Reference:

1. [PHP - Felix Wiki](http://felixc.at/PHP)
2. [DigitalOcean Tutorials](https://www.digitalocean.com/community/tutorials/how-to-install-ioncube-loader)
3. [AskUbuntu](http://askubuntu.com/questions/484921/php5-imap-on-ubunut-14-04-is-not-enabled)


