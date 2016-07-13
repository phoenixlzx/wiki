title: NGINX
category: Server Configuration
---
#### HTTP Proxy
```
resolver 8.8.4.4;
server {
    listen 3128;
    location / {
        proxy_pass http://$http_host$request_uri;
    }
}
```

#### Reverse Proxy
```
server {
    listen 80;
    server_name new-site.com;

    location / {
        proxy_pass http://origin-site.com/;
        proxy_redirect default;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forward-IP $remote_addr;
        port_in_redirect    on;
        server_name_in_redirect off;
        proxy_connect_timeout 300;
    }
}
```

#### Cached Reverse Proxy

Make directories for cache:
```
mkdir -p /var/cache/nginx/cache
mkdir -p /var/cache/nginx/temp
```

Paste below `log_format` directive in `nginx.conf`:
```
client_body_buffer_size  512k;
proxy_connect_timeout    5;
proxy_read_timeout       60;
proxy_send_timeout       5;
proxy_buffer_size        16k;
proxy_buffers            4 64k;
proxy_busy_buffers_size 128k;
proxy_temp_file_write_size 128k;
proxy_temp_path   /var/cache/nginx/temp;
proxy_cache_path  /var/cache/nginx/cache levels=1:2 keys_zone=cache_one:500m inactive=7d max_size=30g;
```

Add to vhost configuration:
```
proxy_cache cache_one;
proxy_cache_valid  200 304 3d;
proxy_cache_key $host$uri$is_args$args;
expires 10d;
```

#### Reverse pass to a subdirectory (Map domain root to sub-path)

```
server {
    listen 80;
	server_name www.example.com;
	location / {
		proxy_pass http://sub.example.com/path;
		sub_filter /path/ /; # You may need this, otherwise comment it out.
		proxy_redirect off;
	}
}
```

#### Set Expires Headers For Static Content
```
location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
    expires 1y;
    log_not_found off;
}
```

#### Increase HTTP Post Size Limit
```
http {
    #...
    client_max_body_size 100m;
    #...
}
```

#### Set Correct Files/Folders Permissions
```
chown -R www-data:www-data .
find . -type f -exec chmod 644 {} \;
find . -type d -exec chmod 755 {} \;
```

#### Setup Basic-Auth on NGINX

Install `apache2-utils`
```
apt-get install apache2-utils
```

Create User and Password
```
htpasswd -c /etc/nginx/.htpasswd exampleuser
```

Add to NGINX site config
```
server {
	...
	location / {
		...
		auth_basic "Restricted";
		auth_basic_user_file /etc/nginx/.htpasswd;  #For Basic Auth
	}
}
```

#### NGINX with SSL

```
listen 443 ssl;
ssl_certificate /etc/nginx/server.crt;
ssl_certificate_key /etc/nginx/server.key;
ssl_protocols TLSv1.2 TLSv1.1 TLSv1;
ssl_prefer_server_ciphers on;
ssl_ciphers "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH+aRSA+RC4 EECDH EDH+aRSA !aNULL !eNULL !LOW !3DES !MD5 !EXP !PSK !SRP !DSS !RC4";
keepalive_timeout    70;
ssl_session_cache    shared:SSL:10m;
ssl_session_timeout  10m;
```

Concatenate certificates via `cat`

```
cat domain.crt intermediate.pem rootCA.pem > domain.signed.crt
```

Avoid using SHA-1 as key exchange method, use Diffie Hellman Ephemeral parameters.

```
cd /etc/ssl/certs
openssl dhparam -out dhparam.pem 4096 # use 2048 if you are on a low-end-box.
```

Then tell nginx to use it for DHE key-exchange:

```
ssl_dhparam /etc/ssl/certs/dhparam.pem;
```

##### Redirect to HTTPS

```
location / {
    return 301 https://example.com$request_uri;
}
```

##### HSTS

```
    add_header Strict-Transport-Security max-age=63072000;
    add_header X-Frame-Options DENY;
    add_header X-Content-Type-Options nosniff;
```

#### CORS Config

Add the following to `location` block with proper options.
```
    add_header 'Access-Control-Allow-Origin' "*";
    add_header 'Access-Control-Allow-Credentials' 'true';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS';
    add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Mx-ReqToken,X-Requested-With';
```

If you get errors like `... from origin 'https://...' has been blocked from loading by Cross-Origin Resource Sharing policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.`, try to change value of `Access-Control-Allow-Origin` to specific domains.

```
    add_header 'Access-Control-Allow-Origin' "example.com static.example.com";
```

#### Enable Certificate Transparency for DV certificates

[nginx-ct](https://www.certificate-transparency.org/resources-for-site-owners/nginx)

Reference:

1. [Nginx - FelixWiki](http://felixc.at/Nginx)
2. [DigitalOcean Tutorials](https://www.digitalocean.com/community/tutorials/how-to-set-up-http-authentication-with-nginx-on-ubuntu-12-10)
3. [enable-cors](http://enable-cors.org/server_nginx.html)
4. [Gist - michiel](https://gist.github.com/michiel/1064640)
5. [Strong SSL Security on nginx](https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html)
