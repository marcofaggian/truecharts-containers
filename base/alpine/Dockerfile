FROM alpine:3.17.2@sha256:ff6bdca1701f3a8a67e328815ff2346b0e4067d32ec36b7992c1fdc001dc8517

ARG CONTAINER_NAME
ARG CONTAINER_VER

ENV XDG_CONFIG_HOME=/config
ENV UMASK=0002

USER root

SHELL ["/bin/sh", "-o", "pipefail", "-c"]

WORKDIR /app

# hadolint ignore=DL3018
RUN \
  apk update && apk add --no-cache ca-certificates \
  && addgroup apps -g 568 \
  && adduser apps -u 568 -g 568 -D -S -H \
  && mkdir -p /config \
  && chown -R apps:apps /config \
  && chmod -R 775 /config \
  && chown -R apps:apps /app \
  && chmod -R 775 /app \
  && update-ca-certificates

VOLUME [ "/config" ]

USER apps

COPY ./base/ubuntu/entrypoint.sh /entrypoint.sh
COPY ./base/ubuntu/shim /etc/profile.d

ENTRYPOINT ["/bin/ash", "--"]

# iX NFS4_XDR WINACL ripped from toolkit ripped from TrueNAS SCALE 22.02.04
COPY ./base/ubuntu/nfs4xdr/* /usr/sbin/

LABEL org.opencontainers.image.licenses="BSD-3-Clause"
LABEL org.opencontainers.image.title="${CONTAINER_NAME}"
LABEL "maintainer"="TrueCharts <info@truecharts.org>"
LABEL "org.opencontainers.image.source"="https://github.com/truecharts/containers"
LABEL org.opencontainers.image.version="${CONTAINER_VER}"
LABEL org.opencontainers.image.description="Container for ${CONTAINER_NAME} by TrueCharts"
LABEL org.opencontainers.image.authors="TrueCharts"
