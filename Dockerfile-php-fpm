ARG PHP_VERSION
FROM php:${PHP_VERSION}

ARG PHP_XDEBUG
ARG PHP_SWOOLE
ARG PHP_REDIS
ARG PHP_MEMCACHED
ARG PHP_IMAGICK
ARG REPLACE_SOURCE_LIST

# install base rely on
# search by https://pkgs.alpinelinux.org/packages
# alpine not include gcc g++ make
RUN apk --update add --no-cache gcc g++ make \
    autoconf \
    bzip2 \
    bzip2-dev \
    binutils \
    cyrus-sasl-dev \
    freetype-dev \
    icu-dev \
    libc-dev \
    icu-dev \
    libpcre32 \
    libbz2 \
    \
    imagemagick-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libmcrypt-dev \
    libmemcached-dev \
    openldap-dev \
    rabbitmq-c \
    rabbitmq-c-dev

# install core extensions
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install bz2 \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install opcache \
    && docker-php-ext-install ldap \
    && docker-php-ext-install sockets \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install sysvmsg \
    && docker-php-ext-install sysvsem \
    && docker-php-ext-install sysvshm \
    \
    # install pecl state version extensions
    # search from https://pecl.php.net/package-search.php
    \
    # install memcached
    && pecl install memcached ${PHP_MEMCACHED} \
    && docker-php-ext-enable memcached \
    \
	# install redis
    && pecl install redis ${PHP_REDIS} \
    && docker-php-ext-enable redis \
    \
    # install imagick
    && pecl install imagick ${PHP_IMAGICK} \
    && docker-php-ext-enable imagick \
    \
    # install swoole
    && pecl install swoole ${PHP_SWOOLE} \
    && docker-php-ext-enable swoole \
    \
    && pecl update-channels \
    && rm -rf /var/cache/apk/* \
    && rm -rf /tmp/pear

#
# build image
# new image
#

FROM php:${PHP_VERSION}

RUN apk --update add --no-cache \
    bzip2 \
    bzip2-dev \
    binutils \
    cyrus-sasl-dev \
    freetype-dev \
    icu-dev \
    libc-dev \
    icu-dev \
    libpcre32 \
    libbz2 \
    \
    imagemagick-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libmcrypt-dev \
    libmemcached-dev \
    openldap-dev \
    rabbitmq-c \
    rabbitmq-c-dev \
    && rm -rf /var/cache/apk/* \
    && rm -rf /tmp/pear

COPY --from=0 /usr/local/lib/php/extensions/no-debug-non-zts-20170718/* /usr/local/lib/php/extensions/no-debug-non-zts-20170718/
COPY --from=0 /usr/local/etc/php/conf.d/* /usr/local/etc/php/conf.d/

ENTRYPOINT ["docker-php-entrypoint"]

EXPOSE 9000

CMD ["php-fpm"]