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

## How to use

See https://github.com/blauerberg/drupal-on-docker

## License

MIT
