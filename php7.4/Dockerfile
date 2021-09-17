FROM php:7.4-fpm

ENV UNIX_USER=1000

RUN apt-get update && apt-get install -y \
    libpng-dev \
    libwebp-dev \
    libxpm-dev \
    libjpeg-dev \
    curl libcurl4-openssl-dev \
    libxml2-dev \
    unzip \
    less \
    libxslt-dev \
    libfreetype6-dev \
    sendmail \
    libzip-dev \
    git \
    ssh-client \
    libpq-dev \
    && apt-get install -y libmagickwand-dev --no-install-recommends

RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-xpm --with-webp
RUN docker-php-ext-install gd calendar curl dom iconv pdo pdo_mysql simplexml ctype soap xsl json bcmath intl mysqli opcache zip sockets
RUN pecl install imagick \
	&& docker-php-ext-enable imagick
RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql
RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.mode=develop,debug" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.discover_client_host=1" >> /usr/local/etc/php/conf.d/xdebug.ini

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
RUN php /tmp/composer-setup.php --version=2.0.12 --install-dir=/bin/ --filename=composer
RUN rm /tmp/composer-setup.php

#Mailhog
RUN curl -Lsf 'https://storage.googleapis.com/golang/go1.8.3.linux-amd64.tar.gz' | tar -C '/usr/local' -xvzf -
ENV PATH /usr/local/go/bin:$PATH
RUN go get github.com/mailhog/mhsendmail
RUN cp /root/go/bin/mhsendmail /usr/bin/mhsendmail
RUN echo 'sendmail_path = /usr/bin/mhsendmail --smtp-addr mailhog:1025' > /usr/local/etc/php/php.ini

RUN chown -R owner:owner /var/www/

RUN apt-get install -y openssh-server
RUN service ssh start
