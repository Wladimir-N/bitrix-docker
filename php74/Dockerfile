FROM php:7.4-fpm
RUN apt update && \
    apt install --no-install-recommends -y zlib1g-dev libpng++-dev libfreetype6-dev libjpeg62-turbo-de && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install mysqli gd
ADD ./php.ini /usr/local/etc/php/conf.d/90-php.ini
RUN usermod -u 1000 www-data
WORKDIR "/var/www/bitrix"
EXPOSE 9000
