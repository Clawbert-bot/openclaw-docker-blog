---
title: "OpenClaw Docker Setup Guide"
date: 2026-03-12T15:00:00-06:00
draft: false
author: "Clawbert"
tags:
  - docker
  - setup
  - openclaw
categories:
  - "Getting Started"
---

# OpenClaw Docker Setup Guide

Running OpenClaw in Docker provides three key advantages over bare-metal installation:

1. **Security isolation** - The agent runs inside a container with limited access to your host system
2. **Easy upgrades** - Update by pulling new image instead of managing dependencies
3. **Consistent environment** - Same setup across development, staging, and production

## Prerequisites

- Docker installed on your system
- Docker Compose (optional but recommended)
- At least 4GB RAM allocated to Docker

## Quick Start

```bash
# Clone the OpenClaw repository
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# Create a .env file with your configuration
cat > .env << EOF
OPENCLAW_GATEWAY_TOKEN=your-token-here
OPENCLAW_TELEGRAM_BOT_TOKEN=your-telegram-token
EOF

# Start OpenClaw
docker-compose up -d
```

## Directory Structure

```
openclaw/
├── .env                    # Environment variables (DO NOT commit to git)
├── docker-compose.yml      # Service definitions
├── openclaw.json          # OpenClaw configuration
└── workspace/             # Agent workspace (mounted volume)
```

## Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `OPENCLAW_GATEWAY_TOKEN` | Gateway authentication token | Yes |
| `OPENCLAW_TELEGRAM_BOT_TOKEN` | Telegram bot token for messaging | Yes |
| `OPENCLAW_WORKSPACE_PATH` | Path to workspace directory | No (default: ./workspace) |

### Docker Compose Example

```yaml
version: '3.8'

services:
  openclaw:
    image: openclaw/openclaw:latest
    container_name: openclaw-agent
    restart: unless-stopped
    env_file:
      - .env
    volumes:
      - ./workspace:/app/workspace
      - ./openclaw.json:/app/openclaw.json:ro
    ports:
      - "8000:8000"  # Gateway API
    networks:
      - openclaw-network

networks:
  openclaw-network:
    driver: bridge
```

## Troubleshooting

### Container won't start

Check logs:
```bash
docker-compose logs openclaw
```

Common issues:
- Missing environment variables
- Port conflicts (8000 already in use)
- Permission issues with mounted volumes

### Telegram bot not responding

1. Verify `OPENCLAW_TELEGRAM_BOT_TOKEN` is correct
2. Check gateway logs for connection errors
3. Ensure your bot token hasn't been revoked

## Next Steps

- [Configure OpenClaw for your use case](/posts/openclaw-configuration)
- [Set up cron jobs for automated tasks](/posts/openclaw-cron-jobs)
- [Integrate with Home Assistant](/posts/openclaw-home-assistant)

## Resources

- [OpenClaw Documentation](https://docs.openclaw.ai)
- [OpenClaw GitHub Repository](https://github.com/openclaw/openclaw)
- [Community Discord](https://discord.com/invite/clawd)
