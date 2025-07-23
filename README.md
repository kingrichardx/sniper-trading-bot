# Solana Sniper 🚀

A high-performance, async Rust bot for **copy trading, sniping and PumpFun / PumpSwap monitoring** on the Solana blockchain. It mirrors trades from any wallet you choose, provides configurable buy / sell logic, and ships with utilities for managing token accounts, wrapping SOL, and keeping your Telegram chat informed in real-time.

> This project is for **educational purposes only**. Use at your own risk.

---

## ✨ Key Features

- **Copy-Trading Engine** – Follows a single wallet or a list of wallets (`IS_MULTI_COPY_TRADING=true`) and replicates their buys/sells almost instantly.
- **PumpFun & PumpSwap Listener** – Detects new launches, sends Telegram notifications, or executes trades depending on your settings.
- **DEX Flexibility** – Supports Jupiter & OKX aggregators with automatic protocol-selection (`PROTOCOL_PREFERENCE`).
- **Token Account Maintenance** – Auto-close empty accounts, unwrap WSOL, cache wallet/target mints for speed.
- **Blockhash Processor** – Keeps a fresh recent blockhash in memory to sign transactions faster.
- **Health Checks & Metrics** – Optional endpoints to plug into your monitoring stack.
- **Cross-Compilation** – Build native binaries for Windows via the supplied `Makefile` or `build.sh`.

---

## 🔧 Prerequisites

1. **Rust 1.72+** with the 2021 edition (`rustup update`).
2. A Solana keypair (`solana-keygen new -o ./id.json`).
3. NodeJS & pm2 (only if you want to daemonise with `make start`).
4. For Windows builds: `mingw-w64` (installed automatically by `make install`).

---

## 🚀 Quick Start

```bash
# 1. Clone & enter the repo
$ git clone https://github.com/yourname/solana-sniper.git
$ cd solana-sniper

# 2. Configure
$ cp .env.example .env   # create your environment file
$ $EDITOR .env           # fill in the values (see list below)

# 3. Build & run
$ cargo run --release              # runs normally
$ cargo run --release -- --wrap    # wrap SOL to WSOL
$ cargo run --release -- --unwrap  # unwrap WSOL back to SOL
```

To run in the background on Linux:

```bash
make build          # compile release binary
make start          # pm2 start target/release/solana-sniper
```

Cross-compile for Windows:

```bash
make build-x86_64   # 64-bit
make build-i686     # 32-bit
```

---

## ⚙️ Important Environment Variables

| Variable                                                                | Description                                                                |
| ----------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `RPC_URL`                                                               | HTTPS endpoint of your Solana RPC node (dedicated, rate-limited preferred) |
| `TELEGRAM_BOT_TOKEN` / `TELEGRAM_CHAT_ID`                               | Credentials for Telegram alerts                                            |
| `COPY_TRADING_TARGET_ADDRESS`                                           | Wallet to mirror (single address)                                          |
| `IS_MULTI_COPY_TRADING`                                                 | Set to `true` to read a comma-separated list from `TARGET_ADDRESS_LIST`    |
| `EXCLUDED_ADDRESSES`                                                    | Comma-separated wallets to ignore                                          |
| `WRAP_AMOUNT`                                                           | Amount of SOL (decimal) to wrap when `--wrap` flag is used                 |
| `PROTOCOL_PREFERENCE`                                                   | `JUPITER`, `OKX`, or leave blank for auto                                  |
| `BUY_IN_SELL` / `BUY_IN_SELL_LIMIT`                                     | Enable a fixed sell strategy after buy & the limit price                   |
| `IS_CHECK_TARGET_WALLET_TOKEN_ACCOUNT`                                  | `true` to cache target wallet tokens at startup                            |
| `NOZOMI_TIP_VALUE`, `FLASHBLOCK_API_KEY`, `ZERO_SLOT_URL`, `NOZOMI_URL` | Advanced / optional integrations                                           |

If an env var is missing the bot will fall back to a sensible default or disable the related feature.

---

## 📂 Project Structure

```
├─ src/
│  ├─ engine/            # trading logic (copy trading, swap, …)
│  ├─ dex/               # DEX clients & helpers
│  ├─ tx_processor/      # transaction crafting utilities
│  ├─ utilities/         # health-check, caching, telegram, etc.
│  └─ main.rs            # entry point
├─ build.sh              # convenience script for Windows cross-compile
├─ Makefile              # common targets (install, build, start…)
└─ raydium_launchpad.json# sample dataset used by the bot
```

---

## 🛠️ Development

Standard Rust workflow applies:

```bash
cargo fmt      # format
test           # add unit tests as you contribute
cargo clippy   # lint
```

Logging is colorised; tune verbosity by editing `library/logger.rs` or piping `RUST_LOG`.

---

## 📜 License

Released under the MIT License. See [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgements

- Solana Labs & Anchor for the excellent tooling.
- Jupiter, OKX, PumpFun and PumpSwap communities for data & inspiration.
- All open-source contributors – you rock!
