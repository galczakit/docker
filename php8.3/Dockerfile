FROM php:8.3-fpm

ARG UNIX_USER_ARG=1000
ENV UNIX_USER=${UNIX_USER_ARG}

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
RUN docker-php-ext-install ctype soap xsl bcmath intl gd calendar curl dom iconv pdo pdo_mysql simplexml mysqli opcache zip sockets
RUN pecl install imagick \
	&& docker-php-ext-enable imagick
RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

# create 'owner' user
RUN adduser --disabled-password --uid ${UNIX_USER} owner
RUN chsh -s /bin/bash owner

# run fpm as a 'owner'
RUN sed -i -e 's/www-data/owner/g' /usr/local/etc/php-fpm.d/www.conf

# run apache as a 'owner'
RUN echo "memory_limit=2024M" > /usr/local/etc/php/conf.d/memory.ini

# install composer
RUN curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
RUN php /tmp/composer-setup.php --install-dir=/bin/ --filename=composer
RUN rm /tmp/composer-setup.php

#Mailhog
RUN curl -Lsf 'https://storage.googleapis.com/golang/go1.8.3.linux-amd64.tar.gz' | tar -C '/usr/local' -xvzf -
ENV PATH=/usr/local/go/bin:$PATH
RUN go get github.com/mailhog/mhsendmail
RUN cp /root/go/bin/mhsendmail /usr/bin/mhsendmail
RUN echo 'sendmail_path = /usr/bin/mhsendmail --smtp-addr mailhog:1025' > /usr/local/etc/php/php.ini

RUN chown -R owner:owner /var/www/
