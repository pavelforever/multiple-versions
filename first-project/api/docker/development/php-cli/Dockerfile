FROM php:8.2-cli-alpine

ENV XDEBUG_VERSION 3.1.3
RUN apk add --no-cache fcgi git \
    && docker-php-ext-install \
#      xdebug \
      bcmath
RUN apk add --no-cache libpq-dev \
  libpng-dev \
  libzip-dev
RUN  docker-php-ext-install    gd
RUN  docker-php-ext-install    mysqli
RUN  docker-php-ext-install    pdo
RUN  docker-php-ext-install    pdo_mysql
RUN  docker-php-ext-install    opcache
#    && git clone --branch $XDEBUG_VERSION --depth 1 https://github.com/xdebug/xdebug.git /usr/src/php/ext/xdebug \
#    && docker-php-ext-configure xdebug --enable-xdebug-dev \
#RUN apk del git

RUN apk add --no-cache unzip

RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini

COPY ./common/php/conf.d /usr/local/etc/php/conf.d
COPY ./development/php/conf.d /usr/local/etc/php/conf.d

COPY ./common/wait-for-it.sh /usr/local/bin/wait-for-it
RUN chmod 555 /usr/local/bin/wait-for-it

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/bin --filename=composer --quiet

RUN addgroup -g 1000 app && adduser -u 1000 -G app -s /bin/sh -D app

WORKDIR /app

USER app
