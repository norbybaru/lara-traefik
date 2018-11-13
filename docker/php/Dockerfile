# PHP Application
FROM php:7.1-fpm

LABEL maintainer="norbyb@roam.africa"

# Install any custom system requirements here
RUN apt-get update \
    && apt-get install -y libpq-dev libmcrypt-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev zlib1g-dev \
     libxml2-dev libxslt-dev libcurl3 libcurl3-dev gnupg gnupg2 \
     nano mysql-client unzip\
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install zip mbstring pdo pdo_mysql mysqli bcmath opcache json xsl intl curl \
    && docker-php-ext-enable mysqli

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash
RUN apt-get install --yes nodejs
RUN node -v
RUN npm -v

# Install Yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update \
    && apt-get install yarn --yes
RUN yarn --version

ENV APP_ENV ${APP_ENV}

# DIR
WORKDIR /var/www/app
COPY . ./

# composer configuration:
COPY ./composer.json ./composer.json

# node configuration
COPY ./package.json ./package.json

# install depencies
RUN composer install --no-interaction
#RUN yarn install

# build assets
#RUN yarn development

# www-data:
RUN chown -R www-data:www-data ./
RUN chown -R www-data:www-data storage

# mode-root:
USER root

CMD ["php-fpm"]
