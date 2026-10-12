# VESC Joystick

A single-file web page that turns an Android phone into an analog remote for a [VESC](https://vesc-project.com/): one vertical joystick for proportional throttle and brake, over Bluetooth LE.

> ⚠️ **Electric boards can hurt you.** Provided as is, no warranty. Always test with the board on its side and the wheels off the ground, at low current, before riding.

## Requirements

- A VESC with a BLE module exposing the **Nordic UART service** (`6e400001-b5a3-f393-e0a9-e50e24dcca9e`), such as the VESC project / Trampa module.
- **Chrome on Android** (Web Bluetooth). iOS Safari is not supported.
- The page must be served over **HTTPS** (e.g. GitHub Pages) or `localhost`.

## Use

1. Close VESC Tool (the module accepts one connection at a time).
2. Tap **Connect** and pick your VESC.
3. Tap **Arm**, then keep your finger on the joystick: **up = accelerate, down = brake**. Release = 0 A (coast).

Settings: max accelerate / brake current (0–85 A), ramp rate (A/s), optional CAN ID for a second VESC. Defaults are conservative (6 A / 6 A / 8 A/s); the VESC's own motor limits still apply.

## Safety

- Nothing moves until you arm. The page disarms itself if it goes to the background or the connection drops.
- In VESC Tool, set a **short timeout** (0.2–0.5 s) and **Timeout Brake Current = 0**, so the board coasts if the phone disconnects.
- The VESC BLE module has no authentication: anyone nearby can connect while you aren't. Switch the board off when not in use.

## How it works

Plain VESC serial protocol over BLE: `COMM_SET_CURRENT` (6), `COMM_SET_CURRENT_BRAKE` (7), `COMM_FORWARD_CAN` (34), resent at ~20 Hz.

## Install

Host `index.html` on GitHub Pages, open it in Chrome, then ⋮ → **Add to Home screen**.
