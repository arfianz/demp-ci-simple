FROM php:7.4-fpm

# Set Environment Variables
ENV DEBIAN_FRONTEND noninteractive

#
#--------------------------------------------------------------------------
# Software's Installation
#--------------------------------------------------------------------------
#
# Installing tools and PHP extentions using "apt", "docker-php", "pecl",
#

# Install "curl", "libmemcached-dev", "libpq-dev", "libjpeg-dev",
#         "libpng-dev", "libfreetype6-dev", "libssl-dev", "libmcrypt-dev",
RUN set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
            curl \
            libmemcached-dev \
            libz-dev \
            libpq-dev \
            libjpeg-dev \
            libpng-dev \
            libfreetype6-dev \
            libssl-dev \
            libmcrypt-dev \
            libonig-dev; \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    # Install the PHP pdo_mysql extention
    docker-php-ext-install pdo_mysql; \
    # Install the PHP pdo_pgsql extention
    docker-php-ext-install pdo_pgsql; \
    # Install the PHP gd library
    docker-php-ext-configure gd \
            --prefix=/usr \
            --with-jpeg \
            --with-freetype; \
    docker-php-ext-install gd; \
    php -r 'var_dump(gd_info());'

#--------------------------------------------------------------------------
# xDebug:
#--------------------------------------------------------------------------
RUN pecl install xdebug && \
    docker-php-ext-enable xdebug

#--------------------------------------------------------------------------
# MongoDB:
#--------------------------------------------------------------------------
#RUN pecl install mongodb && \
#    docker-php-ext-enable mongodb

#--------------------------------------------------------------------------
# ZipArchive:
#--------------------------------------------------------------------------
#RUN pecl install zip && \
#    docker-php-ext-enable zip

#--------------------------------------------------------------------------
# PHP Memcached:
#--------------------------------------------------------------------------
#RUN curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz" \
#    && mkdir -p memcached \
#    && tar -C memcached -zxvf /tmp/memcached.tar.gz --strip 1 \
#    && ( \
#        cd memcached \
#        && phpize \
#        && ./configure \
#        && make -j$(nproc) \
#        && make install \
#    ) \
#    && rm -r memcached \
#    && rm /tmp/memcached.tar.gz \
#    && docker-php-ext-enable memcached

#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
ADD ./codeigniter.ini /usr/local/etc/php/conf.d
ADD ./vh-codeigniter.pool.conf /usr/local/etc/php-fpm.d/

RUN rm -r /var/lib/apt/lists/*

RUN usermod -u 1000 www-data

WORKDIR /var/www/laravel

CMD ["php-fpm"]

EXPOSE 9000
