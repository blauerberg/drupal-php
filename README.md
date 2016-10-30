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

if you want to create full stack for develop your Drupal site, you can use this container with official [nginx](https://hub.docker.com/_/nginx) and [mariadb](https://hub.docker.com/_/mariadb) (or [mysql](https://hub.docker.com/_/mysql)) image.

## Quick start
```
$ cd ~
$ git clone https://github.com/blauerberg/drupal-php.git
$ mkdir -p ~/volumes/mysql
$ mkdir -p ~/volumes/drupal
$ cd ~/volumes/drupal
$ curl https://ftp.drupal.org/files/projects/drupal-X.YY.tar.gz | tar zx --strip=1
$ cd drupal-php/example/small
$ docker-compose up
```

### Example of docker-compose.yml for Quick start
``` 
version: '2'

services:
    mysql:
      image: mariadb:10.1
      volumes:
        # override mysql config if necessary
        - "~/drupal-php/example/small/mysql/server.cnf:/etc/mysql/conf.d/server.cnf"
        # It is also possible to save mysql files to on host filesystem.
        # - "~/volumes/mysql:/var/lib/mysql"
      environment:
        MYSQL_ROOT_PASSWORD: root
        MYSQL_DATABASE: drupal
        MYSQL_USER: drupal
        MYSQL_PASSWORD: drupal
      ports:
        - "3306:3306"
      container_name: drupal_mysql

    php:
      image: blauerberg/drupal-php:7.0-fpm
      volumes:
        # override php-fpm config to use xdebug with port 9001.
        - "~/drupal-php/example/small/php/www.conf:/usr/local/etc/php-fpm.d/www.conf"
        # put your drupal source code and mount as volume
        - "~/volumes/drupal:/var/www/html"
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
        # override nginx config to execute drupal through the php container.
        - "~/drupal-php/example/small/nginx/default.conf:/etc/nginx/conf.d/default.conf"
        # put your drupal source code and mount as volume
        - "~/volumes/drupal:/var/www/html"
      ports:
        - "80:80"
      container_name: drupal_nginx
```

## License

MIT
