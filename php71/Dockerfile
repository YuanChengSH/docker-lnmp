FROM hub.c.163.com/library/php:7.1-fpm

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    git \
    mysql-client \
    libmemcached-dev \
    libz-dev \
    libzip-dev \
    libpq-dev \
    libjpeg-dev \
    libpng12-dev \
    libfreetype6-dev \
    libicu-dev \
    libssl-dev \
    libmemcached-dev \
    zlib1g-dev \
    libmcrypt-dev && \
    docker-php-ext-configure gd \
        --enable-gd-native-ttf \
        --with-freetype-dir=/usr/include/freetype2 \
        --with-png-dir=/usr/include \
        --with-jpeg-dir=/usr/include && \
    docker-php-ext-install gd pdo_mysql mysqli opcache intl bcmath zip mcrypt sockets && \
    docker-php-ext-enable bcmath zip pdo_mysql mcrypt sockets

RUN pecl install memcached
RUN pecl install xdebug
RUN pecl install redis
RUN php -r "readfile('https://getcomposer.org/installer');" > composer-setup.php && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer
RUN composer config -g repo.packagist composer https://packagist.phpcomposer.com
RUN curl -fsSLO --compressed "https://github.com/edenhill/librdkafka/archive/v0.11.6.tar.gz" \
    && tar -xzf v0.11.6.tar.gz -C /tmp/ \
    && rm -rf v0.11.6.tar.gz \
    && cd /tmp/librdkafka-0.11.6/ \
    && ./configure \
    && make \
    && make install \
    && rm -rf /tmp/librdkafka-0.11.6 \
    && pecl install rdkafka