FROM php:8.1-apache

# 設定ファイルをコンテナ内にコピー
COPY ./php.ini /usr/local/etc/php
COPY ./000-default.conf /etc/apache2/sites-enabled/

# composerの取得・設定
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

# npm取得
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash -

# ミドルウェアインストール
RUN apt-get update \
&& apt-get install -y \
nodejs npm \
git \
zip \
unzip \
vim \
mariadb-client \
&& docker-php-ext-install pdo_mysql

# Apache設定
RUN mv /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled
RUN a2enmod headers
RUN /bin/sh -c a2enmod rewrite

# 安定版のLaravel取得
RUN composer global require "laravel/installer"

# ユーザーを作成
ARG DOCKER_UID=1000
ARG DOCKER_USER=docker
ARG DOCKER_PASSWORD=docker
ARG ROOT_USER=root
ARG ROOT_PASSWORD=root
RUN useradd -m --uid ${DOCKER_UID} --groups sudo ${DOCKER_USER} \
    && echo ${DOCKER_USER}:${DOCKER_PASSWORD} | chpasswd \
    && echo ${ROOT_USER}:${ROOT_PASSWORD} | chpasswd

# 作成したユーザーに切り替える
USER ${DOCKER_USER}


