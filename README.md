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
$ git clone https://github.com/blauerberg/drupal-php.git
$ cd drupal-php/example
$ mkdir -p volumes/mysql
$ mkdir -p volumes/drupal
$ curl https://ftp.drupal.org/files/projects/drupal-X.YY.tar.gz | tar zx --strip=1 -C volumes/drupal
$ docker-compose up -d

# Linux only: fix permissions for your drupal directory with:
$ docker-compose exec php chown -R www-data:www-data /var/www/html

# access your drupal site!
$ open http://127.0.0.1
```

### Example of docker-compose.yml for Quick start
``` 
version: '2'

services:
  datastore:
    image: busybox
    volumes:
      # put your drupal source code and mount as volume.
      - ./volumes/drupal:/var/www/html
      # It is also possible to save mysql files to on host filesystem.
      - ./volumes/mysql:/var/lib/mysql
    container_name: drupal_datastore
  mysql:
    image: mariadb:10.1
    volumes_from:
      - datastore
    volumes:
      # override mysql config if necessary.
      - ./mysql/server.cnf:/etc/mysql/conf.d/server.cnf
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
    volumes_from:
      - datastore
    volumes:
      # allocate large memory_limit for drush.
      - ./php/php.ini:/usr/local/etc/php/php.ini
      # override php-fpm config to use xdebug with port 9000.
      - ./php/www.conf:/usr/local/etc/php-fpm.d/www.conf
      - ./php/zz-docker.conf:/usr/local/etc/php-fpm.d/zz-docker.conf
    links:
      - mysql
    container_name: drupal_php
  nginx:
    image: nginx:1.10
    links:
      - php
    volumes_from:
      - datastore
    volumes:
      # override nginx config to execute drupal through the php container.
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    ports:
      - "80:80"
      - "443:443"
    container_name: drupal_nginx
```

## License

MIT
