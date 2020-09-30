FROM centos:7

ARG PHP_MOUNT_DIR="74"

ARG PHP_VERSION="7.4.8"

ARG PHPIZE_DEPS="gcc wget libxml2-devel openssl-devel libjpeg-devel libpng-devel freetype-devel curl-devel boost-system libmcrypt-devel libyaml-devel openldap-devel libgearman libgearman-devel GraphicsMagick GraphicsMagick-devel ImageMagick-devel gcc-c++ librabbitmq-devel automake autoconf libtool make glibc net-tools sqlite-devel oniguruma-devel"

ARG PHP_EXTRA_CONFIGURE_ARGS="--with-libdir=/lib64 --with-mysqli --with-pdo-mysql --with-zlib --with-curl --with-mcrypt  --enable-gd --with-jpeg --with-png-dir --with-freetype-dir --with-openssl --with-mhash --with-ldap --disable-rpath --disable-ipv6 --enable-pcntl --enable-fpm --enable-gd-native-ttf --enable-sockets --enable-mbregex --enable-mbstring --enable-zip --enable-soap --enable-bcmath --enable-calendar --enable-opcache --enable-inline-optimization"

COPY php-${PHP_VERSION}.tar.gz /tmp

#运行命令
RUN  cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
     && mkdir -p /data/php${PHP_MOUNT_DIR} \
     && mkdir -p /data/php${PHP_MOUNT_DIR}/extension \
     && mkdir -p /data/php${PHP_MOUNT_DIR}/fpm \
     && yum install -y epel-release \
     && yum install -y ${PHPIZE_DEPS} \
     && ln -s /usr/lib64/libldap* /usr/lib/ \
     && cd /tmp \
     && wget https://cmake.org/files/v3.12/cmake-3.12.0-rc1.tar.gz \
     && tar -zxvf cmake-3.12.0-rc1.tar.gz \
     && cd cmake-3.12.0-rc1 \
     && ./bootstrap \
     && gmake \
     && gmake install \
     && cd /tmp \
     && wget https://libzip.org/download/libzip-1.5.2.tar.gz \
     && tar -zxf libzip-1.5.2.tar.gz \
     && cd libzip-1.5.2 \
     && mkdir build \
     && cd build \
     && cmake .. \
     && make -j4 \
     && make install \
     && cd /tmp \
     && echo -e "/usr/local/lib64\n/usr/local/lib\n/usr/lib\n/usr/lib64" >> /etc/ld.so.conf \
     && ldconfig -v \
     && tar xf php-${PHP_VERSION}.tar.gz  \
     && cd php-${PHP_VERSION}  \
     && ./configure --prefix=/usr/local/php --with-config-file-path=/data/php${PHP_MOUNT_DIR} ${PHP_EXTRA_CONFIGURE_ARGS}  \
     && make \
     && make install \
     && make clean \
     && ln -s /usr/local/php/bin/php /sbin/php \
     && ln -s /usr/local/php/bin/phpize /sbin/phpize \
     && ln -s /usr/local/php/sbin/php-fpm /sbin/php-fpm \
     && mv /usr/local/php/etc/php-fpm.conf.default  /usr/local/php/etc/php-fpm.conf \
     && cd /tmp \
     && curl -o go-pear.php https://pear.php.net/go-pear.phar \
     && php go-pear.php \
     && ln -s /usr/local/php/bin/pecl /sbin/pecl \
     && ln -s /usr/local/php/bin/pear /sbin/pear \
     && pecl channel-update pecl.php.net \
     && rm -rf *.tar.gz cmake-3.12.0-rc1 libzip-1.5.2 php-${PHP_VERSION}

#运行php-fpm命令

CMD ["php-fpm"]

STOPSIGNAL SIGQUIT
