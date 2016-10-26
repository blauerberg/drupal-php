# Simple php-fpm container for drupal

This container provide below features:
  - php-fpm 5.6 or 7.0 (depending on tag) with extensions to support generic drupal development.
    - mbstring
    - mysqlndg
    - curl
    - openssl
    - mcrypt
    - gd
    - intl
    - zip
    - pdo
    - opcache
    - xdebuge
    - apcu
  - composer
  - drush 8.x

So if you want to create full stack for develop your Drupal site, you can use this container with official [nginx](https://hub.docker.com/_/nginx) and [mariadb](https://hub.docker.com/_/mariadb) (or [mysql](https://hub.docker.com/_/mysql)) image.


## Quick start
```
$ cd ~
$ git clone https://github.com/blauerberg/drupal-php.git
$ mkdir -p ~/your/mysql/volume/path
$ mkdir -p ~/your/drupal/code/path
$ (put your durpal source code into ~/your/drupal/code/path)
$ cd drupal-php/example/small
$ docker-compose up
```

### Example of docker-compose.yml for Quick start
``` 
version: '2'

services:
    mysql:
      image: mysql:5.5
      volumes:
        - "~/drupal-php/example/small/mysql/server.cnf:/etc/mysql/conf.d/server.cnf" # override mysql config if necessary
        - "~/your/mysql/volume/path:/var/lib/mysql" # put your drupal source code and mount as volume
      environment:
        MYSQL_ROOT_PASSWORD: root
        MYSQL_DATABASE: drupal
        MYSQL_USER: drupal
        MYSQL_PASSWORD: drupal
      ports:
        - "3306:3306"
      container_name: drupal_mysql

    php:
      image: blauerberg/drupal-php:5.6-fpm
      volumes:
        - "~/drupal-php/example/small/php/www.conf:/usr/local/etc/php-fpm.d/www.conf" # override php-fpm config to use xdebug with port 9001.
        - "~/your/drupal/code/path:/var/www/html" # put your drupal source code and mount as volume
      links:
        - mysql
      ports:
        - "9000:9000"
        - "9001:9001"
      container_name: drupal_php

    nginx:
      image: nginx:1.10
      links:
        - php
      volumes:
        - "~/drupal-php/example/small/nginx/default.conf:/etc/nginx/conf.d/default.conf" # override nginx config to execute drupal through the php container.
        - "~/your/drupal/code/path:/var/www/html" # put your drupal source code and mount as volume.
      ports:
        - "80:80"
      container_name: drupal_nginx
```

## License

MIT
