# ============================================================
# Raptoreum (RTM) Full Node Docker Image
# Based on Ubuntu 22.04 - supports AMD64 and ARM64
# Downloads official binaries from Raptoreum GitHub releases
# ============================================================
FROM ubuntu:22.04

ARG TARGETARCH
ARG RTM_VERSION=2.0.3.01

RUN apt-get update && apt-get install -y \
    wget tar xz-utils curl \
    && rm -rf /var/lib/apt/lists/*

# Download the correct binary based on architecture
RUN if [ "$TARGETARCH" = "arm64" ]; then \
      BINARY="raptoreum-arm64-${RTM_VERSION}-mainnet.tar.gz"; \
    else \
      BINARY="raptoreum-x86_64-${RTM_VERSION}-mainnet.tar.gz"; \
    fi && \
    mkdir -p /opt/raptoreum && \
    cd /opt/raptoreum && \
    wget -q "https://github.com/Raptor3um/raptoreum/releases/download/${RTM_VERSION}-mainnet/${BINARY}" && \
    tar -xzf "${BINARY}" && \
    find . -name "raptoreumd" -exec cp {} /usr/local/bin/ \; && \
    find . -name "raptoreum-cli" -exec cp {} /usr/local/bin/ \; && \
    chmod +x /usr/local/bin/raptoreum* && \
    rm -rf /opt/raptoreum

RUN mkdir -p /root/.raptoreumcore

# Create default raptoreum.conf
RUN echo "# Raptoreum Node Configuration\n\
server=1\n\
txindex=1\n\
listen=1\n\
maxconnections=40\n\
dbcache=512\n\
rpcbind=0.0.0.0\n\
rpcallowip=0.0.0.0/0\n\
printtoconsole=1" > /root/.raptoreumcore/raptoreum.conf

EXPOSE 10226 9998

VOLUME ["/root/.raptoreumcore"]

ENTRYPOINT ["raptoreumd"]
CMD ["-conf=/root/.raptoreumcore/raptoreum.conf"]