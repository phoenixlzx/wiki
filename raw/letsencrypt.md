title:Let's Encrypt 
category: Security
time: 1476844866446
---

## From Production Environment

### Update NGINX Config

```
server {
	listen 80;
	server_name example.com www.example.com;

	location /.well-known {
		default_type "text/plain";
		root /var/www/html; # or use distro-default directory
	}

	location / {
		# origin configuration
	}
}
```

### Install Certbot

```
wget -c https://dl.eff.org/certbot-auto
./certbot-auto
```

### Setup SSL Certificate

#### Generate DHParams

```
cd /etc/ssl/certs
openssl dhparam -out dhparam.pem 4096 # use 2048 if you are on a low-end-box.
```

#### Request Certificate

```
./certbot certonly --webroot -w /var/www/html -d example.com -d www.example.com
```

#### Setup Cron Job

```
24	15	*	*	2,5	/path/to/certbot/certbot-auto renew --quiet --no-self-upgrade
```

#### Update NGINX Config

```
server {
	listen 80;
	server_name example.com www.example.com;

	location /.well-known {
		default_type "text/plain";
		root /var/www/html;
	}

	location / {
		return 301 https://example.com$request_uri; # or keep origin config if you don't want to force redirect to https
	}
}

server {
	listen 443 ssl http2; # remove http2 if nginx version < 1.10 
	listen [::]:443 ssl http2;
	server_name example.com www.example.com;

	ssl on;
	ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
	ssl_prefer_server_ciphers on;
	ssl_ciphers "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH+aRSA+RC4 EECDH EDH+aRSA !aNULL !eNULL !LOW !3DES !MD5 !EXP !PSK !SRP !DSS !RC4";
	keepalive_timeout    70;
	ssl_session_cache    shared:SSL:10m;
	ssl_session_timeout  10m;

	ssl_dhparam /etc/ssl/certs/dhparam.pem;

	add_header Strict-Transport-Security max-age=63072000;
	add_header X-Frame-Options SAMEORIGIN;
	add_header X-Content-Type-Options nosniff;

    access_log /var/log/nginx/example.access.log;
	error_log /var/log/nginx/example.error.log;

	root /var/www/example;

	location / {
		# origin config
	}
}
```

#### Test NGINX config and reload service

```
nginx -t
service nginx restart
```


