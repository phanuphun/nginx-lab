## Virtural Host
- create config file for new website `nano /etc/nginx/sites-available/mywebsite`
- setting follow this 
```
server {
    listen 80;
    server_name mywebsite.com;
    root /var/www/mywebsite;
    index index.html;
}
``` 
- create new directory for serving new website `mkdir /var/www/mywebsite` and then `nano /var/www/mywebsite/index.html`
- add new context to index.html in mywebsite directory
- `ln -s /etc/nginx/sites-available/mywebsite /etc/nginx/sites-enabled/` to create symlink
- Run `nginx -t`
- config your dns which i use localhost and container i fig in `C:\Windows\System32\drivers\etc\hosts` then add this line `127.0.0.1   mywebsite.com`
- go to `mywebsite.com` on browser