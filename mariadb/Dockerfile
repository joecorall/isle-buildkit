# syntax=docker/dockerfile:1.5.1@sha256:d2d74ff22a0e47b21f4bbde337e2ac4cd0a02a2226ef79264878db3dc7e87df8
FROM base

ARG TARGETARCH

EXPOSE 3306

# Platform specific does require arch specific identifier.
RUN --mount=type=cache,id=mariadb-apk-${TARGETARCH},sharing=locked,target=/var/cache/apk \
    apk add \
        mariadb \
        mysql-client \
        mariadb-server-utils \
    && \
    mkdir -p \
        /var/lib/mysql \
        /var/lib/mysql-files \
    && \
    chown -R mysql:mysql \
        /var/lib/mysql \
        /var/lib/mysql-files \
    && \
    cleanup.sh

# Installation sometimes needs more than the default 30 seconds defined in the
# base image. Set to 10 minutes just incase it ran on very old or overallocated
# hardware.
ENV S6_CMD_WAIT_FOR_SERVICES_MAXTIME=600000

ENV \ 
    # Default Mariadb value of 16 MB (bytes)
    MYSQL_MAX_ALLOWED_PACKET=16777216 \
    MYSQL_TRANSACTION_ISOLATION=READ-COMMITTED

COPY --link rootfs /
