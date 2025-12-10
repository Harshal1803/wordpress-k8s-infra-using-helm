FROM debian:bullseye-slim


RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    libpcre3 \
    libpcre3-dev \
    libssl-dev \
    zlib1g-dev \
    libpq-dev \
    libicu-dev \
    libreadline-dev \
    libncurses5-dev \
    libtool \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /tmp

ENV OPENRESTY_VERSION=1.27.1.2
ENV OPENRESTY_DOWNLOAD_URL=https://github.com/openresty/openresty/releases/download/v${OPENRESTY_VERSION}/openresty-${OPENRESTY_VERSION}.tar.gz

RUN curl --retry 5 --retry-delay 5 --connect-timeout 30 --tlsv1.2 \
      -fSL "$OPENRESTY_DOWNLOAD_URL" -o openresty.tar.gz \
    && tar -xzf openresty.tar.gz \
    && cd openresty-${OPENRESTY_VERSION} \
    && ./configure \
        --prefix=/opt/openresty \
        --with-pcre-jit \
        --with-ipv6 \
        --without-http_redis2_module \
        --with-http_iconv_module \
        --with-http_postgres_module \
    && make -j8 \
    && make install \
    && rm -rf /tmp/*

#log
RUN mkdir -p /opt/openresty/nginx/logs \
    /opt/openresty/nginx/conf \
    /var/cache/nginx

COPY nginx.conf /opt/openresty/nginx/conf/nginx.conf

EXPOSE 80

CMD ["/opt/openresty/nginx/sbin/nginx", "-g", "daemon off;"]
