FROM php:7-fpm
RUN apt-get update \
    && docker-php-ext-install pdo pdo_mysql mysqli iconv mbstring json

    RUN gpg --keyserver pool.sks-keyservers.net --recv-keys 8277377A

    ENV MAGICK_VERSION 6.9.1-10

    RUN apt-get update -y \
      && apt-get install -y --no-install-recommends \
        libpng-dev libjpeg-dev libtiff-dev libopenjpeg-dev \
      && apt-get remove -y imagemagick

    RUN cd /tmp
    RUN curl -SLO "http://imagemagick.org/download/releases/ImageMagick-${MAGICK_VERSION}.tar.xz" \
      && curl -SLO "http://imagemagick.org/download/releases/ImageMagick-${MAGICK_VERSION}.tar.xz.asc" \
      && gpg --verify "ImageMagick-${MAGICK_VERSION}.tar.xz.asc" "ImageMagick-${MAGICK_VERSION}.tar.xz" \
      && tar xf "ImageMagick-${MAGICK_VERSION}.tar.xz"

    # http://www.imagemagick.org/script/advanced-unix-installation.php#configure
    RUN cd "ImageMagick-${MAGICK_VERSION}" \
      && ./configure \
        --disable-static \
        --enable-shared \

        --with-jpeg \
        --with-jp2 \
        --with-openjp2 \
        --with-png \
        --with-tiff \
        --with-quantum-depth=8 \

        --without-magick-plus-plus \
        # disable BZLIB support
        --without-bzlib \
        # disable ZLIB support
        --without-zlib \
        # disable Display Postscript support
        --without-dps \
        # disable FFTW support
        --without-fftw \
        # disable FlashPIX support
        --without-fpx \
        # disable DjVu support
        --without-djvu \
        # disable fontconfig support
        --without-fontconfig \
        # disable Freetype support
        --without-freetype \
        # disable JBIG support
        --without-jbig \
        # disable lcms (v1.1X) support
        --without-lcms \
        # disable lcms (v2.X) support
        --without-lcms2 \
        # disable Liquid Rescale support
        --without-lqr \
         # disable LZMA support
        --without-lzma \
        # disable OpenEXR support
        --without-openexr \
        # disable PANGO support
        --without-pango \
        # disable TIFF support
        --without-webp \
        # don't use the X Window System
        --without-x \
        # disable XML support
        --without-xml \

      && make \
      && make install \
      && ldconfig /usr/local/lib

    RUN apt-get -y autoclean && apt-get -y autoremove && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


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

CMD ["php-fpm"]