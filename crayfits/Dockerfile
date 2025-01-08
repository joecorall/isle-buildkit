# syntax=docker/dockerfile:1.5.1@sha256:d2d74ff22a0e47b21f4bbde337e2ac4cd0a02a2226ef79264878db3dc7e87df8
FROM crayfish

ARG TARGETARCH

EXPOSE 8000

WORKDIR /var/www/crayfish/CrayFits/

# Required at the moment as the log location is hard-coded in crayfits.
RUN mkdir -p /var/log/islandora && \
    touch /var/log/islandora/fits.log && \
    chown -R nginx:nginx /var/log/islandora && \
    cleanup.sh

# Platform agnostic does not require arch specific identifier.
RUN --mount=type=cache,id=crayfits-composer-${TARGETARCH},sharing=locked,target=/root/.composer/cache \
    composer install -d /var/www/crayfish/CrayFits && \
    ln -s /var/www/crayfish/CrayFits/public /var/www/html && \
    cleanup.sh

ENV \
    CRAYFITS_LOG_LEVEL=info \
    CRAYFITS_WEBSERVICE_URI=fits:8080/fits/examine

COPY --link rootfs /

RUN chown -R nginx:nginx /var/www
