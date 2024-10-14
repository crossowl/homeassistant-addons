ARG BUILD_FROM="ghcr.io/home-assistant/amd64-base:3.18"
ARG HASS_VERSION=""
#ARG TARGETPLATFORM="amd64"

# ----------------------------------------------------------------------------
FROM golang:1.22-alpine as builder

RUN <<EOF
  apk update
  apk upgrade
EOF

COPY app_src /app/src

WORKDIR /app/src

RUN go get -u ./...

RUN go build -o /app/build/addon /app/src/cmd/addon/main.go

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

RUN <<EOF
  apk update
  apk upgrade
  apk cache clean
EOF

COPY rootfs /

COPY --from=builder --chmod=a+x /app/build/addon /app/bin/addon

VOLUME [ "/data" ]
EXPOSE 8080/tcp

ENTRYPOINT [ "/app/bin/addon", "--data", "/data" ]
