FROM php:7.4.3-fpm

COPY ./docker/php/php.ini /usr/local/etc/php/
COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer

RUN apt-get update && \
    apt-get install -y git unzip libzip-dev libicu-dev libonig-dev && \
    docker-php-ext-install intl pdo_mysql zip bcmath

WORKDIR /var/www/