FROM php:7.4-fpm-buster
LABEL maintainer="snowcat <snowcat.dew.jp@gmail.com>"
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

# timezone environment
ENV TZ=Asia/Tokyo \
  # locale
  LANG=ja_JP.utf8 \
  LANGUAGE=ja_JP:ja \
  LC_ALL=ja_JP.utf8 \
  # composer environment
  COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer

RUN apt update && \
  apt -y install git libicu-dev libonig-dev libzip-dev unzip locales libpq-dev && \
  apt clean && \
  rm -rf /var/lib/apt/lists/* && \
  locale-gen ja_JP.utf8 && \
  localedef -f UTF-8 -i ja_JP ja_JP.UTF-8 && \
  mkdir /var/run/php-fpm && \
  docker-php-ext-install intl pdo_pgsql zip bcmath && \
  composer config -g process-timeout 3600 && \
  composer config -g repos.packagist composer https://packagist.org

COPY ./docker/app/php-fpm.d/zzz-www.conf /usr/local/etc/php-fpm.d/zzz-www.conf
COPY ./docker/app/php.ini /usr/local/etc/php/php.ini

WORKDIR /work/src
