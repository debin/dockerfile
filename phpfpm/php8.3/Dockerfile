FROM php:8.3-fpm-alpine3.24

LABEL org.opencontainers.image.title="php-fpm-runtime" \
    org.opencontainers.image.authors="debin" \
    org.opencontainers.image.description="PHP-FPM 镜像，支持 FPM 和 CLI。基于php:8.3-fpm-alpine3.24官方镜像，包含 Redis、ZIP、PDO MySQL、Composer 等"

# 基础镜像已包含时区数据，直接设置
ENV TZ="Asia/Shanghai"

# 版本号查看
# https://pecl.php.net/package/redis
# https://getcomposer.org/download/
# https://github.com/mlocati/docker-php-extension-installer/releases
ARG REDIS_VERSION=6.3.0
ARG COMPOSER_VERSION=2.10.3
ARG INSTALL_PHP_EXTENSIONS_VERSION=2.12.3

# 国内云服务器可按需切换 Alpine 软件源（阿里云腾讯云二选一），版本必须与基础镜像一致
# RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
# RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.cloud.tencent.com/g' /etc/apk/repositories

# 安装 PHP 扩展安装脚本
RUN curl --retry 3 --retry-delay 2 -sSLf \
    -o /usr/local/bin/install-php-extensions \
    https://github.com/mlocati/docker-php-extension-installer/releases/download/${INSTALL_PHP_EXTENSIONS_VERSION}/install-php-extensions && \
    chmod 755 /usr/local/bin/install-php-extensions

# 安装 PHP 扩展及 Composer
RUN IPE_GD_WITHOUTAVIF=1 install-php-extensions \
    gd \
    bcmath \
    pdo_mysql \
    pcntl \
    zip \
    redis-${REDIS_VERSION} \
    @composer-${COMPOSER_VERSION}

# 使用生产环境配置并自定义优化项
RUN cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" \
    && printf '%s\n' \
        'date.timezone = Asia/Shanghai' \
        'upload_max_filesize = 16M' \
        'post_max_size = 17M' \
        'expose_php = Off' \
        'opcache.enable = 1' \
        'opcache.validate_timestamps = 1' \
        'opcache.revalidate_freq = 60' \
        'session.upload_progress.enabled = 0' \
        > "$PHP_INI_DIR/conf.d/99-docker-custom.ini"
