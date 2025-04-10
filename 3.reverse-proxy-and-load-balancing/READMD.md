## Lab 3
- Run `docker compose up --build` 
- Run `nano /etc/nginx/nginx.conf` and add 
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