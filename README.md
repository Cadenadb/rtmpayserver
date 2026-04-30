# RTMPayServer

**Self-hosted payment processor with native Raptoreum (RTM), Bitcoin, and Lightning Network support.**

Built on top of [BTCPayServer](https://btcpayserver.org/) — free, open-source, and no third-party involved.

---

## What is this?

RTMPayServer is a fork and extension of BTCPayServer that adds native support for **Raptoreum (RTM)** alongside Bitcoin and Lightning Network. This means you can accept payments in RTM, BTC, and BTC-Lightning from a single self-hosted interface, with full custody of your funds.

## Supported Currencies

| Currency | Symbol | Status |
|---|---|---|
| Raptoreum | RTM | 🔧 In development |
| Bitcoin | BTC | ✅ Supported |
| Bitcoin Lightning | BTC-LN | ✅ Supported |

## Architecture

This project is composed of the following repositories:

| Repository | Purpose |
|---|---|
| [Cadenadb/rtmpayserver](https://github.com/Cadenadb/rtmpayserver) | Main repo: Docker Compose, docs, deployment |
| [Cadenadb/btcpayserver](https://github.com/Cadenadb/btcpayserver) | BTCPayServer fork with RTM UI support |
| [Cadenadb/NBXplorer](https://github.com/Cadenadb/NBXplorer) | NBXplorer fork with RTM network registration |
| [Cadenadb/btcpayserver-docker](https://github.com/Cadenadb/btcpayserver-docker) | Docker Compose configurations |

## Requirements

- Docker + Docker Compose (or Portainer)
- Server: 4 CPU / 8GB RAM / 50GB storage minimum
- A running Raptoreum node (or use the included Docker setup)

## Quick Start (Development)

```bash
git clone https://github.com/Cadenadb/rtmpayserver.git
cd rtmpayserver
cp .env.example .env
# Edit .env with your settings
docker compose up -d
```

## Status

🚧 **This project is under active development.** Not ready for production use.

## Contributing

Pull requests are welcome. Please open an issue first to discuss what you would like to change.

## License

MIT — same as BTCPayServer.
