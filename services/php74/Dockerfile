FROM php:7.4-fpm

RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN apt-get clean && apt-get update
RUN cp "/usr/share/zoneinfo/Asia/Shanghai" /etc/localtime
RUN echo "Asia/Shanghai" > /etc/timezone
RUN apt-get -y install wget

ADD ./install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && sync

RUN install-php-extensions imagick bcmath mcrypt gd sysvsem rdkafka pcntl swoole \
    redis bz2 gettext memcached mongodb pdo_mysql shmop soap sockets zip xmlrpc \
    sysvmsg mysqli sysvshm xdebug
RUN install-php-extensions amqp zookeeper

RUN mkdir /tmp/extensions

WORKDIR /tmp/extensions

RUN mkdir /tmp/scws \
    && wget http://www.xunsearch.com/scws/down/scws-1.2.3.tar.bz2 \
    && tar -xjf /tmp/extensions/scws-1.2.3.tar.bz2 -C /tmp/scws --strip-components=1 \
    && cd /tmp/scws  \
    && ./configure --prefix=/usr/local/scws  \
    && make && make install  \
    && ( cd phpext && phpize && ./configure --with-scws=/usr/local/scws && make && make install ) \
    && cd /usr/local/scws/etc \
    && wget http://www.xunsearch.com/scws/down/scws-dict-chs-gbk.tar.bz2 \
    && wget http://www.xunsearch.com/scws/down/scws-dict-chs-utf8.tar.bz2 \
    && tar -xvjf scws-dict-chs-gbk.tar.bz2 \
    && tar -xvjf scws-dict-chs-utf8.tar.bz2 \
    && docker-php-ext-enable scws \
    && rm -rf /tmp/*