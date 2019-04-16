FROM php:5.6-fpm

ARG SWOOLE_VER=1.10.3
ARG REDIS_VER=3.1.6
ARG XDEBUG_VER=2.5.5
ARG DOC_ROOT=/var/www

RUN ln -fs /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && dpkg-reconfigure -f noninteractive tzdata

#COPY ./sources.list /etc/apt/sources.list
RUN apt-get update && apt-get install -y --no-install-recommends \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    zlib1g-dev \
    curl \
    libmemcached-dev \
    libssl-dev \
    libmcrypt-dev

RUN docker-php-ext-install pdo_mysql bcmath mbstring mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

RUN pecl install -f redis-${REDIS_VER} \
    && docker-php-ext-enable redis

RUN pecl install -f xdebug-${XDEBUG_VER} \
    && docker-php-ext-enable xdebug

RUN pecl install mongo \
    && docker-php-ext-enable mongo

RUN apt-get autoremove && \
    apt-get autoclean && \
    apt-get clean && \
    rm -rf /tmp/pear

RUN mkdir -p ${DOC_ROOT} \
    && chown -R www-data.www-data ${DOC_ROOT}
WORKDIR ${DOC_ROOT}

CMD ["php-fpm"]