FROM centos:7

# Version
ENV PHPREDIS_VERSION = 3.1.6

# Libs
RUN   yum update -y   \
      && yum clean all  \
      && yum makecache   \
      && yum -y install \
       gcc \
       gcc-c++ \
       automake \
       autoconf \
       libtool \
       make \
       wget \
       epel-release\
       libxml2-devel\
       m4 pcre-devel \
       cmake \
       bison \
       openssl \
       openssl-devel \
       php-pear \
       php-devel \
       && yum clean all  \
       && rm -rf /var/cache/yum/*

RUN  mkdir /data && cd /data    \
     &&  wget  http://cn2.php.net/distributions/php-7.2.15.tar.gz    \
     && tar -zxf php-7.2.15.tar.gz     \
     && cd php-7.2.15     \
     && ./configure \
     --prefix=/usr/local/php  \
     --prefix=/usr/local/php   \
     --with-config-file-path=/usr/local/php/etc     \
     -with-libxml-dir=/usr    \
     --with-mhash \--with-openssl    \
     --with-mysqli=shared,mysqlnd    \
     --with-pdo-mysql=shared,mysqlnd     \
     --with-zlib    \
     --enable-zip    \
     --enable-inline-optimization    \
     --disable-debug    \
     --disable-rpath     \
     --enable-shared    \
     --enable-xml     \
     --enable-bcmath    \
     --enable-shmop   \
     --enable-sysvsem  \
     --enable-mbregex    \
     --enable-mbstring   \
     --enable-pcntl      \
     --enable-sockets   \
     --without-pear     \
     --with-gettext        \
     --enable-session         \
      && make && make install

RUN  mkdir   /usr/local/php/etc \
    && cp /data/php-7.2.15/php.ini-production /usr/local/php/etc/php.ini \
    && echo "export PATH=$PATH:/usr/local/php/bin/" >> /etc/profile \
    && source /etc/profile
#COPY php.ini     /usr/local/php/etc/
#安装redis extension                                                                                # 安装扩展
RUN  wget https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz -O /data/redis.tar.tgz \
    #wget http://pecl.php.net/get/redis-${PHPREDIS_VERSION}.tgz -O /data/redis.tar.tgz \
    && pecl install /data/redis.tar.tgz \
    && echo "extension=redis.so" >> /usr/local/php/etc/php.ini \
    && rm -rf /data/redis.tar.tgz \
    && rm -rf  /data/php-7.2.15.tar.gz      \
    && rm -rf  /data/php-7.2.15

EXPOSE 9000
#设置工作目录
WORKDIR  /www
