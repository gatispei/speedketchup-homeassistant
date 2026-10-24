# https://contributors.home-assistant.io/apps/guides/dockerfile/
ARG BUILD_FROM=ghcr.io/home-assistant/base:3.24
ARG SPEEDKETCHUP_VERSION=0.5.0

# the official image is FROM scratch with just the static binary, BuildKit
# picks the platform matching the one being built
FROM docker.io/gatispei/speedketchup:${SPEEDKETCHUP_VERSION} AS speedketchup

FROM ${BUILD_FROM}

COPY --from=speedketchup /speedketchup /usr/bin/speedketchup
COPY rootfs /

# the Supervisor restarts the app when this fails, every connection logs a
# line or two, so not too often
HEALTHCHECK --interval=5m --timeout=10s --start-period=1m --start-interval=5s --retries=3 \
    CMD curl -fsS -o /dev/null http://127.0.0.1:8288/metrics || exit 1

LABEL \
    org.opencontainers.image.title="Home Assistant App: SpeedKetchup" \
    org.opencontainers.image.description="Periodic internet speed tests with charts, a full history and MQTT sensors" \
    org.opencontainers.image.source="https://github.com/gatispei/speedketchup-homeassistant" \
    org.opencontainers.image.licenses="GPL-2.0-only"
