# Shelly Manager - All-in-One Image for Unraid
# Combines API and Web UI with nginx reverse proxy

FROM ghcr.io/jfmlima/shelly-manager-api:latest AS api-source
FROM ghcr.io/jfmlima/shelly-manager-web:latest AS web-source

FROM lscr.io/linuxserver/baseimage-alpine:3.20

LABEL maintainer="jfmlima" \
      org.opencontainers.image.title="Shelly Manager" \
      org.opencontainers.image.description="Local management for Shelly IoT devices without cloud connectivity" \
      org.opencontainers.image.vendor="Shelly Manager" \
      org.opencontainers.image.source="https://github.com/jfmlima/unraid-templates" \
      org.opencontainers.image.licenses="MIT"

# Install dependencies
RUN apk add --no-cache \
        python3 \
        py3-pip \
        curl \
        nginx

# Copy API from source
COPY --from=api-source /app/packages/core /app/packages/core
COPY --from=api-source /app/packages/api /app/packages/api

# Copy Web UI from source
COPY --from=web-source /usr/share/nginx/html /var/www/html/

# Setup Python environment
WORKDIR /app
RUN python3 -m venv .venv \
    && . .venv/bin/activate \
    && pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir /app/packages/core /app/packages/api \
    && rm -rf /root/.cache/pip

ENV PATH="/app/.venv/bin:$PATH" \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    SHELLY_DATA_DIR=/config \
    SHELLY_FIRMWARE_DIR=/config/firmware

# Copy rootfs (s6 services, nginx config)
COPY rootfs /

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=10s --start-period=60s --retries=3 \
    CMD sh -c 'curl -f http://localhost:${PORT:-8080}/health || exit 1'
