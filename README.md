# VESC Joystick

A single-page web app that turns an Android phone into an analog remote for a [VESC](https://vesc-project.com/) over Bluetooth LE: a vertical joystick for proportional throttle and brake, live gauges, and battery tracking.

> ⚠️ **Electric boards can hurt you.** Provided as is, no warranty. Always test with the board on its side and the wheels off the ground, at low current, before riding.

## Features

- **Joystick**: up = accelerate, down = brake, release = 0 A (coast). Arm switch, adjustable ramp, max accelerate/brake current (0–85 A).
- **Live gauges**: speed, power (battery voltage × battery current), motor current, battery %.
- **Infos tab**: battery %, last voltage read, last recharge detected, used mAh, estimated capacity, Wh used, distance since last charge, Wh/km, estimated remaining range.
- **Battery %**: coulomb counting stored on the phone. Capacity starts from your nominal value, then becomes the average of the last 5 measured deep discharges.
- **Automatic recharge detection**: on connection, if the voltage is above a threshold (default 32.2 V) and more than 1.5 V above the last voltage seen, counters reset. A manual button is also available.
- **Settings are saved** on the phone (current limits, wheel/motor/gear, cells in series, capacity, CAN ID).
- Optional **CAN forwarding** to a second VESC.

## Requirements

- A VESC with a BLE module exposing the **Nordic UART service** (`6e400001-b5a3-f393-e0a9-e50e24dcca9e`), e.g. the VESC project / Trampa module.
- **Chrome on Android** (Web Bluetooth). iOS Safari is not supported.
- The page must be served over **HTTPS** (e.g. GitHub Pages) or `localhost`.

## Install

1. Put these files at the root of a GitHub Pages site: `index.html`, `manifest.webmanifest`, `icon-192.png`, `icon-512.png`.
2. Open the page in Chrome, then ⋮ → **Install app**. It opens full screen and locked in portrait.

## Use

1. Close VESC Tool (the module accepts one connection at a time).
2. **Connect**, pick your VESC, then **Arm**, and keep your finger on the joystick.
3. In **⚙ Settings**, set pole pairs, gear ratio, wheel diameter, cells in series and nominal capacity so speed and battery % are right.
4. Press **Battery recharged** after each charge if it wasn't detected automatically.

## Safety

- Nothing moves until you arm. The page disarms itself when it goes to the background, on tab change, or if the connection drops.
- In VESC Tool, set a **short timeout** (0.2–0.5 s) and **Timeout Brake Current = 0**, so the board coasts if the phone disconnects.
- Saved current limits are restored at launch: check the sliders before riding.
- The VESC BLE module has no authentication: anyone nearby can connect while you aren't. Switch the board off when not in use.

## How it works

Plain VESC serial protocol over BLE: `COMM_SET_CURRENT` (6), `COMM_SET_CURRENT_BRAKE` (7), `COMM_GET_VALUES` (4) for telemetry, `COMM_FORWARD_CAN` (34), at ~20 Hz (telemetry ~5 Hz).

## Limits

- Battery % is an estimate and drifts over time; data lives in the browser storage (clearing Chrome data resets it).
- Temperatures, Ah and tachometer fields depend on the VESC firmware version.
- Not tested with every BLE module; modules not using Nordic UART need other UUIDs.
