<div align="center">

<img src="https://raw.githubusercontent.com/btcpayserver/btcpayserver/master/BTCPayServer/wwwroot/img/btcpay-logo.svg" alt="RTMPayServer Logo" width="120"/>

# RTMPayServer

### Self-Hosted Payment Gateway for Bitcoin, Lightning Network & Raptoreum (RTM)

**Own your payments. No middlemen. No fees. No censorship.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Docker](https://img.shields.io/badge/Docker-ready-blue?logo=docker)](https://hub.docker.com)
[![Bitcoin](https://img.shields.io/badge/Bitcoin-BTC-orange?logo=bitcoin)](https://bitcoin.org)
[![Lightning](https://img.shields.io/badge/Lightning-⚡-yellow)](https://lightning.network)
[![Raptoreum](https://img.shields.io/badge/Raptoreum-RTM-blue)](https://raptoreum.com)
[![Telegram](https://img.shields.io/badge/Contact-@cadenadb-blue?logo=telegram)](https://t.me/cadenadb)

---

*Built on [BTCPayServer](https://btcpayserver.org/) — extended with native Raptoreum support*

</div>

---

## 🌟 What is RTMPayServer?

**RTMPayServer** is a free, open-source, self-hosted payment processor that lets you accept **Bitcoin (BTC)**, **Bitcoin Lightning (⚡)**, and **Raptoreum (RTM)** — all from a single interface you control 100%.

No account registrations with a payment provider. No percentage cuts on your sales. No third party can freeze your funds, block your transactions, or access your financial data. **You are the bank.**

This project was born from the Raptoreum community's need for a professional payment infrastructure. BTCPayServer is the gold standard for self-hosted Bitcoin payments — so we forked it, added native RTM support at the network level, and packaged everything into a ready-to-deploy Docker stack.

---

## 💡 Why does this matter?

| Problem | RTMPayServer Solution |
|---|---|
| PayPal/Stripe take 2-4% per transaction | **Zero platform fees** — only blockchain network fees |
| Payment processors can freeze your account | **Nobody can freeze your funds** — you hold the keys |
| Centralized exchanges know your customers | **Full privacy** — no KYC, no data collection |
| No existing gateway accepts RTM | **Native RTM support** built from the ground up |
| Lightning is complex to self-host | **One `docker compose up`** and Lightning is ready |
| Hosted solutions are black boxes | **100% open source** — read every line of code |

---

## ✅ Supported Payment Methods

| Currency | Symbol | Type | Status |
|---|---|---|---|
| Bitcoin | BTC | On-chain | ✅ Ready (requires ~2-5 day initial sync) |
| Bitcoin Lightning | BTC ⚡ | Instant | ✅ Ready (wallet init required) |
| Raptoreum | RTM | On-chain | ✅ Ready |

---

## 🏗️ Project Architecture

RTMPayServer is a collection of forked and extended open-source components, all published under the `cadenadb` organization on GitHub:

```
┌─────────────────────────────────────────────────────────┐
│                 https://your-domain.com                  │
│              (nginx reverse proxy + SSL)                  │
└───────────────────────┬─────────────────────────────────┘
                        │
              ┌─────────▼──────────┐
              │    BTCPayServer     │  ← Custom fork with RTM plugin
              │  (rtm + btc + ln)  │    github.com/cadenadb/btcpayserver
              └──┬──────┬──────┬───┘
                 │      │      │
        ┌────────▼─┐ ┌──▼───┐ ┌▼────────┐
        │NBXplorer │ │ LND  │ │NBXplorer│
        │  (RTM)   │ │  ⚡  │ │  (BTC)  │
        └────┬─────┘ └──┬───┘ └────┬────┘
             │          │          │
    ┌────────▼─┐   ┌────▼────┐ ┌──▼──────┐
    │Raptoreum │   │Bitcoin  │ │Bitcoin  │
    │   Node   │   │  Core   │ │  Core   │
    │(external │   │(pruned) │ │(pruned) │
    │ or local)│   └─────────┘ └─────────┘
    └──────────┘
             │
    ┌────────▼─────────────────┐
    │       PostgreSQL         │
    │  (shared database for    │
    │   all NBXplorer chains)  │
    └──────────────────────────┘
```

### Repositories

| Repo | Purpose | Image |
|---|---|---|
| [`cadenadb/rtmpayserver`](https://github.com/cadenadb/rtmpayserver) | **This repo** — Docker Compose, deployment docs | — |
| [`cadenadb/btcpayserver`](https://github.com/cadenadb/btcpayserver) | BTCPayServer fork with native RTM plugin | `ghcr.io/cadenadb/btcpayserver-rtm:latest` |
| [`cadenadb/NBXplorer`](https://github.com/cadenadb/NBXplorer) | NBXplorer fork with Raptoreum network support | `ghcr.io/cadenadb/nbxplorer-rtm:latest` |
| [`cadenadb/raptoreumd`](https://github.com/cadenadb/rtmpayserver) | Raptoreum full node Docker image (ARM64 + amd64) | `ghcr.io/cadenadb/raptoreumd:latest` |

All Docker images are **multi-architecture** (AMD64 + ARM64) and publicly available on GitHub Container Registry.

---

## 🖥️ Server Requirements

| Component | Minimum | Recommended |
|---|---|---|
| CPU | 2 cores | 4 cores |
| RAM | 4 GB | 8–16 GB |
| Disk | 60 GB | 150 GB |
| OS | Ubuntu 20.04+ | Ubuntu 22.04 LTS |
| Docker | 24.0+ | latest |
| Network | 10 Mbps | 100 Mbps |

> **Cloud options that work well:** Oracle Cloud Free Tier (4 CPU / 24 GB ARM64), Hetzner CAX21, DigitalOcean Droplets, any VPS with Docker support.

> **Disk breakdown:** Bitcoin blockchain pruned to 20 GB + Raptoreum ~15 GB + system ~10 GB = ~45 GB total minimum.

---

## 🚀 Quick Start

### Step 1 — Clone this repository

```bash
git clone https://github.com/cadenadb/rtmpayserver.git
cd rtmpayserver
```

### Step 2 — Configure your environment

```bash
cp .env.example .env
nano .env   # or: vim .env
```

Fill in these required values:

```env
# Strong random password for the database
POSTGRES_PASSWORD=your_very_strong_password_here

# Your public domain name (no https://, no trailing slash)
BTCPAY_HOST=pay.yourdomain.com

# Raptoreum node RPC credentials
RTM_RPC_USER=your_rtm_rpc_user
RTM_RPC_PASSWORD=your_rtm_rpc_password
```

### Step 3 — Start the stack

```bash
docker compose up -d
```

That's it. Docker will pull all images and start the services. Check the status with:

```bash
docker compose ps
docker compose logs -f rtmpay-btcpayserver
```

### Step 4 — Set up your domain & SSL

Point your domain's DNS A record to your server's IP. Then configure your web server to proxy port 23001 to your domain with SSL. See the [SSL Setup Guide](#-ssl-setup-optional-but-recommended) section below.

### Step 5 — Create your BTCPayServer account

Open `https://your-domain.com` in your browser. The **first user to register becomes the administrator**.

Create your store and start accepting payments!

---

## 🔧 Detailed Configuration

### Raptoreum Node Options

**Option A — Use an external Raptoreum node** *(recommended if you already have one)*

Set `RTM_NODE_HOST` in your `.env` to the IP or hostname of your existing node:

```env
RTM_NODE_HOST=10.0.0.5
RTM_RPC_PORT=10225
RTM_P2P_PORT=10226
```

**Option B — Run a local Raptoreum node** *(uncomment in `docker-compose.yml`)*

Uncomment the `rtmpay-raptoreumd` service block in `docker-compose.yml`. The Raptoreum blockchain is approximately 10–15 GB and will sync automatically.

```yaml
  rtmpay-raptoreumd:
    image: ghcr.io/cadenadb/raptoreumd:latest
    # ... (see docker-compose.yml for full config)
```

### Bitcoin Node

Bitcoin Core is configured with **pruning enabled** (default: 20 GB). This means:
- The node downloads the entire Bitcoin blockchain during initial sync (~600 GB downloaded, pruned to 20 GB stored)
- Initial sync takes **2–5 days** depending on your internet connection and hardware
- After sync, BTCPayServer and Lightning will be fully operational for BTC

The sync happens automatically in the background. RTM payments work immediately — you don't need to wait for Bitcoin to sync.

### Lightning Network (LND)

Lightning requires a **one-time wallet initialization** on first boot. Follow these steps carefully — skipping or doing them out of order will cause LND to enter a crash loop.

#### Step 1 — Set your wallet password before starting

In your `.env` file, set a strong password for `LND_WALLET_PASSWORD` **before** running `docker compose up` for the first time:

```env
LND_WALLET_PASSWORD=your_strong_password_here
```

> ⚠️ **Critical:** This password encrypts your Lightning wallet on disk. LND needs it every time it starts to unlock the wallet automatically. If the container restarts without this variable — or with a different value — LND will fail to unlock and enter a crash loop with the error `unable to unlock macaroons: invalid password`.

#### Step 2 — Start the stack and wait for LND to be ready

```bash
docker compose up -d
sleep 30
docker compose logs rtmpay-lnd | tail -20
```

Look for this line — it means LND is waiting for wallet creation:

```
[INF] LTND: Waiting for wallet encryption password
```

#### Step 3 — Create the wallet via LND REST API

Run this from inside any container on the same Docker network, or from the host if port 8080 is exposed:

```bash
# Generate a seed phrase
SEED=$(curl -sk https://localhost:8080/v1/genseed | python3 -c "import json,sys; print(' '.join(json.load(sys.stdin)['cipher_seed_mnemonic']))")
echo "YOUR SEED (save this NOW): $SEED"

# Create the wallet with your password
PASS_B64=$(echo -n "your_strong_password_here" | base64)
MNEMONIC_JSON=$(echo "$SEED" | python3 -c "import sys,json; print(json.dumps(sys.stdin.read().split()))")

curl -sk -X POST https://localhost:8080/v1/initwallet   -H "Content-Type: application/json"   -d "{"wallet_password": "$PASS_B64", "cipher_seed_mnemonic": $MNEMONIC_JSON, "stateless_init": false}"
```

> ⚠️ **Save your seed phrase immediately.** Write it down on paper and store it in a safe place. This 24-word phrase is the only way to recover your Lightning funds if you lose access to your server. It cannot be retrieved later.

The password in the `curl` command must match `LND_WALLET_PASSWORD` in your `.env` exactly.

#### Step 4 — Verify Lightning is running

```bash
docker compose logs rtmpay-lnd | grep -E "INF|ERR" | tail -10
```

You should see lines like `[INF] LNWL: Opened wallet` and `[INF] DISC: Attempting to bootstrap` — these confirm the wallet is unlocked and LND is running.

#### Step 5 — Connect BTCPayServer to your Lightning node

The `docker-compose.yml` already configures BTCPayServer to connect to LND automatically via:

```
type=lnd-rest;server=https://rtmpay-lnd:8080;macaroonfilepath=/lnd/data/chain/bitcoin/mainnet/admin.macaroon;allowinsecure=true
```

No manual configuration in the dashboard is needed.

#### What happens on future restarts

After the wallet is created, LND uses `LND_WALLET_PASSWORD` from your `.env` to unlock automatically on every startup. As long as that variable is set correctly, no manual intervention is needed.

---

#### ⚠️ LND Crash Loop — How to diagnose

If LND is stuck in a restart loop, check its logs:

```bash
docker compose logs rtmpay-lnd | tail -20
```

| Error message | Cause | Fix |
|---|---|---|
| `unable to unlock macaroons: invalid password` | `LND_WALLET_PASSWORD` doesn't match the wallet's password | Restore the correct password in `.env` and restart |
| `invalid passphrase for master public key` | Same issue, at the wallet level | Same fix |
| `Waiting for wallet encryption password` | Wallet not created yet | Follow Step 3 above |
| `UNLOCK FILE DOESN'T EXIST` | Warning from old unlock mechanism — can be ignored if wallet unlocks | No action needed |

If you lost the wallet password and have no funds in Lightning channels, you can reset the wallet:

```bash
# Stop LND
docker compose stop rtmpay-lnd

# Clear wallet and macaroon data (ALL LIGHTNING FUNDS WILL BE LOST)
docker run --rm -v rtmpayserver_rtmpay_lnd:/data alpine sh -c   "rm -f /data/data/chain/bitcoin/mainnet/wallet.db          /data/data/chain/bitcoin/mainnet/macaroons.db          /data/data/chain/bitcoin/mainnet/*.macaroon &&    grep -v noseedbackup /data/lnd.conf > /tmp/lnd.conf && mv /tmp/lnd.conf /data/lnd.conf"

# Set new password in .env, then restart and follow Step 3 again
docker compose up -d rtmpay-lnd
```

---

## 🔒 SSL Setup (Optional but Recommended)

If you already have **nginx** installed on your server, add this configuration:

**Step 1 — Create the site config:**

```bash
nano /etc/nginx/sites-available/pay.yourdomain.com
```

```nginx
server {
    listen 80;
    server_name pay.yourdomain.com;
    location /.well-known/acme-challenge/ { root /var/www/html; }
    location / { return 301 https://$host$request_uri; }
}

server {
    listen 443 ssl;
    http2 on;
    server_name pay.yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/pay.yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/pay.yourdomain.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;

    location / {
        proxy_pass http://localhost:23001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection upgrade;
        proxy_read_timeout 600;
        client_max_body_size 100M;
    }
}
```

**Step 2 — Enable and get SSL certificate:**

```bash
ln -s /etc/nginx/sites-available/pay.yourdomain.com /etc/nginx/sites-enabled/
nginx -t && nginx -s reload

# Install certbot if needed
apt install -y certbot python3-certbot-nginx

# Get certificate
certbot certonly --webroot -w /var/www/html -d pay.yourdomain.com \
  --email your@email.com --agree-tos --non-interactive
nginx -s reload
```

Certbot auto-renews your certificate every 90 days.

---

## 📊 Monitoring Your Services

Check the status of all services at any time:

```bash
# See all running containers
docker compose ps

# Follow logs for a specific service
docker compose logs -f rtmpay-btcpayserver   # BTCPayServer
docker compose logs -f rtmpay-lnd            # Lightning node
docker compose logs -f rtmpay-bitcoind       # Bitcoin Core sync progress
docker compose logs -f rtmpay-nbxplorer-rtm  # RTM indexer

# Check Bitcoin sync progress
docker compose logs rtmpay-bitcoind | grep progress | tail -5

# Restart a service
docker compose restart rtmpay-btcpayserver
```

### Expected Startup Sequence

| Time | What happens |
|---|---|
| 0 min | PostgreSQL starts and becomes healthy |
| 1 min | NBXplorer (RTM) connects to Raptoreum node |
| 1 min | BTCPayServer starts, accessible on port 23001 |
| 2 min | Bitcoin Core starts downloading headers |
| 5 min | LND starts, ready for wallet initialization |
| 2–5 days | Bitcoin Core finishes syncing → BTC + Lightning fully active |

---

## 🛒 Accepting Your First RTM Payment

Once your store is created in BTCPayServer:

1. **Set up your RTM wallet**: Store Settings → Wallets → Raptoreum → Generate new wallet (or import existing xpub)
2. **Create an invoice**: Invoices → Create invoice → set amount and currency
3. **Share the payment link** with your customer
4. **Done** — BTCPayServer automatically detects the payment, confirms it, and marks the invoice as paid

You can also integrate RTMPayServer into your website or e-commerce store using the BTCPayServer API or plugins for WooCommerce, Shopify, and more.

---

## 🛠️ Troubleshooting

### "RTM is not connecting to the blockchain"

Check that your Raptoreum node is reachable and the RPC credentials in `.env` are correct:

```bash
docker compose logs rtmpay-nbxplorer-rtm | tail -20
```

Look for: `Connected to WebSocket of NBXplorer (RTM)` — that means it's working.

### "Bitcoin is taking forever to sync"

This is normal. Bitcoin Core downloads the full history before pruning it. Progress is visible in:

```bash
docker compose logs rtmpay-bitcoind | grep progress | tail -3
```

You'll see `progress=0.XXXXX` — multiply by 100 for the percentage.

### "Lightning node is waiting for wallet"

This is expected on **first boot only**. The wallet has not been created yet. Follow the [Lightning Network setup steps](#lightning-network-lnd) — specifically Step 3 (create wallet via REST API).

### "LND is restarting every 30 seconds"

Check the logs: `docker compose logs rtmpay-lnd | tail -20`

The most common cause is `LND_WALLET_PASSWORD` not being set or not matching the password used when the wallet was created. See the [LND Crash Loop diagnosis table](#️-lnd-crash-loop--how-to-diagnose) in the Lightning section.

### "I can't access the web interface"

1. Check that BTCPayServer is running: `docker compose ps`
2. Check that port 23001 is accessible: `curl http://localhost:23001`
3. If using a domain, verify your DNS and nginx/SSL configuration

### General debugging

```bash
# See all logs at once
docker compose logs --tail=50

# Check which containers are unhealthy
docker compose ps | grep -v Up
```

---

## 🔄 Updating

```bash
# Pull latest images and restart
docker compose pull
docker compose up -d

# Or update a specific service
docker compose pull rtmpay-btcpayserver
docker compose up -d rtmpay-btcpayserver
```

---

## 🤝 Contributing & Community

This project is **100% open source** and community-driven. Contributions of any kind are welcome:

- 🐛 **Found a bug?** [Open an issue](https://github.com/cadenadb/rtmpayserver/issues) — describe what happened and we'll work on it together
- 💡 **Have an idea?** Open a feature request — all suggestions are welcome
- 🔧 **Can code?** Submit a pull request — even small improvements matter
- 📖 **Can write?** Help improve the documentation
- 🌍 **Speak another language?** Help translate the docs

No contribution is too small. If you found this useful, give the repo a ⭐ — it helps others find the project.

---

## 💼 Professional Services

Need help setting up your RTMPayServer, configuring Lightning, integrating with your store, or customizing the platform?

**Contact me on Telegram: [@cadenadb](https://t.me/cadenadb)**

Services available:
- Full server setup and deployment
- Custom domain + SSL configuration
- Store integration (WooCommerce, custom APIs)
- Lightning channel management
- Ongoing maintenance and support

---

## 📋 Related Repositories

| Repo | Description |
|---|---|
| [cadenadb/btcpayserver](https://github.com/cadenadb/btcpayserver) | BTCPayServer fork — RTM altcoin plugin, synced with upstream |
| [cadenadb/NBXplorer](https://github.com/cadenadb/NBXplorer) | NBXplorer fork — Raptoreum network provider |
| [cadenadb/raptoreumd](https://github.com/cadenadb/rtmpayserver) | Raptoreum full node Docker image (multi-arch) |

---

## 📄 License

[MIT License](LICENSE) — same as BTCPayServer. Free to use, modify, and distribute.

---

<div align="center">

**Built with ❤️ for the Raptoreum and Bitcoin communities**

[🌐 Live Demo](https://rtmpay.duckdns.org) · [📬 Telegram @cadenadb](https://t.me/cadenadb) · [🐛 Report a Bug](https://github.com/cadenadb/rtmpayserver/issues)

</div>
