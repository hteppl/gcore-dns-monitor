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

A lightweight monitoring service that tracks [Gcore DNS](https://gcore.com/dns) healthcheck metrics and sends Telegram
notifications when IP address health status changes.

## Features

<p align="center">
  <img src="https://raw.githubusercontent.com/hteppl/gcore-dns-monitor/master/.github/images/preview.png" alt="Preview" width="300">
</p>

- **Real-time Monitoring** - Continuously monitors Gcore DNS healthcheck states (UP/DOWN)
- **Instant Notifications** - Sends Telegram alerts when node status changes
- **Periodic Reminders** - Configurable reminders for DOWN healthchecks
- **Domain Filtering** - Monitor specific domains or all domains at once
- **Lightweight** - Single Python file with minimal dependencies
- **Docker Ready** - Easy deployment with Docker and Docker Compose

## Prerequisites

Before you begin, ensure you have the following:

- **Gcore Account** with DNS zones configured and healthchecks enabled
- **Gcore API Token** - Generate from [Gcore Portal](https://portal.gcore.com/) under API Tokens
- **Telegram Bot** - Create via [@BotFather](https://t.me/BotFather) and get the bot token
- **Telegram Chat ID** - Get your chat ID via [@username_to_id_bot](https://t.me/username_to_id_bot)

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

### Configuration Reference

| Variable            | Description                                       | Default | Required |
|---------------------|---------------------------------------------------|---------|----------|
| `API_TOKEN`         | Gcore API token in format `account_id$token`      | -       | Yes      |
| `TG_BOT_TOKEN`      | Telegram bot token from BotFather                 | -       | Yes      |
| `TG_CHAT_ID`        | Telegram chat ID for notifications                | -       | Yes      |
| `TG_TOPIC_ID`       | Telegram topic ID for forum groups                | -       | No       |
| `CHECK_INTERVAL`    | Polling interval in seconds                       | 30      | No       |
| `REMINDER_INTERVAL` | DOWN reminder interval in minutes (-1 to disable) | 30      | No       |
| `FILTER_DOMAIN`     | Filter by domain (empty = all domains)            | -       | No       |

## Installation

### Docker (recommended)

1. Create the docker-compose.yml:

```bash
services:
  gcore-monitor:
    image: hteppl/gcore-dns-monitor:latest
    container_name: gcore-dns-monitor
    restart: unless-stopped
    env_file:
      - .env
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
```

2. Create and configure your environment file:

```bash
nano .env  # or use your preferred editor
```

3. Start the container:

```bash
docker compose up -d && docker compose logs -f
```

### Manual Installation

1. Clone the repository:

```bash
git clone https://github.com/hteppl/gcore-dns-monitor.git
cd gcore-dns-monitor
```

2. Create a virtual environment (recommended):

```bash
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# or
.venv\Scripts\activate     # Windows
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Create and configure your environment file:

```bash
cp .env.example .env
```

5. Run the application:

```bash
python main.py
```

## How It Works

1. **Initial State** - On startup, the service fetches all healthcheck metrics from Gcore DNS API and sends an initial
   status report to Telegram

2. **Continuous Monitoring** - The service polls the Gcore API at the configured interval (`CHECK_INTERVAL`)

3. **Change Detection** - When a healthcheck state changes (UP to DOWN or vice versa), an immediate notification is sent

4. **Reminders** - If configured, periodic reminders are sent for all DOWN healthchecks at the specified interval (
   `REMINDER_INTERVAL`)

### Logs

Monitor logs to diagnose issues:

```bash
# Docker
docker compose logs -f

# Manual
# Logs are output to stdout with timestamps
```

## License

This project is licensed under the [GNU General Public License v3.0](LICENSE).
