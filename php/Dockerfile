FROM php:7-fpm
RUN apt-get update \
    && docker-php-ext-install pdo pdo_mysql mysqli iconv mbstring json
# imagik
RUN runtimeRequirements="libmagickwand-6.q16-dev --no-install-recommends" \
	&& apt-get update && apt-get install -y ${runtimeRequirements} \
	&& ln -s /usr/lib/x86_64-linux-gnu/ImageMagick-6.8.9/bin-Q16/MagickWand-config /usr/bin/ \
	&& pecl install imagick \
	&& echo "extension=imagick.so" > /usr/local/etc/php/conf.d/ext-imagick.ini \
	&& rm -rf /var/lib/apt/lists/*

COPY ./upload.ini /usr/local/etc/php/conf.d/upload.ini

# xdebug
ADD http://xdebug.org/files/xdebug-2.4.0.tgz /

RUN tar -xvzf /xdebug-2.4.0.tgz \
    && cd xdebug-2.4.0 \
    && phpize \
    && ./configure \
    && make \
    && cp modules/xdebug.so /usr/local/lib/php/extensions/no-debug-non-zts-20151012/ \
    && make clean

RUN echo "xdebug.max_nesting_level = 250" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable = on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart = off" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_connect_back = on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port = 9000" >> /usr/local/etc/php/conf.d/xdebug.ini

RUN docker-php-ext-enable xdebug

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
	--install-dir=/usr/local/bin \
	--filename=composer

CMD ["php-fpm"]