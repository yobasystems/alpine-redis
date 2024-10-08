FROM yobasystems/alpine:3.20.2-armhf

ARG BUILD_DATE
ARG VCS_REF

LABEL maintainer="Dominic Taylor <dominic@yoba.systems>" \
    architecture="arm32v7/armhf" \
    redis-version="7.2.5" \
    alpine-version="3.20.2" \
    build="19-Aug-2024" \
    org.opencontainers.image.title="alpine-redis" \
    org.opencontainers.image.description="Redis container image running on Alpine Linux" \
    org.opencontainers.image.authors="Dominic Taylor <dominic@yoba.systems>" \
    org.opencontainers.image.vendor="Yoba Systems" \
    org.opencontainers.image.version="7.2.5" \
    org.opencontainers.image.url="https://hub.docker.com/r/yobasystems/alpine-redis/" \
    org.opencontainers.image.source="https://github.com/yobasystems/alpine-redis" \
    org.opencontainers.image.base.name="docker.io/yobasystems/alpine:3.20.2" \
    org.opencontainers.image.revision=$VCS_REF \
    org.opencontainers.image.created=$BUILD_DATE

RUN addgroup -S redis && adduser -S -G redis redis

RUN apk add --no-cache su-exec tzdata

ENV REDIS_VERSION 7.2.5
ENV REDIS_DOWNLOAD_URL http://download.redis.io/releases/redis-7.2.5.tar.gz
ENV REDIS_DOWNLOAD_SHA 5981179706f8391f03be91d951acafaeda91af7fac56beffb2701963103e423d

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
