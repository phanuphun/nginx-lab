## Setting
- `docker pull nginx`
- `docker run -p 8088:80 --name my-nginx -d nginx:latest`
- `docker exec -it my-nginx bash`

- serving html directory `/usr/share/nginx/html/` by default (if u use apt install manual directory is `/var/www/html/`)

## Lab 2 Virtural Host
- create config file for new website `nano /etc/nginx/sites-available/mywebsite`
- setting follow this 

```
server {
        listen 80;
        server_name mywebsite.com;
        root /var/www/mywebsite;
        index index.html;
}

server {
        listen 80;
        server_name mywebsite2.com;
        root /var/www/mywebsite2;
        index index.html;
}
``` 
- create new directory for serving new website `mkdir /var/www/mywebsite` and then `nano /var/www/mywebsite/index.html` to add new context to index.html in mywebsite directory
- `ln -s /etc/nginx/sites-available/mywebsite /etc/nginx/sites-enabled/` to create symlink for nginx loading to `/sites-enabled`
- Run `nano /etc/nginx/nginx.conf` to inclued `sites-enabled` for nginx loading
```conf
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```
- Run `nginx -t` to check syntax

- config your dns which i use localhost and container i fig in `C:\Windows\System32\drivers\etc\hosts` (open with administrator)
```
# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host

# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost
# Added by Docker Desktop
191.20.207.53 host.docker.internal
191.20.207.53 gateway.docker.internal
# To allow the same kube context to work on the host and the container:
127.0.0.1 kubernetes.docker.internal
# End of section

	127.0.0.1	mywebsite.com
```
- go to `mywebsite.com` on browser