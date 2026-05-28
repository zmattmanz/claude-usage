# Claude Usage Stick

A tiny standalone dashboard for your Claude Max subscription usage,
running on the M5StickS3 (ESP32-S3).

Connects to WiFi, polls your claude.ai usage every 5 minutes,
and shows your rate limit bars on screen.

## Screens

**Screen 1 — Current Session**
Big percentage showing your 5-hour rolling usage, colored bar
(green → yellow → red), and countdown timer until reset.

**Screen 2 — Weekly Limits**
All four usage bars: 5-hour session, all models (7-day),
Sonnet-only, and Claude Design.

**Screen 3 — Device**
WiFi signal, battery, last sync time, API status.
Hints for reconfiguring.

## Controls

- **Button A (front)** — cycle screens
- **Button B (side)** — force refresh now
- **Hold A (3s)** — reconfigure (WiFi, session key)
- Screen dims after 60s, turns off after 5m. Any button wakes.

## Setup

### 1. Flash

```bash
cd claude-usage-stick
pio run -t upload
pio device monitor
```

### 2. Configure (first boot)

The serial monitor will prompt you for four things:

1. **WiFi SSID** — your network name
2. **WiFi Password** — your network password
3. **Org ID** — from Chrome DevTools:
   - Go to claude.ai, press F12
   - Application → Cookies → https://claude.ai
   - Copy the value of `lastActiveOrg`
4. **Session Key** — same place, copy the value of `sessionKey`

These are saved to the device and persist across reboots.

### 3. Refresh Session Key

The `sessionKey` cookie expires every ~2 weeks. When the device
shows "Session expired", grab a fresh one from your browser
(same steps as setup) and hold Button A for 3 seconds to
re-enter config.

## How It Works

Calls `GET https://claude.ai/api/organizations/{org}/usage`
with your session cookie. Returns your exact rate limit
utilization percentages — the same data shown on the
claude.ai settings/usage page.

No API costs. No BLE needed. Just WiFi.
