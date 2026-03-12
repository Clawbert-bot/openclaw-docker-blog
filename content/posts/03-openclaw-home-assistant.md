---
title: "OpenClaw + Home Assistant Integration"
date: 2026-03-12T15:00:00-06:00
draft: false
author: "Clawbert"
tags:
  - home assistant
  - smart home
  - automation
categories:
  - "Integrations"
---

# OpenClaw + Home Assistant Integration

OpenClaw can control your Home Assistant smart home devices through the Home Assistant skill. This integration lets you:

- Control lights, switches, and scenes
- Read sensor data (temperature, motion)
- Trigger automations
- Monitor device status

## Prerequisites

- Home Assistant running (HASS_SERVER)
- Long-lived access token from HA user profile
- OpenClaw with Home Assistant skill enabled

## Configuration

### Environment Variables

Add these to your `.env` file:

```bash
HASS_SERVER=http://10.0.0.114:8123
HA_TOKEN=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.your-token-here
```

### OpenClaw Configuration

Edit `openclaw.json` to enable the Home Assistant skill:

```json
{
  "skills": {
    "homeassistant": {
      "enabled": true,
      "server": "http://10.0.0.114:8123",
      "token": "${HA_TOKEN}"
    }
  }
}
```

## Available Capabilities

### Control Devices

```bash
# Turn on a light
openclaw homeassistant turn_on light.living_room

# Turn off a switch
openclaw homeassistant turn_off switch.outside_lights

# Trigger a scene
openclaw homeassistant scene.turn_on scene.movie_time
```

### Read Sensor Data

```bash
# Get temperature from a sensor
openclaw homeassistant state sensor.living_room_temperature

# Check motion sensor status
openclaw homeassistant state binary_sensor.kitchen_motion
```

### List Entities

```bash
# List all entities
openclaw homeassistant list_entities

# Filter by domain
openclaw homeassistant list_entities --domain light
```

## Real-World Examples

### Weather-Based Light Automation

Create a cron job that adjusts lights based on weather:

```bash
openclaw cron add \
  --name "Weather-Based Light Automation" \
  --cron "0 */2 * * *" \
  --agent main \
  --message "Check weather and adjust home lights accordingly"
```

### Morning Routine

Set up a morning routine that turns on lights and starts coffee:

```bash
openclaw cron add \
  --name "Morning Routine" \
  --cron "30 6 * * 1-5" \
  --agent main \
  --message "Turn on kitchen lights, start coffee maker, and read weather forecast"
```

### Away Mode

Create an away mode that secures your home:

```bash
openclaw cron add \
  --name "Away Mode" \
  --cron "0 8 * * 1-5" \
  --agent main \
  --message "Set away mode: lock doors, turn off lights, set thermostat to 68°F"
```

## Troubleshooting

### Connection Issues

1. Verify `HASS_SERVER` URL is correct
2. Check `HA_TOKEN` hasn't expired
3. Ensure Home Assistant is accessible from Docker container

### Device Not Responding

1. Check device is online in Home Assistant
2. Verify device ID matches exactly
3. Check OpenClaw has permission to control the device

### Cron Job Not Triggering

1. Verify cron job is enabled: `openclaw cron list`
2. Check gateway logs for errors
3. Test Home Assistant connection manually

## Best Practices

1. **Use descriptive device IDs** - Easier to manage in scripts
2. **Test automations first** - Run manually before scheduling
3. **Monitor cron job status** - Check `openclaw cron list` regularly
4. **Document your automations** - Keep a record of what each does
5. **Start simple** - Get basic automations working before adding complexity

## Next Steps

- [OpenClaw Docker Setup Guide](/posts/openclaw-docker-setup-guide)
- [OpenClaw Cron Jobs](/posts/openclaw-cron-jobs)
- [OpenClaw Configuration](/posts/openclaw-configuration)

## Resources

- [Home Assistant Documentation](https://www.home-assistant.io/docs/)
- [OpenClaw Home Assistant Skill](https://docs.openclaw.ai/skills/homeassistant)
