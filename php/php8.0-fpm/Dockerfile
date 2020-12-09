FROM php:8.0-fpm

ARG COMPOSER_VERSION

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 设置镜像源
RUN cat /dev/null > /etc/apt/sources.list
RUN echo '\
deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster main non-free contrib \n\
deb http://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main \n\
deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main non-free contrib \n\
deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main non-free contrib \n\
' >> /etc/apt/sources.list

# 安装依赖
RUN apt-get update && apt-get install -y --no-install-recommends \
        libzip-dev \
        libpng-dev \
        libjpeg-dev \
        libfreetype6-dev \
        libpq-dev
    
# 安装核心扩展
RUN set -eux; \

    # 安装 opcache
    docker-php-ext-install opcache; \

    # 安装 zip 扩展
    docker-php-ext-install zip; \

    # 安装 pdo_mysql 扩展
    docker-php-ext-install pdo_mysql; \

    # 安装 gd 扩展
    docker-php-ext-configure gd \
            --prefix=/usr \
            --with-jpeg \
            --with-freetype; \
    docker-php-ext-install gd; \
    php -r 'var_dump(gd_info());' \

    # 安装bcmath
    docker-php-ext-install bcmath;

    # 安装composer
    RUN wget https://getcomposer.org/download/${COMPOSER_VERSION}/composer.phar -O /usr/local/bin/composer
    RUN chmod +x /usr/local/bin/composer

# 清理缓存和无用软件
RUN rm -rf /var/lib/apt/lists/* && apt-get -y autoclean && apt-get -y clean && apt-get -y autoremove
