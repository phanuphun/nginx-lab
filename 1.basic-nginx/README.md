
## Lab 1: Serve Static File
- Run `apt install nginx` to install Nginx. Then, use `service nginx status` to check its status. If it's not running, use `service nginx start` to start the service.
- Run `cd /var/www/html` to navigate to the directory where the static files are served.
- Run `nano index.html` to edit the `index.html` file and add some content.
- Run `nginx -t` to check the syntax of the Nginx configuration.
- Run `service nginx reload` to reload the service without shutting it down.
- Visit `http://localhost:8888` to view the index.html file (make sure to expose port 80 to port 8888).
