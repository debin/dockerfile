##  php-fpm docker
php-fpm 镜像，已开启FCGI，可用于web服务，也可以用于运行 cli 程序。基于php:fpm-alpine官方镜像，包含redis,memcached,zip,pdo_mysql,composer等扩展

###  run php 8
```
sudo docker run -d --name fpm8 -p 9000:9000 -u $UID:$UID -v /opt/htdocs:/opt/htdocs  blueblue/php-fpm

```

###  run php 8 witch php.ini
```
sudo docker run -d --rm --name fpm8  blueblue/php-fpm
mkdir -p /opt/data/php8
sudo docker cp fpm8:/usr/local/etc /opt/data/php8/
sudo docker stop fpm8
# php.ini path: /opt/data/php8/etc/php/php.ini
sudo docker run -d --name fpm8 -p 9000:9000 -u $UID:$UID -v /opt/htdocs:/opt/htdocs  -v /opt/data/php8/etc:/usr/local/etc  blueblue/php-fpm
```

###  run php 5.6
```
sudo docker run -d --name fpm5 -p 9000:9000 -u $UID:$UID -v /opt/htdocs:/opt/htdocs  blueblue/php-fpm:5.6-fpm-alpine
```

###  PHP 8 Cli
```
cat << 'TEXT' > /usr/local/bin/php
#!/bin/sh
export PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin
echo "Current working directory: '"$(pwd)"'"
sudo docker exec fpm8 php $@
TEXT

sudo chmod +x /usr/local/bin/php

php -v
php /opt/htdocs/xxx/xxx.php
```

###  PHP 8 composer
```
cat << 'TEXT' > /usr/local/bin/composer
#!/bin/sh
export PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin
echo "Current working directory: '"$(pwd)"'"
sudo docker exec fpm8 composer $@

TEXT

sudo chmod +x /usr/local/bin/composer

composer -V
composer install -d /opt/htdocs/xxxx
```

