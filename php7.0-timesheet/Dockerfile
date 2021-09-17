FROM php:7.0-fpm

ENV UNIX_USER=1000

RUN apt-get update && apt-get install -y \
        git \
        curl \
        mysql-client \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libssl-dev \
        libmemcached-dev \
        libz-dev \
        zlib1g-dev \
        libsqlite3-dev \
        zip \
        libxml2-dev \
        libcurl3-dev \
        libedit-dev \
        libpspell-dev \
        libldap2-dev \
        unixodbc-dev \
        libpq-dev \
        libpng-dev \
        libjpeg-dev \
        libcurl4-openssl-dev \
        unzip \
        less \
        libxslt-dev \
        sendmail \
        libwebp-dev \
        libxpm-dev \
        python2.7 \
        python-pip \
        xauth \
        xvfb \
        wkhtmltopdf \
        wget \
        && apt-get install -y libmagickwand-dev --no-install-recommends

RUN ln -fs /usr/lib/x86_64-linux-gnu/libldap.so /usr/lib/

RUN pip install --upgrade google-api-python-client

RUN echo "Installing PHP extensions" \
    && docker-php-ext-install -j$(nproc) iconv mcrypt gd pdo_mysql mysqli pdo_pgsql pcntl pdo_sqlite zip curl bcmath opcache simplexml xmlrpc xml soap session readline pspell ldap mbstring \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-enable iconv mcrypt gd pdo_mysql pdo_pgsql mysqli pcntl pdo_sqlite zip curl bcmath opcache simplexml xmlrpc xml soap session readline pspell ldap mbstring
RUN pecl install imagick \
	&& docker-php-ext-enable imagick

RUN curl -O https://download.libsodium.org/libsodium/releases/libsodium-1.0.18.tar.gz \
  && tar xfvz libsodium-1.0.18.tar.gz \
  && cd libsodium-1.0.18 \
  && ./configure \
  && make && make install \
  && pecl install -f libsodium

RUN wget -O /usr/local/etc/php/conf.d/browscap.ini https://browscap.org/stream?q=PHP_BrowsCapINI
RUN sed -E 's/;/\\;/g' /usr/local/etc/php/conf.d/browscap.ini > /usr/local/etc/php/conf.d/browscap.ini
RUN sed -E 's/[\\;]{40}/;;;/g' /usr/local/etc/php/conf.d/browscap.ini > /usr/local/etc/php/conf.d/browscap.ini
RUN sed -E "s/\'/\\\'/g" /usr/local/etc/php/conf.d/browscap.ini > /usr/local/etc/php/conf.d/browscap.ini

RUN curl -fsSL https://deb.nodesource.com/setup_6.x | bash -
RUN apt-get install -y nodejs
RUN apt-get update && apt-get install -y npm
RUN npm install -g bower gulp gulp-cli
#RUN npm install -g npm@3.10.10

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
