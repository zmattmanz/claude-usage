# Claude Usage

A desk gadget that shows your Claude AI usage limits in real time on a tiny always-on screen.

## Overview

If you've hit Claude's usage limits mid-conversation, you know the frustration. Your only options are keeping a second tab open to monitor it or squinting at the usage indicator in the chat window. Neither is great when you're actually trying to get work done.

This fixes that. Plug it in, set it on your desk, and your usage is always one glance away. No API costs—it just reads from Claude.ai's usage endpoint with your session cookie.

## v1.1—Polish Release

Split the old DEVICE screen into CONNECTION and DEVICE. CONNECTION shows WiFi SSID, signal strength (RSSI in dBm), API status, and sync time. DEVICE is now hardware-only—power voltage, chip temp in both °F and °C, and uptime since boot. Five screens instead of four.

The session arc is anti-aliased in both orientations—supersampled at 3× and downscaled so the edges are smooth instead of stair-stepped. When new data arrives the arc sweeps and the number counts to the new value with an ease-out curve instead of snapping. Breathing shadow bands pulse beyond the fill at two different sine rates.

Audio got real: distinct chimes when usage crosses 50%, 75%, and 90% so you hear the device shift gears from across the room without looking. An ascending chord plays when the 5-hour window resets back to zero—a little "you're free" moment. Normal fetches still get the subtle two-tone ding.

Double-tap the case to refresh—firmware detects two quick taps on the accelerometer and triggers a fetch without buttons or shaking. Shares the 8-second cooldown with shake so you can't spam it.

Fetch interval is adaptive. Polls every 10 minutes under 60% usage, every 5 minutes from 60–80%, every 2 minutes above 80%. More data when you're watching the gauge, less when you're not.

When your session key expires the session screen shows a clear KEY EXPIRED notice with a "hold A to re-setup" hint instead of a raw HTTP code.

Relax mode drifts the dot grid in a random direction each time it activates—subtle, but it makes the idle screen feel alive.

Under the hood—CPU dropped from 240 to 160 MHz (the workload doesn't need it, saves power, runs a few degrees cooler), WiFi modem sleep enabled between fetches, ~160 lines of dead code removed, typography standardized across all screens, and a handful of bug fixes including the 0%/reset ping loop that caused rapid API calls and premature relax mode, landscape dot flicker, the arc gap painting over the status label, and transition glitches between relax and session screens.

Five screens, one button press apart:

- **Session**—The main screen. A 240° arc gauge with your current 5-hour usage percentage, a live countdown until reset, and a status label (Low, Moderate, High, Critical). The arc breathes—layered shadows drift at the fill edge so you know the thing is alive and working.
- **Usage**—A trend chart of your usage over the last 5 hours with a filled curve, plus breakdowns for overall, Sonnet, and design tool usage with progress bars.
- **Connection**—WiFi network, signal strength, API status, and last sync time. The "is this thing on?" screen.
- **Device**—Power voltage, chip temperature in °F and °C, and uptime since boot.
- **Key Status**—Session key validity, age, and estimated expiration. The expiration countdown changes color as it gets close.

## Features

**Adaptive updates.** Polls every 10 minutes under 60% usage, every 5 minutes from 60–80%, every 2 minutes above 80%. Distinct chimes play when usage crosses 50%, 75%, and 90%. An ascending chord plays when the window resets.

**Shake to refresh.** Don't want to wait? Shake the device, press the side button (B), or double-tap the case. An orbiting dot plays while it syncs.

**Three 'Themes'.** There are three "themes". They have official names: Double-press the side button (B) to rotate:
- **Amber Cream**—warm orange (default), vintage
- **Sage**—muted green, industrial
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
| Double-tap case | Refresh (gentler alternative to shake) |
| Enter setup | Hold front button (A), 3 seconds |

## Limitations + Known Issues

- **The usage endpoint is unofficial.** This reads from Claude.ai's internal API, not a documented public endpoint. Anthropic could change or remove it at any time.
- **Session cookies expire.** There's no way around this—you'll need to update your `sessionKey` periodically. The Key Status screen tells you when it's getting close.
- **Landscape is session-only.** The other four screens are portrait only. Tilt back upright to access them.
- **No battery percentage.** The M5PM1 power chip doesn't expose accurate battery telemetry. The device shows "USB" when plugged in, which is the honest answer. It's designed to stay plugged in anyway.
- **Requires WiFi.** No offline mode. If your network goes down, the device shows stale data until it reconnects.

## Build

PlatformIO project targeting M5StickS3.

```
pio run -t upload --upload-port COM4
```

Swap `COM4` for your port.

## License

MIT License. See [LICENSE](LICENSE) for details.
