# TFRNotifier

Polls the [FAA TFR list](https://tfr.faa.gov/tfr3/) every 30 minutes and sends a
Telegram alert when a West Palm Beach TFR is published or revoked early.

## How it works

- `.github/workflows/tfr-notify.yml` runs `tfr_wpb.py` on a 30-minute cron
  (plus manual dispatch).
- `tfr_wpb.py` fetches the FAA TFR feed, filters for ZMA / Palm Beach TFRs,
  and diffs against `state/last_wpb.json` to detect new and revoked TFRs.
- New/revoked TFRs are sent via Telegram; the updated state file is committed
  back to `main` so alerts are only sent once.
- A keepalive step re-enables the workflow via the GitHub API on every run so
  GitHub's 60-day inactivity auto-disable never kicks in.

## Setup

Repository secrets (Settings → Secrets and variables → Actions):

- `TELEGRAM_TOKEN` — Telegram bot token
- `TELEGRAM_CHAT_ID` — chat ID to send alerts to
