# syntax=docker/dockerfile:1.12.1@sha256:93bfd3b68c109427185cd78b4779fc82b484b0b7618e36d0f104d4d801e66d25
FROM crayfish

ARG TARGETARCH

EXPOSE 8000

WORKDIR /var/www/crayfish/Homarus

# Platform agnostic does not require arch specific identifier.
RUN --mount=type=cache,id=homarus-composer-${TARGETARCH},sharing=locked,target=/root/.composer/cache \
    composer install -d /var/www/crayfish/Homarus && \
    ln -s /var/www/crayfish/Homarus/public /var/www/html && \
    cleanup.sh

# Platform specific does require arch specific identifier.
RUN --mount=type=cache,id=homarus-apk-${TARGETARCH},sharing=locked,target=/var/cache/apk \
    apk add ffmpeg && \
    addgroup nginx jwt && \
    cleanup.sh

ENV \
    HOMARUS_APIX_MIDDLEWARE_ENABLED=false \
    HOMARUS_FCREPO_URL=http://fcrepo:8080/fcrepo/rest \
    HOMARUS_LOG_LEVEL=info

COPY --link rootfs /

RUN chown -R nginx:nginx /var/www
