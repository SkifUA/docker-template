FROM php:7-fpm
RUN apt-get update \
    && docker-php-ext-install pdo pdo_mysql mysqli iconv mbstring json
CMD ["php-fpm"]