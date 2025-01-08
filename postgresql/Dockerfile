# syntax=docker/dockerfile:1.5.1@sha256:d2d74ff22a0e47b21f4bbde337e2ac4cd0a02a2226ef79264878db3dc7e87df8
FROM base

ARG TARGETARCH

EXPOSE 5432

# Platform specific does require arch specific identifier.
RUN --mount=type=cache,id=postgresql-apk-${TARGETARCH},sharing=locked,target=/var/cache/apk \
    apk add \
        postgresql \
        postgresql-client \
    && \
    mkdir -p /var/lib/postgresql/data /etc/postgresql && \
    chown -R postgres:postgres /var/lib/postgresql && \
    chmod 700 /var/lib/postgresql/data && \
    cleanup.sh

ENV \
    PGDATA=/var/lib/postgresql/data

COPY --link rootfs /
