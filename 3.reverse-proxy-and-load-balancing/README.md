## Lab 3: Load Balancing and Reverse Proxy
- Run the following command to build and start the Docker containers: `docker compose up --build`
- Edit the Nginx configuration file to set up load balancing and reverse proxy: `nano /etc/nginx/nginx.conf`
- Add the following configuration to the `nginx.conf` file:
```
upstream backend_servers {
        server load-1:3000;
        server load-2:3000;
}

server {
        listen 80;
        server_name mywebsite.com;

        location / {
                proxy_pass http://backend_servers;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
        }
}
```
 - This configuration defines an upstream group (`backend_servers`) that consists of two backend servers (`load-1` and `load-2`), both listening on port 3000.
 - The `proxy_pass` directive sends requests to one of these backend servers.
 - The `proxy_set_header` directives ensure that relevant headers are passed to the backend servers, including the host, IP addresses, and protocol information.
- Once the changes are made, save and exit the file.
