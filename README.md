# Opinion Signal Bot

> Real-time signal alerts for Opinion prediction market on BNB Chain. Monitors oracle probability shifts, order book imbalances, and large position entries across multi-outcome markets and delivers structured trade signals to Telegram before the crowd moves.

*Last updated: March 2026*

## What is Opinion Signal Bot?

Opinion Signal Bot is a market intelligence layer for Opinion traders on BNB Chain. It watches all active Opinion markets for high-probability setups - AI oracle probability upgrades, sudden CLOB imbalances, whale entries in specific outcomes - and sends structured signal cards to your Telegram the moment conditions align.

![preview_opinion-signal-bot](https://github.com/user-attachments/assets/a4a7dfef-d671-4484-8138-6dd4cb63903d)

No auto-execution. Signals are delivered with market, outcome, direction, entry zone, suggested stop, and the specific trigger that fired. You decide whether to trade. Designed for Opinion traders who want systematic market scanning without giving up execution control.

Opinion's AI oracle is the core signal source - when oracle probability for an outcome diverges from the CLOB implied price by more than your threshold, a signal fires.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github.com/cobracat88/opinion-signal-bot/releases) |

---

## Signal Triggers

| Signal Type | Trigger Condition | Description |
|-------------|------------------|-------------|
| Oracle Upgrade | Oracle prob increases > 8% on an outcome | AI model revised its estimate upward |
| Oracle Downgrade | Oracle prob decreases > 8% | AI model becoming less confident |
| CLOB Imbalance | Bid/ask ratio > 3:1 on one outcome | Strong directional order pressure |
| Whale Entry | Single order > $5,000 on one outcome | Large participant positioning |
| Price Drift | Outcome price moves > 10% in 30 min | Rapid market re-pricing |

---

## Engine Features

* **Full market monitor** - scans all active Opinion markets continuously
* **AI oracle feed** - reads real-time oracle probability updates from Opinion API
* **CLOB imbalance detector** - calculates bid/ask ratio at each outcome
* **Whale filter** - flags single orders above configurable size threshold
* **Signal combinator** - optionally require 2+ triggers before alerting
* **Telegram delivery** - sends formatted signal cards with outcome, entry zone, SL, TP
* **Signal log** - stores all alerts with trigger data and subsequent price outcome
* **Backtest mode** - evaluates historical signal accuracy against market resolution

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **Signal rules** | Config-based | Full code access |
| **Delivery** | Telegram | Telegram + JSON |
| **Config** | `config.toml` | Direct code access |
| **Logs** | Dashboard | JSON per signal |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - set Telegram bot token and signal thresholds
# 3. Run Signal Bot - alerts arrive within seconds of trigger conditions
```

### Python

```bash
cd opinion-signal-bot/python
pip install -r requirements.txt
python opinion-signal-bot-raw-v.1.4.14.py
```

---

## How It Works

1. **Poll** - fetches oracle probabilities, CLOB depth, and recent trades for all Opinion markets
2. **Evaluate** - checks each outcome against all active signal triggers
3. **Combine** - fires alert when configured minimum triggers are met
4. **Deliver** - sends Telegram signal card with actionable trade detail

### Config Reference

```toml
[opinion]
api_base = "https://api.opinion.io"
poll_interval_seconds = 8
categories = []   # empty = all categories

[signals]
min_triggers_to_fire = 1
oracle_upgrade_threshold_pct = 8.0
clob_imbalance_ratio = 3.0
whale_order_usd = 5000
price_drift_pct = 10.0
drift_window_min = 30

[alerts]
telegram_bot_token = ""
telegram_chat_id = ""
signal_cooldown_min = 20
```

---

## Signal Card Format (Telegram)

```
OPINION SIGNAL
Market: Will BTC hit $120K in 2026?
Outcome: Yes - above $120K
Direction: BUY
Entry zone: 0.38 - 0.42
Stop-loss: 0.18
Take-profit: 0.75
Triggers: oracle_upgrade (+11.2%) + clob_imbalance (4.1:1 bid)
Confidence: HIGH (2/2 triggers)
Time: 2026-03-19 05:12 UTC
```

---

## Verified Live

**Configuration used:**
* All categories, oracle upgrade threshold 8%, CLOB imbalance 3:1

| | Details |
|---|---|
| Market | Will BTC hit $120K in 2026? |
| Outcome | Yes - above $120K |
| Triggers | Oracle +11.2% + CLOB 4.1:1 bid |
| Entry zone | 0.38 - 0.42 |
| Delivered | 2026-03-19 05:12 UTC |

| | Result (tracked) |
|---|---|
| Price after signal | moved to 0.71 within 48h |
| Signal accuracy | TP zone reached |

![opinion signal history log](https://github.com/user-attachments/assets/e081236c-2f2c-485f-8f79-d64c9126babf)

---

## Frequently Asked Questions

**Does the bot trade automatically?**
No. Opinion Signal Bot is a signal delivery tool only. It sends alerts to Telegram; you execute trades manually on Opinion.

**What is the oracle upgrade signal?**
When Opinion's AI oracle increases its probability estimate for a specific outcome by more than your threshold, it signals the AI model has found new evidence supporting that outcome. Buying that outcome before the CLOB reprices is the core edge.

**Can I require multiple triggers before a signal fires?**
Yes. Set `min_triggers_to_fire = 2` to only receive high-confidence alerts where two independent signals align simultaneously.

**Does it cover all Opinion market categories?**
Yes. Leave `categories = []` to monitor all categories, or specify a subset to reduce noise.

**Can I backtest signal accuracy?**
Yes. `scripts/backtest.py` replays historical oracle and CLOB data and shows what percentage of signals resulted in outcomes hitting the take-profit zone before stop.

**How fast are alerts delivered?**
Signals are evaluated on every poll cycle (default 8 seconds). Alert delivery to Telegram typically takes under 2 seconds after trigger detection.

---

## Use Cases

- **Opinion signal alerts** - receive Telegram alerts when AI oracle upgrades an outcome probability
- **BNB prediction market signals** - detect CLOB imbalances and whale entries before price moves
- **Oracle probability signal bot** - track AI oracle confidence shifts across all Opinion markets
- **Opinion whale tracker** - alert on large single-outcome CLOB orders from institutional participants
- **Prediction market edge signals** - surface mispricings between oracle probability and market price

---

## Repository Structure

```
opinion-signal-bot/
+-- opinion-signal-bot-raw-v.1.4.14.exe
+-- config.toml
+-- data/
|   +-- signals/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- monitor.py
|   |   +-- oracle_reader.py
|   |   +-- clob_analyzer.py
|   |   +-- telegram_sender.py
|   +-- scripts/
|   |   +-- backtest.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, httpx, web3, typer[all], python-telegram-bot, devtools
```

* Opinion API access (public market data, no key required)
* Telegram bot token for signal delivery
* Python 3.10+

---

*Oracle moves first. You move second.*
