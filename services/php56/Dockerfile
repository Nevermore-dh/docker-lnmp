FROM php:5.6.40-fpm-alpine

RUN sed -i "s/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g" /etc/apk/repositories \
  && apk update \
  && apk --no-cache add tzdata \
    && cp "/usr/share/zoneinfo/Asia/Shanghai" /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
  &&  apk add --no-cache --virtual .build-deps autoconf g++ libtool make linux-headers \
    libpng-dev libjpeg-turbo-dev freetype-dev bzip2-dev \
    gettext-dev \
    libmcrypt-dev \
    libxml2-dev \
    libmemcached-dev zlib-dev \
    libzip-dev \
    file-dev \
    imagemagick-dev \
    --repository http://mirrors.aliyun.com/alpine/edge/community gnu-libiconv \
  && docker-php-ext-install -j$(nproc) bcmath bz2 gettext \
    mcrypt mysql mysqli pcntl pdo_mysql \
    shmop soap sockets \
    sysvmsg sysvsem sysvshm xmlrpc zip \
  && docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install -j$(nproc) gd

COPY ./extensions /tmp/extensions
WORKDIR /tmp/extensions

RUN pecl install redis-4.1.1.tgz \
    && docker-php-ext-enable redis \
  && pecl install memcached-2.2.0.tgz \
    && docker-php-ext-enable memcached \
  && pecl install imagick-3.4.4.tgz \
    && docker-php-ext-enable imagick \
  && pecl install mongodb-1.5.5.tgz \
    && docker-php-ext-enable mongodb \
  && mkdir /tmp/swoole \
    && tar -xf /tmp/extensions/swoole-2.0.11.tgz -C /tmp/swoole --strip-components=1 \
    && ( cd /tmp/swoole && phpize && ./configure && make -j$(nproc) && make install ) \
    && docker-php-ext-enable swoole \
  && pecl install xdebug-2.5.5.tgz \
    && docker-php-ext-enable xdebug \
  &&  mkdir /tmp/scws \
    && tar -xjf /tmp/extensions/scws-1.2.3.tar.bz2 -C /tmp/scws --strip-components=1 \
    && cd /tmp/scws  \
    && ./configure --prefix=/usr/local/scws  \
    && make && make install  \
    && ( cd phpext && phpize && ./configure --with-scws=/usr/local/scws && make && make install ) \
    && cd /usr/local/scws/etc \
    && wget http://www.xunsearch.com/scws/down/scws-dict-chs-gbk.tar.bz2 \
    && wget http://www.xunsearch.com/scws/down/scws-dict-chs-utf8.tar.bz2 \
    && tar -xvjf scws-dict-chs-gbk.tar.bz2 \
    && tar -xvjf scws-dict-chs-utf8.tar.bz2 \
    && docker-php-ext-enable scws \
    && rm -rf /tmp/*

ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php

WORKDIR /var/www/html