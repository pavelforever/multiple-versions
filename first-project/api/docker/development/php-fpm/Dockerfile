FROM php:8.1-fpm-alpine

ENV XDEBUG_VERSION 3.1.2

RUN apk add --no-cache fcgi git \
    && docker-php-ext-install \
#      xdebug \
      bcmath
RUN apk add --no-cache libpq-dev \
  libpng-dev \
  libzip-dev
RUN   docker-php-ext-install   gd
RUN   docker-php-ext-install   mysqli
RUN   docker-php-ext-install   pdo
RUN   docker-php-ext-install   pdo_mysql
RUN   docker-php-ext-install   opcache
#    && git clone --branch $XDEBUG_VERSION --depth 1 https://github.com/xdebug/xdebug.git /usr/src/php/ext/xdebug \
#    && docker-php-ext-configure xdebug --enable-xdebug-dev \
RUN apk del git

RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini

COPY ./common/php/conf.d /usr/local/etc/php/conf.d
COPY ./common/php/php-fpm.d /usr/local/etc/php-fpm.d
COPY ./development/php/conf.d /usr/local/etc/php/conf.d
COPY ./development/php-fpm/conf.d /usr/local/etc/php/conf.d

WORKDIR /app

COPY ./development/php-fpm/entrypoint.sh /usr/local/bin/docker-php-entrypoint
RUN chmod +x /usr/local/bin/docker-php-entrypoint

HEALTHCHECK --interval=5s --timeout=3s --start-period=1s \
    CMD REDIRECT_STATUS=true SCRIPT_NAME=/ping SCRIPT_FILENAME=/ping REQUEST_METHOD=GET \
    cgi-fcgi -bind -connect 127.0.0.1:9000 || exit 1
