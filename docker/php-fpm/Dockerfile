FROM php:7.3-fpm-alpine

RUN apk --update add libzip-dev zlib-dev libpng-dev && \
    docker-php-ext-install pdo_mysql zip gd


