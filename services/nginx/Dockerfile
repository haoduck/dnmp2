ARG NGINX_VERSION

FROM ${NGINX_VERSION} AS builder

# 安装构建工具和依赖
RUN apk update && \
    apk add --no-cache \
        build-base \
        git

# 下载并编译 ngx_brotli 模块
# RUN git clone https://github.com/google/ngx_brotli.git && \
#     cd ngx_brotli && \
#     git submodule update --init && \
#     ./autogen.sh && \
#     ./configure && \
#     make

# 下载并编译 nginx-dav-ext-module 模块
RUN git clone https://github.com/arut/nginx-dav-ext-module.git

# 下载并编译 ngx-fancyindex 模块
RUN git clone --depth=1 https://github.com/aperezdc/ngx-fancyindex.git

# 下载并编译 Nginx Lua 模块
RUN git clone https://github.com/openresty/lua-nginx-module.git

# 构建Nginx，添加指定的模块
RUN cd /usr/src/nginx && \
    ./configure \
        --with-http_realip_module \
        --with-http_ssl_module \
        --with-openssl \
        --add-module=/usr/src/nginx-dav-ext-module \
        # --add-module=/usr/src/ngx_brotli \
        --add-module=/usr/src/ngx-fancyindex \
        --add-module=/usr/src/lua-nginx-module \
        --with-http_lua_module && \
    make

FROM ${NGINX_VERSION}

# 复制构建阶段编译得到的二进制文件
COPY --from=builder /usr/src/nginx/objs/nginx /usr/sbin/nginx

ARG TZ
ARG NGINX_VERSION
ARG CONTAINER_PACKAGE_URL
ARG NGINX_INSTALL_APPS

ENV INSTALL_APPS=",${NGINX_INSTALL_APPS},"

RUN if [ "${CONTAINER_PACKAGE_URL}" != "" ] ; then sed -i "s/dl-cdn.alpinelinux.org/${CONTAINER_PACKAGE_URL}/g" /etc/apk/repositories ; fi

RUN if [ -z "${INSTALL_APPS##*,certbot,*}" ]; then \
        echo "---------- Install certbot ----------"; \
        apk add --no-cache certbot; \
    fi

WORKDIR /www
