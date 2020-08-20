FROM composer:latest AS composer

FROM php:7.4.9-fpm-alpine3.12

LABEL maintainer="John Komarov <komarov.j@gmail.com>"

COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
    && apk add --no-cache libnsl libaio-dev icu-dev \
    && ln -s /usr/lib/libnsl.so.2 /usr/lib/libnsl.so.1 \
    \
    && pecl install xdebug-2.9.6 \
    && pecl install apcu-5.1.18 \
    \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install -j$(nproc) opcache intl pdo_mysql \
    && docker-php-ext-enable xdebug apcu \
    \
    && apk del .phpize-deps