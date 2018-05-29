FROM php:7.2-fpm

MAINTAINER Axel PERROUD <axel.perroud@outlook.com>

USER root

RUN apt-get update && apt-get install -y \
    libxml2-dev \
    libxslt-dev \
    libfreetype6-dev \
    libjpeg-dev \
    gnupg \
    nodejs \
    unzip \
    ssmtp \
    mailutils && \
    docker-php-ext-configure bcmath && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) \
        bcmath \
        intl \
        gd \
        opcache \
        pdo \
        pdo_mysql \
        xsl \
        zip \
        calendar && \
    perl -pi -e "s/mailhub=mail/mailhub=maildev/" /etc/ssmtp/ssmtp.conf

# Install npm and yarn
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    apt-get install -y \
        npm && \
    apt-get clean && \
    npm install -g yarn

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN echo 'alias sf="php bin/console"' >> ~/.bashrc

# Install custom PHP configuration
COPY custom.ini /usr/local/etc/php/conf.d/

WORKDIR /var/www/html