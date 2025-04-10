## Setting Up Nginx with Docker
- Pull the Nginx Docker image `docker pull nginx`
- Run the Nginx container, exposing port 8088 on the host `docker run -p 8088:80 --name my-nginx -d nginx:latest`
- Access the Nginx container's shell `docker exec -it my-nginx bash`
- By default, Nginx serves files from the `/usr/share/nginx/html/` directory. (If you manually install Nginx using `apt`, the default directory is `/var/www/html/`.)

## Lab 2: Virtual Host Setup
- Create a new configuration file for the website `nano /etc/nginx/sites-available/mywebsite`
- Add the following configuration to set up two virtual hosts:
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
- Create the directory for the new website: `mkdir /var/www/mywebsite`
- Edit the index.html file for the new website: `nano /var/www/mywebsite/index.html`
- Create a symlink for Nginx to load the configuration from sites-available: `ln -s /etc/nginx/sites-available/mywebsite /etc/nginx/sites-enabled/`
- Edit the `nginx.conf` file to include the `sites-enabled` directory for Nginx to load the configuration: `nano /etc/nginx/nginx.conf`
- Add the following to the `http` block:
```
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
    keepalive_timeout  65;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```
- Test the Nginx configuration for syntax errors: `nginx -t`
- Configure your DNS. If you're using localhost and Docker containers, edit the `hosts` file in `C:\Windows\System32\drivers\etc\hosts` (open with administrator rights):
```
# Added by Docker Desktop
191.20.207.53 host.docker.internal
191.20.207.53 gateway.docker.internal
# To allow the same kube context to work on the host and the container:
127.0.0.1 kubernetes.docker.internal
# End of section

127.0.0.1 mywebsite.com
```
- Visit `http://mywebsite.com` in your browser to access the new website.

