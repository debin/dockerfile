FROM php:7.4-fpm-alpine

LABEL MAINTAINER="debin<touch767@gmail.com>"
LABEL description="php-fpm 镜像，已开启FCGI，可用于web服务，也可以用于运行 cli 程序。基于php:7.4-fpm-alpine官方镜像，包含redis,zip,pdo_mysql,composer等扩展"

ENV TZ "Asia/Shanghai"
ENV TERM xterm
# 默认关闭opcode
ENV OPCODE 0

ARG REDIS_VERSION=5.3.7
ARG COMPOSER_VERSION=2.8.2

# 切换alpine国内源
#RUN echo 'https://mirrors.aliyun.com/alpine/v3.16/main/' > /etc/apk/repositories && \
#    echo 'https://mirrors.aliyun.com/alpine/v3.16/community/' >> /etc/apk/repositories

# 通过pecl指令安装扩展
# PHPIZE_DEPS 包含 gcc g++ 等编译辅助类库，完成编译后删除
RUN apk update \
    && apk add $PHPIZE_DEPS \
    && pecl install redis-${REDIS_VERSION} \
    && docker-php-ext-enable redis \
    && apk del $PHPIZE_DEPS \
# 通过docker-php-ext-install指令安装扩展
#RUN apk update \
    && apk add --no-cache freetype freetype-dev libpng libpng-dev libjpeg-turbo libjpeg-turbo-dev libwebp-dev \
    && docker-php-ext-configure gd --with-webp=/usr/include/webp --with-jpeg=/usr/include --with-freetype=/usr/include/freetype2/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) opcache \
    && docker-php-ext-install -j$(nproc) bcmath \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
# 安装composer
    && curl https://getcomposer.org/download/${COMPOSER_VERSION}/composer.phar  -o /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer \
# 收尾
    && rm -rf /var/cache/apk/* \
    && rm -rf /root/.cache \
    && rm -rf /tmp/*
# docker-php-ext-install 指令已经包含编译辅助类库的删除逻辑

# php.ini配置文件修改
RUN cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"  \
    && sed -i 's/^;* *date.timezone.*/date.timezone = Asia\/Shanghai/'  "$PHP_INI_DIR/php.ini" \
    && sed -i 's/^;* *upload_max_filesize.*/upload_max_filesize = 16M/' "$PHP_INI_DIR/php.ini" \
    && sed -i 's/^;* *post_max_size.*/post_max_size = 17M/'               "$PHP_INI_DIR/php.ini" 
