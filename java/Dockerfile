# syntax=docker/dockerfile:1.12.1@sha256:93bfd3b68c109427185cd78b4779fc82b484b0b7618e36d0f104d4d801e66d25
FROM base

ARG TARGETARCH

# Install packages and tools required by all downstream images.
#
# Platform specific does require arch specific identifier.
RUN --mount=type=cache,id=java-apk-${TARGETARCH},sharing=locked,target=/var/cache/apk \
    apk add \
        openjdk17 \
        maven \
    && \
    cleanup.sh

ENV JAVA_HOME=/usr/lib/jvm/default-jvm
