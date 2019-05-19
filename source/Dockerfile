# docker build . -t yohang/php-laravel:latest

FROM php:7.2-fpm-alpine

RUN apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS && \
    apk add --no-cache --virtual .imagemagick-edge \
            --repository http://dl-3.alpinelinux.org/alpine/v3.6/main/ \
            --allow-untrusted \
        imagemagick \
        imagemagick-dev && \
    apk add --update --virtual .pecl-build-deps \
        icu-dev \
        curl-dev \
        freetype-dev \
        pcre-dev \
        postgresql-dev \
        libtool \
        libmcrypt-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libxml2-dev && \
    apk add --no-cache --virtual .build-dependencies $PHPIZE_DEPS \
        zlib-dev \
        cyrus-sasl-dev \
        g++ \
        libtool \
        make \
        pcre-dev && \
    apk add \
        git \
        curl \
        bash \
        bash-completion \
        icu \
        pcre \
        freetype \
        libmcrypt \
        libintl \
        libjpeg-turbo \
        imagemagick \
        libpng \
        libltdl \
        libxml2 \
        mysql-client \
        nodejs \
        postgresql-client \
        libmemcached-dev && \
    export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" && \
    pecl install \
        apcu \
        imagick-3.4.3 \
        xdebug \
        memcached && \
    docker-php-ext-enable imagick && \
    docker-php-ext-enable apcu && \
    docker-php-ext-enable memcached && \
    docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure bcmath && \
    docker-php-ext-install \
        soap \
        zip \
        curl \
        bcmath \
        exif \
        gd \
        iconv \
        intl \
        mbstring \
        opcache \
        pdo_mysql \
        pdo_pgsql && \
    apk del \
        .pecl-build-deps .phpize-deps


# Configure version constraints
ENV PATH=/application:/application/vendor/bin:/root/.composer/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1

# Install composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer

WORKDIR /application

COPY . /application

RUN /usr/local/bin/composer install --prefer-dist

