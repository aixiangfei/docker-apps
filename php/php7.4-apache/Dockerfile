FROM php:7.4-apache

ARG COMPOSER_VERSION

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 设置镜像源
RUN echo '\
deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free\n\
deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free\n\
deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free\n\
deb http://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free\n\
' > /etc/apt/sources.list

# 安装核心扩展
RUN apt-get update && apt-get install -y \
        wget \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libzip-dev \
        libpq-dev \
        libmemcached-dev \
        zlib1g-dev \
    && docker-php-ext-install \
        opcache \
        gd \
        zip \
        pdo_mysql \
        pdo_pgsql \
        pgsql \
        bcmath

RUN pecl install redis memcached \
    && docker-php-ext-enable redis memcached

# 安装composer
RUN wget https://getcomposer.org/download/${COMPOSER_VERSION}/composer.phar -O /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer

RUN apt-get install -y \
        iputils-ping \
        vim \
        supervisor \
    && apt-get -y autoremove && apt-get clean && rm -rf /tmp/*
