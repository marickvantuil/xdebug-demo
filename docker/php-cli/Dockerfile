FROM php:7.3-cli-alpine

RUN apk --update add libzip-dev zlib-dev libpng-dev git && \
    docker-php-ext-install pdo_mysql zip gd && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    php -r "unlink('composer-setup.php');"
