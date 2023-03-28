FROM php:8.2-fpm-buster

ENV COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

# Xdebug設定
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

COPY --from=composer:2.2 /usr/bin/composer /usr/bin/composer

# Node.jsをインストール
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get install -y nodejs

# Xdebugインストール
RUN apt-get install -y libxml2-dev libzip-dev

RUN pecl install xdebug-3.2.1 && \
  docker-php-ext-enable xdebug

RUN apt-get update && \
  apt-get -y install --no-install-recommends git unzip libzip-dev libicu-dev libonig-dev && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  docker-php-ext-install intl pdo_mysql zip bcmath

COPY ./php.ini /usr/local/etc/php/php.ini

WORKDIR /data