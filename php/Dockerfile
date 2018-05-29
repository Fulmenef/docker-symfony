# See https://github.com/docker-library/php/blob/4677ca134fe48d20c820a19becb99198824d78e3/7.0/fpm/Dockerfile
FROM php:7.1-fpm

MAINTAINER Maxence POUTORD <maxence.poutord@gmail.com>

USER root

RUN apt-get update && apt-get install -y \
    php-calendar \
#    openssl \
    bzip2 \
    libcurl4-openssl-dev \
    libicu-dev \
    libfreetype6-dev \
    libmcrypt-dev \
    libxml2-dev \
    libxslt-dev \
    libssl-dev \
    libjpeg-dev \
    libpng-dev \
    unzip \
    ssmtp \
#    ssh-client \
    nodejs \
    npm \
    mailutils && \
    docker-php-ext-configure bcmath && \
        docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) \
        bcmath \
        gd \
        intl \
        mcrypt \
        opcache \
        pdo_mysql \
        soap \
        xsl \
        sockets \
        calendar \
        zip && \
    apt-get clean && \
    perl -pi -e "s/mailhub=mail/mailhub=maildev/" /etc/ssmtp/ssmtp.conf && \
    npm install -g yarn

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql

# Install and activate SSL
#RUN \
#    a2enmod ssl; \
#    mkdir -p /etc/ssl/private/ /etc/ssl/certs/ && \
#    openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
#        -keyout /etc/ssl/private/localhost.key -out /etc/ssl/certs/localhost.crt \
#        -subj "/CN=localhost"

# Install gulp-cli globally
RUN \
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash && \
    export NVM_DIR="$HOME/.nvm" && \
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && \
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" && \
    nvm install node && \
    nvm use node && \
    npm install -g gulp-cli

RUN echo 'alias sf="php app/console"' >> ~/.bashrc
RUN echo 'alias sf3="php bin/console"' >> ~/.bashrc

# Install custom PHP configuration
COPY custom.ini /usr/local/etc/php/conf.d/

WORKDIR /var/www/html