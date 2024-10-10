# server 
My Resources about servers

### NGINX - Installation and Usage
```sh
1. Install NGINX and PHP-FPM
kaito@coding~$ sudo apt install nginx php-fpm

2. Configure nginx and create web server with port 80
kaito@coding~$ sudo nano /etc/nginx/sites-available/default

server {
    listen 80;  # Listen on port 80
    root /var/www/html;  # This is root directory
    index index.php index.html index.htm; # this is the default home files
    location / {
        try_files $uri $uri/ =404;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;  # Include the fastcgi configuration
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;  # Adjust PHP version as necessary
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
    location ~ /\.ht {
        deny all;  # Deny access to .htaccess files
    }
}

3. Test nginx configuration
kaito@coding~$ sudo nginx -t

4. Restart nginx
kaito@coding~$ sudo systemctl restart nginx
```

### PM2 - Installation 
what is pm2? 
- PM2 is a popular process manager for Node.js applications that makes it easier to keep applications running, manage them, and perform various operations such as monitoring, clustering, and logging. Here are some of the key features and benefits of using PM2.
Install pm2
```sh
npm install pm2 -g
```

| pm2 command          | Description   |
| ------------- | ------------- |
| `pm2 start "npm run dev" `        | Start application |
| `pm2 list`         | to check list running  |
| `pm2 stop <app-name-or-id>` | to stop |
| `pm2 restart <app-name-or-id>`  | to restart  |
| `pm2 delete <app-name-or-id>` |  to delete |
| `pm2 logs <id>`  | see logs |

