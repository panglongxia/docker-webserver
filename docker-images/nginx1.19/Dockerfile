FROM debian:latest

ENV TZ=Asia/Shanghai
ENV NGINX_VERSION=1.19.3
ENV SOFTWARE_DIR=/software

RUN sed -i "s@/deb.debian.org/@/mirrors.aliyun.com/@g" /etc/apt/sources.list \
  && apt-get -y update --assume-yes \
  && apt-get install -y --assume-yes tzdata \
                                     procps \
                                     wget \
                                     libpcre3 \
                                     libpcre3-dev \
                                     zlib1g-dev \
                                     openssl \
                                     libssl-dev \
                                     build-essential \
                                     gcc \
                                     libxml2 \
                                     libxml2-dev \
                                     libxslt1-dev \
                                     perl-modules \
                                     make \
                                     libperl-dev \
  && useradd -M -s /sbin/nologin nginx \
  && mkdir -p ${SOFTWARE_DIR}/src \
  # 下载nginx 可以自行先下载好 然后 CP YourNginxPackagePath ${SOFTWARE_DIR}/src/
  && wget -P ${SOFTWARE_DIR}/src/ http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz \
  && tar zxvf ${SOFTWARE_DIR}/src/nginx-${NGINX_VERSION}.tar.gz -C ${SOFTWARE_DIR}/src/ \
  # 下载zlib 可以自行先下载好 然后 CP YourZlibPackagePath ${SOFTWARE_DIR}/src/
  && wget -P ${SOFTWARE_DIR}/src/ https://www.zlib.net/zlib-1.2.11.tar.gz \
  && tar zxvf ${SOFTWARE_DIR}/src/zlib-1.2.11.tar.gz -C ${SOFTWARE_DIR}/src/ \
  && cd ${SOFTWARE_DIR}/src/nginx-${NGINX_VERSION} \
  && ./configure --prefix=${SOFTWARE_DIR}/nginx \
                 --user=nginx \
                 --group=nginx \
                 --with-http_v2_module \
                 --with-http_dav_module \
                 --with-http_flv_module \
                 --with-http_addition_module \
                 --with-http_xslt_module \
                 --with-http_stub_status_module \
                 --with-http_sub_module \
                 --with-http_random_index_module \
                 --with-http_degradation_module \
                 --with-http_secure_link_module \
                 --with-http_gzip_static_module \
                 --with-http_perl_module \
                 --with-file-aio \
                 --with-mail \
                 --with-mail_ssl_module \
                 --with-pcre \
                 --with-http_ssl_module \
                 --with-pcre \
                 --with-http_realip_module \
                 --with-stream \
                 --with-zlib=${SOFTWARE_DIR}/src/zlib-1.2.11 \
                 --with-ld-opt="-Wl,-E" \
  && make && make install \
  && ln -s ${SOFTWARE_DIR}/nginx/sbin/nginx /usr/bin/nginx \
  # 时间
  && echo $TZ > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD ["nginx", "-g", "daemon off;"]