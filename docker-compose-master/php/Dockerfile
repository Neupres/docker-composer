FROM centos:7
MAINTAINER wslin
RUN yum -y install epel-release
RUN yum install -y gcc gcc-c++  libxml2 libxml2-devel openssl openssl-devel bzip2 bzip2-devel libcurl libcurl-devel libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel gmp gmp-devel libmcrypt libmcrypt-devel readline readline-devel libxslt libxslt-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel ncurses curl gdbm-devel db4-devel libXpm-devel libX11-devel gd-devel gmp-devel expat-devel xmlrpc-c xmlrpc-c-devel libicu-devel libmcrypt-devel libmemcacheddevel libsqlite3x-devel oniguruma-devel make perl
ADD libzip-1.2.0.tar.gz /tmp
RUN cd /tmp/libzip-1.2.0 && \
    ./configure && \
    make && \
    make install
ADD php-7.3.9.tar.gz /tmp
RUN echo "/usr/local/lib64">>/etc/ld.so.conf && \
    echo "/usr/local/lib">>/etc/ld.so.conf && \
    echo "/usr/lib">>/etc/ld.so.conf && \
    echo "/usr/lib64">>/etc/ld.so.conf && \
    ldconfig -v
RUN cd /tmp/php-7.3.9 && \
    ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-curl --with-freetype-dir --enable-gd --with-gettext --with-iconv-dir --with-kerberos --with-libdir=lib64 --with-libxml-dir --with-mysqli --with-openssl --with-pcre-regex --with-pdo-mysql --with-pdo-sqlite --with-pear --with-png-dir --with-jpeg-dir --with-xmlrpc --with-xsl --with-zlib --with-bz2 --with-mhash --enable-fpm --enable-bcmath --enable-libxml --enable-inline-optimization --enable-mbregex --enable-mbstring --enable-opcache --enable-pcntl --enable-shmop --enable-soap --enable-sockets --enable-sysvsem --enable-sysvshm --enable-xml --enable-zip --enable-fpm && \
    cp /usr/local/lib/libzip/include/zipconf.h /usr/local/include/ && \
    make -j 4 && make install && \
    cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf && \
    cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf  && \
    sed -i "s/127.0.0.1/0.0.0.0/g" /usr/local/php/etc/php-fpm.d/www.conf && \
    cp ./sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm && \
    chmod +x /etc/init.d/php-fpm
#COPY php.ini /usr/local/php/etc
EXPOSE 9000
CMD /etc/init.d/php-fpm start  && tail -F /var/log/messages