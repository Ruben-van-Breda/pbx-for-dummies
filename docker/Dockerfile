# syntax=docker/dockerfile:1.4
FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# --- Install Asterisk ---
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        asterisk \
        asterisk-core-sounds-en-wav \
        asterisk-moh-opsound-wav \
        ca-certificates curl && \
    rm -rf /var/lib/apt/lists/*

# --- Prepare directories and permissions ---
RUN usermod -d /var/lib/asterisk asterisk && \
    mkdir -p /var/run/asterisk && \
    chown -R asterisk:asterisk \
        /etc/asterisk \
        /var/lib/asterisk \
        /var/spool/asterisk \
        /var/log/asterisk \
        /var/run/asterisk

# --- Copy config files from build context ---
COPY asterisk/asterisk.conf /etc/asterisk/asterisk.conf
COPY asterisk/pjsip.conf /etc/asterisk/pjsip.conf
COPY asterisk/extensions.conf /etc/asterisk/extensions.conf
COPY asterisk/modules.conf /etc/asterisk/modules.conf

WORKDIR /var/lib/asterisk

# --- Network ports ---
EXPOSE 5060/udp 5061/tcp 10000-20000/udp

# --- Healthcheck ---
HEALTHCHECK --interval=30s --timeout=5s --start-period=30s --retries=3 \
  CMD asterisk -rx "core show uptime" || exit 1

# --- Run Asterisk in foreground ---
USER asterisk
ENTRYPOINT ["/usr/sbin/asterisk"]
CMD ["-f", "-U", "asterisk", "-G", "asterisk"]