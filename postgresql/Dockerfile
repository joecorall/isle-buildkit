# syntax=docker/dockerfile:1.12.1@sha256:93bfd3b68c109427185cd78b4779fc82b484b0b7618e36d0f104d4d801e66d25
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
