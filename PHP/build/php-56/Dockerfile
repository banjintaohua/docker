# php:5.6.40-fpm already comes with the following extensions
#
# [PHP Modules]
#   Core ctype curl date dom ereg fileinfo filter ftp hash iconv json libxml mbstring mhash mysqlnd
#   openssl pcre PDO pdo_sqlite Phar posix readline Reflection session SimpleXML
#   SPL sqlite3 standard tokenizer xml xmlreader xmlwriter zlib
# [Zend Modules]

FROM php:5.6.40-fpm

LABEL maintainer="banjintaohua"

ENV TZ Asia/Shanghai

RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini \
    && pear config-set php_ini /usr/local/etc/php/php.ini \
    # debian 9.x (buster) mirror \
    && mv /etc/apt/sources.list /etc/apt/sources.list.bak \
    && echo 'deb http://archive.debian.org/debian stretch main' > /etc/apt/sources.list \
    # apt install
    && apt update \
    && apt install -y vim dnsutils iputils-ping \
    && apt install -y netcat-openbsd \
    && apt install -y procps \
    && apt install -y mtr \
    && apt install -y lrzsz \
    && apt install -y git \
    && apt install -y libmcrypt-dev \
    && apt install -y libpng-dev libssl-dev \
    && apt install -y libzip-dev zlib1g-dev \
    && apt install -y nginx \
    # docker-php-ext-install
    && docker-php-ext-install mysql mysqli pdo_mysql zip \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install gd \
    && docker-php-ext-install zip \
    # clear
    && apt clean && rm -rf /tmp/* /var/cache/apt/*

COPY --from=composer:1.10 /usr/bin/composer /usr/local/bin/composer
COPY --chown=root:root --chmod=775 docker-nginx-php-entrypoint.sh /usr/local/bin/docker-nginx-php-entrypoint
COPY --chown=root:root --chmod=644 xdebug-2.5.5.so /usr/local/lib/php/extensions/no-debug-non-zts-20131226/xdebug.so

WORKDIR /var/www

EXPOSE 80 9000

#ENTRYPOINT ["docker-nginx-php-entrypoint"]
#CMD ["nginx", "-g", "daemon off; error_log /dev/stderr info;"]

ENTRYPOINT ["docker-php-entrypoint"]
CMD ["php-fpm"]