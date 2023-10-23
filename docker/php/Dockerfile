FROM php:8.2.11-fpm
COPY php.ini /usr/local/etc/php/

RUN apt-get update \
  && apt-get install -y vim zlib1g-dev mariadb-client curl gnupg libzip-dev \
  && docker-php-ext-install zip pdo_mysql
RUN curl -sL https://deb.nodesource.com/setup_20.x | bash -
RUN apt-get install -y nodejs
RUN npm install npm@latest -g

#Composer install
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === trim(file_get_contents('https://composer.github.io/installer.sig'))) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER 1

ENV COMPOSER_HOME /composer

ENV PATH $PATH:/composer/vendor/bin


WORKDIR /var/www

RUN composer global require "laravel/installer"

COPY install-laravel /usr/local/bin/
RUN chmod 755 /usr/local/bin/install-laravel

COPY remove-laravel /usr/local/bin/
RUN chmod 755 /usr/local/bin/remove-laravel
