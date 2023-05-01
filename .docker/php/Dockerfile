FROM php:8.1.0-fpm

RUN apt-get update

RUN apt-get install -y \ 
        openssl \
        bash \
        vim \
        nano \
        htop \
        curl \
        libmcrypt-dev \
        libpng-dev \
        libzip-dev \
        openrc \
        git \
        sudo

RUN docker-php-ext-install \ 
        pdo \ 
        pdo_mysql \
        zip \
        -j"$(getconf _NPROCESSORS_ONLN)" gd  \
        bcmath \
        sockets

RUN docker-php-ext-configure gd \
        && docker-php-ext-enable gd 


RUN pecl install xdebug \
        && docker-php-ext-enable xdebug

COPY ./php.ini.development "$PHP_INI_DIR/php.ini"

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN rm -rf /var/www/html

EXPOSE 9000

WORKDIR /var/www/project/

RUN usermod -u 1000 www-data
RUN chown -R www-data /var/www/project/

RUN mkdir /var/www/.composer/
RUN chown -R www-data /var/www/.composer/

USER www-data

RUN rm -rf /var/cache/apk/*

CMD ["php-fpm", "--nodaemonize"]
