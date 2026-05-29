# Claude Usage

A desk gadget that shows your Claude AI usage limits in real time on a tiny always-on screen.

Built on the [M5StickS3](https://shop.m5stack.com/products/m5sticks3) (ESP32-S3, 135×240 TFT).

## Overview

If you've hit Claude's usage limits mid-conversation, you know the frustration. Your only options are keeping a second tab open to monitor it or squinting at the usage indicator in the chat window. Neither is great when you're actually trying to get work done.

This fixes that. Plug it in, set it on your desk, and your usage is always one glance away. No API costs—it just reads from Claude.ai's usage endpoint with your session cookie.

Four screens, one button press apart:

- **Session**—The main screen. A 240° arc gauge with your current 5-hour usage percentage, a live countdown until reset, a velocity arrow showing whether you're trending up or down, and a status label (Low, Moderate, High, Critical). The arc breathes—layered shadows drift at the fill edge so you know the thing is alive and working.
- **Usage**—A trend chart of your usage over the last 5 hours with a filled curve, plus breakdowns for overall, Sonnet, and design tool usage with progress bars.
- **Device**—WiFi status, power source, last sync time, API health. The "is this thing on?" screen.
- **Key Status**—Session key validity, age, and estimated expiration. The expiration countdown changes color as it gets close.

## Features

**Auto-updates every 5 minutes.** A rising double-chime means you're fine. A falling tone means you should probably wrap up what you're doing.

**Shake to refresh.** Don't want to wait 5 minutes? Shake the device or press the side button (B). An orbiting dot plays while it syncs.

**Three themes.** Double-press the side button (B) to rotate:
- **Amber Cream**—warm orange (default)
- **Sage**—muted green
- **Electric Indigo**—cool blue/purple

Persists across reboots.

**Landscape mode.** Tilt it sideways. The session screen rearranges—arc on the left, timer on the right. Works both directions.

**80% alert.** The arc and status label pulse red when you cross 80%. Hard to miss. That's the point.

**Idle detection.** If your usage flatlines for ~15 minutes, the screen transitions to a floating "RELAX" animation. Any interaction snaps it back.

**Night mode.** Hold the side button (B) for 2 seconds. Hold again to restore.

**Connection pulse.** A dot in the top-right corner of every screen pulses on a one-second interval when connected. Hollow circle when disconnected.

**Audio feedback.** Screen changes, refreshes, theme switches, fetch completions—each gets a distinct tone. Nothing loud, just enough to register without looking at it.

## Hardware

- **[M5StickS3](https://shop.m5stack.com/products/m5sticks3)** (ESP32-S3, 135×240 TFT, BMI270 IMU)
- USB-C power—designed to stay plugged in on your desk
- Nothing else needed

## Setup

### Prerequisites

You'll need two values from your browser before starting. Open [Claude.ai](https://claude.ai) and pull up Developer Tools (F12):

**Organization ID:**
1. Go to Application → Cookies → `https://claude.ai`
2. Find `lastActiveOrg`
3. Copy the value

**Session Key:**
1. Same panel—find `sessionKey`
2. Copy the full value

These expire periodically. When yours does, the Key Status screen will tell you. Long-press the front button (A) for 3 seconds to get back into setup.

### First Boot

1. Plug in via USB-C. Splash screen fades in.
2. Device creates a WiFi network called **"claude usage"**.
3. Setup screen shows the network name, IP, and a unique password.
4. Connect from your phone.
5. Open **192.168.4.1** in a browser.
6. Paste in your WiFi credentials, org ID, and session key.
7. Hit **Save & Connect**. Device restarts on your network.

### Re-Entering Setup

Long-press the front button (A) for 3 seconds—a progress bar fills at the bottom. Keep holding until it completes. WiFi credentials get cleared; theme and chart history stay.

## Controls

| Action | Input |
|--------|-------|
| Cycle screens | Front button (A) |
| Refresh data | Side button (B) |
| Change theme | Double-press side button (B) |
| Night mode | Hold side button (B), 2 seconds |
| Shake refresh | Shake the device |
| Enter setup | Hold front button (A), 3 seconds |

## Limitations

- **The usage endpoint is unofficial.** This reads from Claude.ai's internal API, not a documented public endpoint. Anthropic could change or remove it at any time.
- **Session cookies expire.** There's no way around this—you'll need to update your `sessionKey` periodically. The Key Status screen tells you when it's getting close.
- **Landscape is session-only.** The other three screens are portrait only. Tilt back upright to access them.
- **No battery percentage.** The M5PM1 power chip doesn't expose accurate battery telemetry. The device shows "USB" when plugged in, which is the honest answer. It's designed to stay plugged in anyway.
- **Requires WiFi.** No offline mode. If your network goes down, the device shows stale data until it reconnects.

## Build

PlatformIO project targeting M5StickS3.

```
pio run -t upload --upload-port COM4
```

Swap `COM4` for your port.

## Dependencies

- **espressif32** (^6.9.0)
- **M5Unified** (^0.2.4)
- **ArduinoJson** (^7.2.0)

Custom fonts (Bebas Neue, Montserrat) included as headers in `src/`.

## License

MIT License. See [LICENSE](LICENSE) for details.
