ARG NGINX_VERSION

FROM nginx:1.25.3

# 安装依赖
RUN apt-get update \
    && apt-get install -y libnginx-mod-http-lua certbot

ARG TZ
ARG CONTAINER_PACKAGE_URL
ARG NGINX_INSTALL_APPS

# ENV INSTALL_APPS=",${NGINX_INSTALL_APPS},"

# RUN if [ "${CONTAINER_PACKAGE_URL}" != "" ] ; then sed -i "s/dl-cdn.alpinelinux.org/${CONTAINER_PACKAGE_URL}/g" /etc/apk/repositories ; fi

# RUN apk update && apk add --no-cache nginx nginx-mod-http-lua

# RUN if [ -z "${INSTALL_APPS##*,certbot,*}" ]; then \
#         echo "---------- Install certbot ----------"; \
#         apk add --no-cache certbot; \
#     fi

WORKDIR /www