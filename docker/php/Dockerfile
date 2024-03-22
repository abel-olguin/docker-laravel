FROM php:8.3-fpm

WORKDIR /var/www/html
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

ENV TZ=UTC

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update
RUN apt-get install -y wget gnupg gosu curl ca-certificates zip unzip git supervisor sqlite3 libcap2-bin  \
    libpng-dev librsvg2-bin fswatch ffmpeg nano

RUN chmod +x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions bcmath mbstring intl mysqli pdo pdo_mysql opcache ldap readline pgsql sqlite3 gd curl imap xml zip bcmath soap imagick memcached

RUN curl -sLS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer
RUN apt-get -y autoremove && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY php.ini /etc/php/8.3/cli/conf.d/99-app.ini

CMD composer install && php /var/www/html/artisan db:wipe && php /var/www/html/artisan migrate:refresh --seed && php /var/www/html/artisan key:generate && php /var/www/html/artisan storage:link && php-fpm

EXPOSE 9000
