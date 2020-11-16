# docker-lnmp
docker-compose Docker LNMP (Nginx, PHP74&PHP56 with swoole, MySQL57, Redis, Memcached)

### 目录结构

```bash
.
├── data
│   ├── mysql
│   └── redis
├── docker-compose.yml
├── logs
│   ├── nginx
│   ├── php56
│   └── php74
├── services
│   ├── memcached
│   ├── mysql
│   │   ├── Dockerfile
│   │   ├── conf.d
│   │   │   └── mysql.cnf
│   │   └── my57.cnf
│   ├── nginx
│   │   ├── Dockerfile
│   │   ├── conf.d
│   │   │   ├── cert
│   │   │   │   ├── test.key
│   │   │   │   └── test.pem
│   │   │   ├── rewrite
│   │   │   │   └── rewrite.conf
│   │   │   └── servers
│   │   │       └── www.conf
│   │   ├── fastcgi_params
│   │   └── nginx.conf
│   ├── php56
│   │   ├── Dockerfile
│   │   ├── extensions
│   │   │   ├── imagick-3.4.4.tgz
│   │   │   ├── memcached-2.2.0.tgz
│   │   │   ├── mongodb-1.5.5.tgz
│   │   │   ├── redis-4.1.1.tgz
│   │   │   ├── scws-1.2.3.tar.bz2
│   │   │   ├── swoole-2.0.11.tgz
│   │   │   └── xdebug-2.5.5.tgz
│   │   ├── php-fpm.conf
│   │   └── php.ini
│   ├── php74
│   │   ├── Dockerfile
│   │   ├── Dockerfile.down
│   │   ├── extensions
│   │   │   ├── imagick-3.4.4.tgz
│   │   │   ├── mcrypt-1.0.3.tgz
│   │   │   ├── memcached-3.1.5.tgz
│   │   │   ├── redis-5.3.1.tgz
│   │   │   ├── scws-1.2.3.tar.bz2
│   │   │   ├── swoole-4.5.5.tgz
│   │   │   └── xdebug-2.9.8.tgz
│   │   ├── php-fpm.conf
│   │   └── php.ini
│   └── redis
│       ├── Dockerfile
│       └── redis.conf
└── www
    └── app
        ├── index.php
        └── php56
            └── index.php
```


### 操作步骤
```
git clone https://github.com/Nevermore-dh/docker-lnmp.git

cd docker-lnmp

docker-compose up -d
```


### 配置说明

- **./services/nginx/conf.d/servers/www.conf** 中 `fastcgi_pass   l_docker_php74:9000` 配置需要和 **PHP** **service** 或者 **container_name** 保持一致
- **./services/nginx/conf.d/servers/www.conf** 中 `location /php56 {...}` 配置只是为了验证 **PHP56** 和 **PHP74** 两个版本共存，可根据需要配置自己的规则或者独立的域名



### PHP 相关扩展安装命令及依赖列表
| **扩展名** | **依赖** | **PHP74 安装方式** | **PHP56 安装方式** |
| --- | --- | --- | --- |
| bcmath | - | docker-php-ext-install bcmath | docker-php-ext-install bcmath |
| bz2 | apk add bzip2-dev | docker-php-ext-install bz2 | docker-php-ext-install bz2 |
| gd | apk add libpng-dev libjpeg-turbo-dev freetype-dev | docker-php-ext-configure gd --with-freetype --with-jpeg && docker-php-ext-install  gd | docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && docker-php-ext-install gd |
| gettext | apk add gettext-dev | docker-php-ext-install gettext | docker-php-ext-install gettext |
| mcrypt | apk add libmcrypt-dev | pecl install mcrypt-1.0.3.tgz | docker-php-ext-install mcrypt |
| memcached | apk add libmemcached-dev zlib-dev | pecl install memcached-3.1.5.tgz | pecl install memcached-2.2.0.tgz |
| mysql | - | removed | docker-php-ext-install mysql |
| mysqli | - | docker-php-ext-install mysqli | docker-php-ext-install mysqli |
| pcntl | - | docker-php-ext-install pcntl | docker-php-ext-install pcntl |
| pdo_mysql | - | docker-php-ext-install pdo_mysql | docker-php-ext-install pdo_mysql |
| redis | - | 详见 Dockerfile | 详见 Dockerfile |
| scws | - | 详见 Dockerfile | 详见 Dockerfile |
| shmop | - | docker-php-ext-install shmop | docker-php-ext-install shmop |
| soap | apk add libxml2-dev | docker-php-ext-install soap | docker-php-ext-install soap |
| sockets | - | docker-php-ext-install sockets | docker-php-ext-install sockets |
| swoole | - | 详见 Dockerfile | 详见 Dockerfile |
| sysvmsg | - | docker-php-ext-install sysvmsg | docker-php-ext-install sysvmsg |
| sysvsem | - | docker-php-ext-install sysvsem | docker-php-ext-install sysvsem |
| sysvshm | - | docker-php-ext-install sysvshm | docker-php-ext-install sysvshm |
| xmlrpc | apk add libxml2-dev | docker-php-ext-install xmlrpc | docker-php-ext-install xmlrpc |
| zip | apk add libzip-dev | docker-php-ext-install zip | docker-php-ext-install zip |

希望本文对你有所帮助，如有不足之处，请不吝赐教～
