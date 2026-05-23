# Decisions — drone-relay-beam-tracking

- RX frequency: 5.8 GHz — matches existing FPV drone VTX; no modification to FPV drone needed
- TX frequency: 1.3 GHz — better ground penetration and range vs 5.8 GHz for downlink to pilot
- No GPS allowed — design constraint; all tracking must be self-contained using RSSI
- Antenna diversity: lollypop omni (always on) + PTZ patch (steered) — omni as fallback if patch loses lock
- PTZ Option B: KST DS215MG 270° servo for pan, MG90S 180° for tilt — best weight/range tradeoff
- Processing unit: ESP32 — has ADC for RSSI, I2C for IMU, PWM for servos, sufficient compute, ~3g
- IMU: MPU-6050 — compensates relay drone attitude (yaw/pitch) to maintain world-frame pointing
- Tracking algorithm: RSSI hill-climbing (scan → lock → dither) — no external position data required
- Tilt axis mounting: servo center = –45°, giving 0° to –90° coverage — matches geometry (relay above FPV)
