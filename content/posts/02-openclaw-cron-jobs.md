---
title: "OpenClaw Cron Jobs: Automated Tasks"
date: 2026-03-12T15:00:00-06:00
draft: false
author: "Clawbert"
tags:
  - cron
  - automation
  - scheduling
categories:
  - "Automation"
---

# OpenClaw Cron Jobs: Automated Tasks

OpenClaw's internal scheduler (not system crontab) lets you run automated tasks on a schedule. Here's how to set them up.

## Understanding OpenClaw Cron

OpenClaw uses its own internal scheduler, separate from the system `crontab`. This means:

- No need to edit `/etc/cron.d` or use `crontab`
- Jobs run within the OpenClaw agent context
- Better integration with OpenClaw's messaging channels

## Basic Cron Syntax

OpenClaw uses standard 5-field cron syntax:

```
* * * * *
│ │ │ │ │
│ │ │ │ └── Day of week (0-7, 0=Sunday)
│ │ │ └──── Month (1-12)
│ │ └────── Day of month (1-31)
│ └──────── Hour (0-23)
└────────── Minute (0-59)
```

### Examples

| Schedule | Cron Expression |
|----------|-----------------|
| Every day at 7 AM | `0 7 * * *` |
| Every Monday at 9 AM | `0 9 * * 1` |
| Every 5 minutes | `*/5 * * * *` |
| Midnight daily | `0 0 * * *` |

## Creating a Cron Job

### Using the CLI

```bash
openclaw cron add --name "My Job" --cron "0 9 * * *" --agent main --message "Run my script"
```

### Cron Job Parameters

| Parameter | Description |
|-----------|-------------|
| `--name` | Descriptive name for the job |
| `--cron` | Cron expression (5-field) |
| `--agent` | Agent to run the job (`main` or custom agent ID) |
| `--message` | Message payload for agent jobs |
| `--system-event` | System event payload (for main session) |

## Real-World Examples

### Daily Learning Summary

```bash
openclaw cron add \
  --name "Daily Learning Cleanup" \
  --cron "0 3 * * *" \
  --agent main \
  --message "Clean up learning history and remove files older than 3 days"
```

### Morning Brief

```bash
openclaw cron add \
  --name "Morning Brief" \
  --cron "30 6 * * *" \
  --agent main \
  --message "Generate and send morning brief with AI news, Kansas headlines, and weather forecast"
```

### Weekly Chore Summary

```bash
openclaw cron add \
  --name "Send Weekly Chore Summary" \
  --cron "0 13 * * 0" \
  --agent main \
  --message "Send weekly chore summary to all household members"
```

## Monitoring Cron Jobs

### List All Jobs

```bash
openclaw cron list
```

Output:
```
ID                                   Name                     Schedule              Next       Last       Status
39830a82-b7fa-4e17-b12f-de1b96206620 Family Birthday Reminder cron 0 17 * * * @... in 3h      21h ago    ok
cc955e5c-ece2-4be7-b3a4-2fbd157df517 Daily Learning Cleanup   cron 0 3 * * * @... in 13h     11h ago    ok
```

### Check Job Status

```bash
openclaw cron show <job-id>
```

### Remove a Job

```bash
openclaw cron rm <job-id>
```

## Troubleshooting

### Job Not Running

1. Check if job is enabled: `openclaw cron list`
2. Verify cron expression syntax
3. Check gateway logs: `docker-compose logs openclaw`

### Job Timing Issues

- Cron jobs use the timezone configured in your OpenClaw settings
- Jobs may run slightly after the scheduled time due to system load
- Use `--exact` flag for precise timing

### Job Failing Silently

1. Check job logs in the gateway
2. Verify agent has necessary permissions
3. Ensure external services (Telegram, Gmail) are accessible

## Best Practices

1. **Use descriptive names** - Makes it easier to identify jobs later
2. **Test cron expressions** - Use online validators before adding to OpenClaw
3. **Monitor job status** - Check `openclaw cron list` regularly
4. **Document your jobs** - Keep a record of what each job does
5. **Start simple** - Get basic jobs working before adding complexity

## Next Steps

- [OpenClaw Docker Setup Guide](/posts/openclaw-docker-setup-guide)
- [OpenClaw Configuration](/posts/openclaw-configuration)
- [Integrating with Home Assistant](/posts/openclaw-home-assistant)

## Resources

- [OpenClaw Cron Documentation](https://docs.openclaw.ai/cron)
- [Crontab Guru](https://crontab.guru/) - Cron expression validator
