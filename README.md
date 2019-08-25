# Simple php-fpm container for drupal

This container provide below features:
  - php-fpm 5.6/7.0/7.1/7.2/7.3 (depending on tag) with extensions to support generic drupal development.
    - mbstring
    - mysqlnd
    - mysqli
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
    - xml
    - zip
    - bz2
    - memcache
  - composer
  - drush 8.x

if you want to create full stack for develop your Drupal site, you can use this container with official [nginx](https://hub.docker.com/_/nginx) and [mariadb](https://hub.docker.com/_/mariadb) (or [mysql](https://hub.docker.com/_/mysql)) image.
And you can also use image that contains set of libphp 5.6/7.0/7.1/7.2/7.3 (depending on tag) and apache2.

## How to use

See https://github.com/blauerberg/drupal-on-docker

## License

MIT
