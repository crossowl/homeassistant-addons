ARG BUILD_FROM
ARG HASS_VERSION
ARG TARGETPLATFORM

# ----------------------------------------------------------------------------
FROM golang:1.22-alpine as builder

COPY app_src /app/src

WORKDIR /app/src

RUN go

# ----------------------------------------------------------------------------
FROM $BUILD_FROM

LABEL \
    org.opencontainers.image.title="Example Golang Addon" \
    org.opencontainers.image.description="Example Addon Containerised Addon for Home Assistant" \
    org.opencontainers.image.source="https://github.com/crossowl/homeassistant-addons/tree/main/co-go-example" \
    org.opencontainers.image.authors="Chris Howell (Cross Owl)" \
    org.opencontainers.image.documentation="https://github.com/crossowl/homeassistant-addons/tree/main/co-go-example/DOCS.md" \
    org.opencontainers.image.licenses="Apache License 2.0" \
    io.hass.version="${HASS_VERSION}" \
    io.hass.type="addon" \
    io.hass.name="Example Golang Addon" \
    io.hass.description="Example Addon Containerised Addon for Home Assistant" \
    io.hass.platform="${TARGETPLATFORM}"

COPY rootfs /

COPY --from=builder --chmod=a+x /app/build/addon /app/bin/addon

VOLUME [ "/data" ]
EXPOSE 8080/tcp

ENTRYPOINT [ "/app/bin/addon", "--data", "/data" ]
