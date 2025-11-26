<p align="center">
  <img src="https://raw.githubusercontent.com/hteppl/gcore-dns-monitor/master/.github/images/logo.png" alt="Gcore DNS Monitor" width="600">
</p>

## gcore-dns-monitor

<p align="center">
  <a href="https://github.com/hteppl/gcore-dns-monitor/releases/"><img src="https://img.shields.io/github/v/release/hteppl/gcore-dns-monitor.svg" alt="Release"></a>
  <a href="https://hub.docker.com/r/hteppl/gcore-dns-monitor/"><img src="https://img.shields.io/badge/DockerHub-gcore--dns--monitor-blue" alt="DockerHub"></a>
  <a href="https://github.com/hteppl/gcore-dns-monitor/actions"><img src="https://img.shields.io/github/actions/workflow/status/hteppl/gcore-dns-monitor/dockerhub-publish.yaml" alt="Build"></a>
  <a href="https://www.python.org/"><img src="https://img.shields.io/badge/python-3.12-blue.svg" alt="Python 3.12"></a>
  <a href="https://opensource.org/licenses/GPL-3.0"><img src="https://img.shields.io/badge/license-GPLv3-green.svg" alt="License: GPL v3"></a>
</p>

---

A lightweight monitoring service that tracks Gcore DNS healthcheck metrics and sends Telegram notifications when IP
address health status changes.

## Features

- Monitors Gcore DNS healthcheck states (UP/DOWN)
- Sends Telegram notifications on status changes
- Periodic reminders for DOWN healthchecks
- Docker support for easy deployment

## Configuration

Copy [`.env.example`](.env.example) to `.env` and fill in your values:

```env
# Gcore API token (format: <account_id>$<token>)
API_TOKEN=

# Telegram bot token
TG_BOT_TOKEN=

# Telegram chat ID for notifications
TG_CHAT_ID=

# Telegram topic ID (optional, for forum groups)
TG_TOPIC_ID=

# Polling interval in seconds (default: 30)
CHECK_INTERVAL=30

# Reminder interval in minutes (default: 30, -1 to disable)
REMINDER_INTERVAL=30

# Filter by domain (optional, leave empty to monitor all domains)
FILTER_DOMAIN=
```

| Variable            | Description                                       | Default  |
|---------------------|---------------------------------------------------|----------|
| `API_TOKEN`         | Gcore API token                                   | required |
| `TG_BOT_TOKEN`      | Telegram bot token                                | required |
| `TG_CHAT_ID`        | Telegram chat ID                                  | required |
| `TG_TOPIC_ID`       | Telegram topic ID (optional)                      | -        |
| `CHECK_INTERVAL`    | Polling interval in seconds                       | 30       |
| `REMINDER_INTERVAL` | DOWN reminder interval in minutes (-1 to disable) | 30       |
| `FILTER_DOMAIN`     | Filter by domain (empty = all domains)            | -        |

## Quick Start

### Docker (recommended)

```bash
docker compose up -d
```

### Manual

```bash
pip install -r requirements.txt
python main.py
```

## License

This project is licensed under the [GNU General Public License v3.0](LICENSE).
