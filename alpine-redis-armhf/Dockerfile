FROM yobasystems/alpine:3.23.0-armhf

ARG BUILD_DATE
ARG VCS_REF

LABEL maintainer="Dominic Taylor <dominic@yoba.systems>" \
	architecture="arm32v7/armhf" \
	redis-version="8.4.0" \
	alpine-version="3.23.0" \
	build="25-Nov-2025" \
	org.opencontainers.image.title="alpine-redis" \
	org.opencontainers.image.description="Redis container image running on Alpine Linux" \
	org.opencontainers.image.authors="Dominic Taylor <dominic@yoba.systems>" \
	org.opencontainers.image.vendor="Yoba Systems" \
	org.opencontainers.image.version="8.4.0" \
	org.opencontainers.image.url="https://hub.docker.com/r/yobasystems/alpine-redis/" \
	org.opencontainers.image.source="https://github.com/yobasystems/alpine-redis" \
	org.opencontainers.image.base.name="docker.io/yobasystems/alpine:3.23.0" \
	org.opencontainers.image.revision=$VCS_REF \
	org.opencontainers.image.created=$BUILD_DATE

RUN addgroup -S redis && adduser -S -G redis redis

RUN apk add --no-cache su-exec tzdata

ENV REDIS_VERSION=8.4.0
ENV REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-8.4.0.tar.gz
ENV REDIS_DOWNLOAD_SHA=ca909aa15252f2ecb3a048cd086469827d636bf8334f50bb94d03fba4bfc56e8

RUN set -ex; \
	apk add --no-cache --virtual .build-deps \
	coreutils gcc g++ jemalloc-dev linux-headers make musl-dev openssl-dev wget pkgconf ; \
	wget -O redis.tar.gz "$REDIS_DOWNLOAD_URL"; \
	echo "VERIFYING CHECKSUM:"; \
	echo "$REDIS_DOWNLOAD_SHA *redis.tar.gz" | sha256sum -c -; \
	echo "CREATING DIRECTORY /usr/src/redis:"; \
	mkdir -p /usr/src/redis; \
	echo "EXTRACTING TARBALL:"; \
	tar -tvzf redis.tar.gz > /tmp/tar_contents.txt; \
	echo "TARBALL CONTENTS (first 20 lines of /tmp/tar_contents.txt):"; \
	head -n 20 /tmp/tar_contents.txt; \
	tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1; \
	echo "LISTING /usr/src/redis AFTER EXTRACTION:"; \
	ls -lR /usr/src/redis; \
	echo "CHECKING SPECIFICALLY FOR /usr/src/redis/src/config.c:"; \
	ls -l /usr/src/redis/src/config.c; \
	echo "ATTEMPTING GREP (if the file exists, this should find the line or exit 1, not 2):"; \
	grep -E '^ *createBoolConfig[(]\"protected-mode\",.*, *1 *,.*[)],$' /usr/src/redis/src/config.c || echo "GREP FAILED OR NO MATCH (exit code $?)" ; \
	echo "ATTEMPTING SED (original line):"; \
	sed -ri 's!^( *createBoolConfig[(]\"protected-mode\",.*, *)1( *,.*[)],)$!\10\2!' /usr/src/redis/src/config.c; \
	echo "VERIFYING SED (should now be 0):"; \
	grep -E '^ *createBoolConfig[(]\"protected-mode\",.*, *0 *,.*[)],$' /usr/src/redis/src/config.c || echo "GREP AFTER SED FAILED OR NO MATCH (exit code $?)" ; \
	export BUILD_TLS=yes; \
	make -C /usr/src/redis MALLOC=jemalloc -j "$(nproc)"; \
	make -C /usr/src/redis MALLOC=jemalloc install; \
	rm -r /usr/src/redis; \
	rm redis.tar.gz; \
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
