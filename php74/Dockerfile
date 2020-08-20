FROM php:7.4-fpm
ADD ./php.ini /usr/local/etc/php/conf.d/90-php.ini
RUN usermod -u 1000 www-data
WORKDIR "/var/www/bitrix"
EXPOSE 9000
