FROM php:7.1-fpm-stretch
ENV TZ=Asia/Shanghai
# 换源
RUN sed -i "s@/deb.debian.org/@/mirrors.aliyun.com/@g" /etc/apt/sources.list \
  && apt-get -y update --assume-yes \
  && apt-get install -y --assume-yes tzdata \
                        libfreetype6-dev \
                        libjpeg62-turbo-dev \
                        libpng-dev \
                        procps \
  && apt-get -y --assume-yes install curl \
                        libbz2-dev \
                        libcurl4-openssl-dev \
                        libfreetype6-dev \
                        libgmp-dev \
                        libicu-dev \
                        libjpeg62-turbo-dev \
                        libldap2-dev \
                        libmcrypt-dev \
                        libmemcached-dev \
                        libncurses5-dev \
                        libpng-dev \
                        libssl-dev \
                        libxml2-dev \
                        libxslt-dev \
                        netcat \
                        unzip \
                        zlib1g-dev \
                        librabbitmq-dev \
  # amqp 扩展
  && pecl install amqp 1.9.4 && docker-php-ext-enable amqp \
  && docker-php-source extract \
  && docker-php-ext-install bcmath \
  # gd 库
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && docker-php-ext-install -j$(nproc) gd \
  && docker-php-ext-install pdo_mysql \
  # redis,swoole,mongo
  && pecl install redis-5.1.1 && docker-php-ext-enable redis \
  && pecl install swoole && docker-php-ext-enable swoole \
  && pecl install mongodb && docker-php-ext-enable mongodb \
  # soap
  && docker-php-ext-configure soap --enable-soap && docker-php-ext-install soap \
  && docker-php-ext-install sockets \
                            sysvmsg \
                            sysvsem \
                            sysvshm \
                            xmlrpc \
                            zip \
                            mcrypt \
                            gettext \
                            pcntl \
                            shmop \
  && cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini \
  # 时间
  && echo $TZ > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata \
  # composer
  && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
  # 减少镜像大小
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*



