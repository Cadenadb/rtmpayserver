# ============================================================
# Raptoreum (RTM) Full Node Docker Image
# Based on Ubuntu 22.04 — supports AMD64 and ARM64
# Downloads official binaries from Raptoreum GitHub releases
# ============================================================
FROM ubuntu:22.04

ARG TARGETARCH
ARG RTM_VERSION=2.0.3.01

RUN apt-get update && apt-get install -y \
    wget tar xz-utils curl \
    && rm -rf /var/lib/apt/lists/*

# Download the correct binary based on architecture.
# Release asset naming (v2.0.3.01-mainnet):
#   arm64 → raptoreum-arm64-{VERSION}-mainnet.tar.gz
#   amd64 → raptoreum-ubuntu22-{VERSION}-mainnet-.tar.gz  (note trailing dash)
RUN if [ "$TARGETARCH" = "arm64" ]; then \
      BINARY="raptoreum-arm64-${RTM_VERSION}-mainnet.tar.gz"; \
    else \
      BINARY="raptoreum-ubuntu22-${RTM_VERSION}-mainnet-.tar.gz"; \
    fi && \
    BASE_URL="https://github.com/Raptor3um/raptoreum/releases/download/${RTM_VERSION}-mainnet" && \
    mkdir -p /opt/raptoreum && \
    cd /opt/raptoreum && \
    wget -q "${BASE_URL}/${BINARY}" && \
    tar -xzf "${BINARY}" && \
    find . -name "raptoreumd" -exec cp {} /usr/local/bin/ \; && \
    find . -name "raptoreum-cli" -exec cp {} /usr/local/bin/ \; && \
    chmod +x /usr/local/bin/raptoreum* && \
    rm -rf /opt/raptoreum

RUN mkdir -p /root/.raptoreumcore

# Default raptoreum.conf — override via volume mount or env vars
RUN printf "# Raptoreum Node Configuration\nserver=1\ntxindex=1\nlisten=1\nmaxconnections=40\ndbcache=512\nrpcbind=0.0.0.0\nrpcallowip=0.0.0.0/0\nprinttoconsole=1\n" \
    > /root/.raptoreumcore/raptoreum.conf

EXPOSE 10226 9998

VOLUME ["/root/.raptoreumcore"]

ENTRYPOINT ["raptoreumd"]
CMD ["-conf=/root/.raptoreumcore/raptoreum.conf"]
