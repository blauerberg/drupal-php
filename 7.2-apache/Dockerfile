FROM php:7.2-apache
MAINTAINER Yoshikazu Aoyama <yskz.aoyama@gmail.com>

RUN a2enmod rewrite
RUN apt-get update && apt-get install -y \
        git \
        mysql-client \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zlib1g-dev \
        libicu-dev \
        g++ \
        unzip \
        ssmtp \
    && pecl install xdebug \
    && docker-php-ext-enable mcrypt \
    && docker-php-ext-install -j$(nproc) mcrypt \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-configure intl \ 
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install -j$(nproc) zip \
    && docker-php-ext-install -j$(nproc) pdo \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) mysqli \
    && docker-php-ext-install -j$(nproc) opcache

    RUN pecl install xdebug
    RUN docker-php-ext-enable xdebug

    RUN pecl install apcu \
    && docker-php-ext-enable apcu \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /var/cache/apt/archives/*

RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && ln -s /usr/local/bin/composer /usr/bin/composer

RUN /usr/local/bin/composer config -g repos.packagist composer https://packagist.jp
RUN /usr/local/bin/composer global require "hirak/prestissimo"
RUN /usr/local/bin/composer global require "drush/drush:8.x" \
        && ln -s ~/.composer/vendor/bin/drush /usr/local/bin/drush \
        && /usr/local/bin/drush -y init

WORKDIR /var/www/html
