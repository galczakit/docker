FROM php:7.2-fpm

ENV UNIX_USER=1000

RUN apt-get update

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libpng-dev libjpeg-dev curl libcurl4-openssl-dev libxml2-dev unzip less libxslt-dev libfreetype6-dev sendmail libwebp-dev libxpm-dev

RUN docker-php-ext-configure gd --with-freetype-dir --with-jpeg-dir --with-png-dir
RUN docker-php-ext-install gd curl dom iconv pdo pdo_mysql simplexml ctype mbstring hash soap xsl zip json bcmath intl mysqli opcache sockets
RUN apt-get update && apt-get install -y \
    libmagickwand-dev --no-install-recommends \
    && pecl install imagick \
	&& docker-php-ext-enable imagick
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql
RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=172.17.0.1" >> /usr/local/etc/php/conf.d/xdebug.ini

RUN curl -O https://download.libsodium.org/libsodium/releases/libsodium-1.0.18.tar.gz \
  && tar xfvz libsodium-1.0.18.tar.gz \
  && cd libsodium-1.0.18 \
  && ./configure \
  && make && make install \
  && pecl install -f libsodium

# create 'owner' user
RUN useradd -u ${UNIX_USER} owner
RUN chsh -s /bin/bash owner

# run fpm as a 'owner'
RUN sed -i -e 's/www-data/owner/g' /usr/local/etc/php-fpm.d/www.conf

# run apache as a 'owner'
RUN echo "memory_limit=2024M" > /usr/local/etc/php/conf.d/memory.ini

RUN mkdir /home/owner
RUN chown owner:owner /home/owner

# install composer
RUN curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
RUN php /tmp/composer-setup.php --version=1.10.1 --install-dir=/bin/ --filename=composer
RUN rm /tmp/composer-setup.php
