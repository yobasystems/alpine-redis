FROM yobasystems/alpine:3.22.0-amd64

ARG BUILD_DATE
ARG VCS_REF

LABEL maintainer="Dominic Taylor <dominic@yoba.systems>" \
    architecture="amd64/x86_64" \
    redis-version="8.0.2" \
    alpine-version="3.22.0" \
    build="16-Jun-2025" \
    org.opencontainers.image.title="alpine-redis" \
    org.opencontainers.image.description="Redis container image running on Alpine Linux" \
    org.opencontainers.image.authors="Dominic Taylor <dominic@yoba.systems>" \
    org.opencontainers.image.vendor="Yoba Systems" \
    org.opencontainers.image.version="8.0.2" \
    org.opencontainers.image.url="https://hub.docker.com/r/yobasystems/alpine-redis/" \
    org.opencontainers.image.source="https://github.com/yobasystems/alpine-redis" \
    org.opencontainers.image.base.name="docker.io/yobasystems/alpine:3.22.0" \
    org.opencontainers.image.revision=$VCS_REF \
    org.opencontainers.image.created=$BUILD_DATE

RUN addgroup -S redis && adduser -S -G redis redis

RUN apk add --no-cache su-exec tzdata

ENV REDIS_VERSION 8.0.2
ENV REDIS_DOWNLOAD_URL http://download.redis.io/releases/redis-8.0.2.tar.gz
ENV REDIS_DOWNLOAD_SHA caf3c0069f06fc84c5153bd2a348b204c578de80490c73857bee01d9b5d7401f

RUN set -ex; \
	\
	apk add --no-cache --virtual .build-deps \
	coreutils dpkg-dev dpkg gcc jemalloc-dev linux-headers make musl-dev openssl-dev wget ; \
	\
	wget -O redis.tar.gz "$REDIS_DOWNLOAD_URL"; \
	echo "$REDIS_DOWNLOAD_SHA *redis.tar.gz" | sha256sum -c -; \
	mkdir -p /usr/src/redis; \
	tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1; \
	rm redis.tar.gz; \
	grep -E '^ *createBoolConfig[(]"protected-mode",.*, *1 *,.*[)],$' /usr/src/redis/src/config.c; \
	sed -ri 's!^( *createBoolConfig[(]"protected-mode",.*, *)1( *,.*[)],)$!\10\2!' /usr/src/redis/src/config.c; \
	grep -E '^ *createBoolConfig[(]"protected-mode",.*, *0 *,.*[)],$' /usr/src/redis/src/config.c; \
	export BUILD_TLS=yes; \
	make -C /usr/src/redis -j "$(nproc)"; \
	make -C /usr/src/redis install; \
	rm -r /usr/src/redis; \
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add --virtual .redis-rundeps $runDeps; \
	apk del .build-deps; \
	redis-server --version

RUN mkdir /data && chown redis:redis /data
VOLUME /data
WORKDIR /data

COPY files/docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 6379
CMD ["redis-server"]
