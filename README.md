# server 
This is all about Servers and Programming

### NGINX - Installation and Usage

1. Install NGINX and PHP-FPM
```sh
kaito@coding~$ sudo apt install nginx php-fpm
```
3. Configure nginx and create web server with port 80
```sh
kaito@coding~$ sudo nano /etc/nginx/sites-available/default
```
Edit and save
```sh
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
```
3. Test nginx configuration
```sh
kaito@coding~$ sudo nginx -t
```
5. Restart nginx
```bash
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

# Docker Composer 
Docker Compose is a tool for defining and running multi-container Docker applications. It allows you to configure your application's services in a YAML file called docker-compose.yml, making it easier to manage and deploy containerized applications.

| Commands          | Description   |
| ------------- | ------------- |
| docker-compose up        | Start services |
| docker-compose up -d     | start services in the background  |
| docker-compose stop      | Stop services  |
| docker-compose down      | Stop and remove containers, networks, images, and volumes |
| docker-compose logs      | View service logs |
| docker-compose scale <service>=<number>      | Scale service |
| docker-compose restart      | Restart service |
| docker-compose build      | Build services |
| docker-compose ps      | List containers |
| docker-compose run <service> <command>      | Run a one-off command on a service |
| docker-compose help      | Display help |

Example build sample project and deploy in docker

Project folder tree


    .
    ├── ...
    ├── .docker                    # hidden dir for docker
    │   ├── nginx                  # nginx configuration
    │   ├── default.conf           # default configuration for nginx
    │   ├── Dockerfile             # here the Dockerfile
    │   └── ...                    # etc.
    ├── src                        # Directory 
    │   ├── index.php              # Your home page
    ├── docker-compose.yml         # here the docker compose file
    └── ...

> docker-compose.yml
```yml
version: '3'
services:
    nginx:
        build:
            context: .
            dockerfile: .docker/nginx/Dockerfile
        volumes:
            - ./src/:/var/www/html
        image: nginx:alpine
        ports:
            - '8080:80  # 8080 is the port you want to use | 80 port of the server
        networks:
            - internal
    php:
        image: php:fpm-alpine
        volumes:
            - ./src/:/var/www/html
        networks:
            - internal
networks:
    internal:
        driver: bridge
```
> default.conf
```conf
server {
    listen 0.0.0.0:80';
    root /var/www/html;
    location / {
        index index.php index.html;
    }
    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass php:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
    }
}
```

> Dockerfile
```Dockerfile
FROM nginx:alpine
ADD .docker/nginx/default.conf /etc/nginx/conf.d
```
BUILD Docker
```sh
docker compose build && docker compose up -d
```

