FROM phalconphp/php-fpm:7-min

ARG SWOOLE_VER=1.10.3
ARG REDIS_VER=3.1.6
ARG DOC_ROOT=/data/www

RUN ln -fs /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && dpkg-reconfigure -f noninteractive tzdata

#COPY ./sources.list /etc/apt/sources.list
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    zlib1g-dev

RUN docker-php-ext-install pdo_mysql bcmath mbstring \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

RUN pecl install -f redis-${REDIS_VER} \
    && docker-php-ext-enable redis


#swoole
RUN curl -L -o /tmp/swoole.tgz https://pecl.php.net/get/swoole-${SWOOLE_VER}.tgz \
    && tar xfz /tmp/swoole.tgz \
    && rm -r /tmp/swoole.tgz \
    && mkdir -p /usr/src/php/ext \
    && mv swoole-${SWOOLE_VER} /usr/src/php/ext/swoole \
    && docker-php-ext-install swoole \
    && rm -rf /usr/src/php/ext/swoole

RUN apt-get autoremove && \
    apt-get autoclean && \
    apt-get clean

RUN mkdir -p ${DOC_ROOT} \
    && chown -R www-data.www-data ${DOC_ROOT}
WORKDIR ${DOC_ROOT}

CMD ["php-fpm"]