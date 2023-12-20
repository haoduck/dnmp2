ARG NGINX_VERSION
ARG TZ
ARG CONTAINER_PACKAGE_URL
ARG NGINX_INSTALL_APPS

# 使用官方的Alpine Linux镜像作为基础镜像
FROM alpine:latest as builder

# 安装编译依赖
RUN apk --no-cache add \
    build-base \
    linux-headers \
    pcre-dev \
    openssl-dev \
    zlib-dev \
    wget \
    git

# 下载 Nginx 源代码
WORKDIR /tmp/nginx-source
RUN wget http://nginx.org/download/nginx-1.21.1.tar.gz \
    && tar -zxf nginx-1.21.1.tar.gz

# 下载 ngx_http_lua_module 源代码
WORKDIR /tmp/lua-module
RUN git clone https://github.com/openresty/lua-nginx-module.git \
    && mv lua-nginx-module ngx_http_lua_module

# 编译 Nginx 并加载 ngx_http_lua_module 模块
WORKDIR /tmp/nginx-source/nginx-1.21.1
RUN ./configure --with-compat --add-dynamic-module=../lua-module \
    && make modules

# 构建最终的镜像
FROM nginx:1.25.3

# 复制编译得到的 ngx_http_lua_module 模块到正确的位置
COPY --from=builder /tmp/nginx-source/nginx-1.21.1/objs/ngx_http_lua_module.so /usr/lib/nginx/modules/ngx_http_lua_module.so

# ENV INSTALL_APPS=",${NGINX_INSTALL_APPS},"

# RUN if [ "${CONTAINER_PACKAGE_URL}" != "" ] ; then sed -i "s/dl-cdn.alpinelinux.org/${CONTAINER_PACKAGE_URL}/g" /etc/apk/repositories ; fi

# RUN apk update && apk add --no-cache nginx nginx-mod-http-lua

# RUN if [ -z "${INSTALL_APPS##*,certbot,*}" ]; then \
#         echo "---------- Install certbot ----------"; \
#         apk add --no-cache certbot; \
#     fi

WORKDIR /www