FROM php:7.4-apache

# 設定ファイルをdockerコンテナ内のPHP、Apacheに読み込ませる
ADD ./docker/php/php.ini /usr/local/etc/php/
ADD ./docker/apache/000-default.conf /etc/apache2/sites-enabled/

# Composerのインストール 素のままインストールすると2.x系が入ってしまうので、1系を指定している
RUN cd /usr/bin && curl -s http://getcomposer.org/installer | php -- --version=1.10.15 && ln -s /usr/bin/composer.phar /usr/bin/composer

# ミドルウェアインストール
RUN apt-get update \
&& apt-get install -y \
git \
zip \
unzip \
vim \
libpng-dev \
libpq-dev \
libfreetype6-dev \
libjpeg62-turbo-dev \
libwebp-dev \
libxpm-dev \
&& docker-php-ext-install pdo_mysql \
&& docker-php-ext-configure gd \
 --with-freetype \
 --with-jpeg \
 --with-webp \
 --with-xpm \
&& docker-php-ext-install -j$(nproc) gd

# Node.jsをインストール
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get install -y nodejs

RUN mv /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled
RUN /bin/sh -c a2enmod rewrite

WORKDIR /var/www/html/