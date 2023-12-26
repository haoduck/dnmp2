ARG ALPINE_VERSION
FROM ${ALPINE_VERSION}

ARG TZ
ARG CONTAINER_PACKAGE_URL

RUN if [ "${CONTAINER_PACKAGE_URL}" != "" ] ; then sed -i "s/dl-cdn.alpinelinux.org/${CONTAINER_PACKAGE_URL}/g" /etc/apk/repositories ; fi

RUN apk update && apk add --no-cache nginx nginx-mod-http-lua certbot certbot-nginx

ENV TZ=${TZ}

RUN set -eux; \
    apk --no-cache add tzdata; \
    cp "/usr/share/zoneinfo/$TZ" /etc/localtime; \
    echo "$TZ" > /etc/timezone


WORKDIR /www

CMD ["nginx", "-g", "daemon off;"]S