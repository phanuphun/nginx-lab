
## Lab 1 Server Static file
- Run `apt install nginx` to install nginx and then use `service nginx status` to check if not running use `service nginx start`
- Run `cd var/www/html` go to serve directory
- Run `nano index.html` and add some context to file
- Run `nginx -t` to check syntax 
- Run `service nginx reload` to reload service but not shut down service
- go to `http://localhost:8888` to check index.html (expose port 80 to 8888)
