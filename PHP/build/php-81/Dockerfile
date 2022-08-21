FROM php:8.1-fpm

LABEL maintainer="banjintaohua"

# COPY . /tmp/build
COPY --from=composer:1.10 /usr/bin/composer /usr/local/bin/composer

# 通过 PECL 安装拓展遇到连接超时问题时, 需要设置代理
# Connection to `pecl.php.net:80' failed: Connection refused
# sudo pear config-set http_proxy http://10.211.55.2:1087

# 使用网易源会有问题，暂时先用官方的源吧
# RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak \
#     && echo 'deb http://mirrors.163.com/debian/ stretch main non-free contrib' > /etc/apt/sources.list \
#     && echo 'deb http://mirrors.163.com/debian/ stretch-updates main non-free contrib' >> /etc/apt/sources.list \
#     && echo 'deb http://mirrors.163.com/debian-security/ stretch/updates main non-free contrib' >> /etc/apt/sources.list \
#     && apt update \

RUN apt update \
    # 基本工具包
    && apt install -y vim dnsutils iputils-ping \
    && apt install -y netcat-openbsd \
    # 命令补全工具
    && apt install -y bash-completion \
    && echo 'source /etc/profile.d/bash_completion.sh' >> /root/.bashrc \
    && alias ll='ls -l' \
    # Git
    && apt install -y git \
    # PHP 配置文件
    && cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini \
    && pear config-set php_ini /usr/local/etc/php/php.ini \
    # 数据库
    && docker-php-ext-install mysqli pdo_mysql \
    # GD
    && apt -y install libssl-dev \
    && apt install -y libjpeg62-turbo-dev libpng-dev libfreetype6-dev \
    && docker-php-ext-configure gd --with-jpeg --with-freetype \
    && docker-php-ext-install gd \
    # intl 国际化拓展
    && apt -y install libicu-dev \
    && docker-php-ext-install intl \
    # SOAP 扩展
    && apt -y install libxml2-dev \
    && docker-php-ext-install soap \
    # Excel 依赖
    && apt install -y libzip-dev zlib1g-dev \
    && docker-php-ext-install zip \
    # 其他依赖
    && docker-php-ext-install bcmath gettext pcntl shmop sockets sysvsem opcache \
    # redis 缓存
    && pecl install redis \
    && docker-php-ext-enable redis \
    # 优先使用 Xdebug3，Xdebug3 的性能比 Xdebug2 的好
    && pecl install xdebug-3.1.2 \
    # 微信
    # && cd /usr/src/php/ext \
    # && git clone https://github.com/pangdahua/php7-wxwork-finance-sdk.git \
    # && cd php7-wxwork-finance-sdk \
    # && phpize \
    # && export INSTALL_PHP_PATH=/usr/local/bin \
    # && export WXWORK_FINANCE_SDK=/usr/local/php/wxwork \
    # && ./configure --with-php-config=$INSTALL_PHP_PATH/php-config --with-wxwork-finance-sdk=$WXWORK_FINANCE_SDK \
    # 清理缓存
    && rm -rf /tmp/* /var/cache/apt/*

 WORKDIR /var/www