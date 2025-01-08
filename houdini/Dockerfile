# syntax=docker/dockerfile:1.5.1
FROM islandora/imagemagick:alpine-3.20.2-imagemagick-7.1.1.36-r0@sha256:a1fa03a18e7e232e380d070d196dc2c0e0a8762dd385640b932e28fcacfd9b05 AS imagemagick
FROM crayfish

ARG TARGETARCH

EXPOSE 8000

WORKDIR /var/www/crayfish/Houdini

# Platform agnostic does not require arch specific identifier.
RUN --mount=type=cache,id=houdini-composer-${TARGETARCH},sharing=locked,target=/root/.composer/cache \
    composer install -d /var/www/crayfish/Houdini && \
    ln -s /var/www/crayfish/Houdini/public /var/www/html && \
    cleanup.sh

# Platform specific does require arch specific identifier.
# Though platform information is included via the FROM imagemagick.
RUN --mount=type=bind,from=imagemagick,source=/packages,target=/packages \
    --mount=type=bind,from=imagemagick,source=/etc/apk/keys,target=/etc/apk/keys \
    apk add /packages/imagemagick-*.apk && \
    addgroup nginx jwt && \
    cleanup.sh

ENV \
    HOUDINI_FCREPO_URL=http://fcrepo:8080/fcrepo/rest \
    HOUDINI_LOG_LEVEL=info

COPY --link rootfs /

RUN chown -R nginx:nginx /var/www
