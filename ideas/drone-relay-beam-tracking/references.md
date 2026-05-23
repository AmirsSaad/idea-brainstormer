# References — drone-relay-beam-tracking

## Relay concept (preserved)
- [1.2GHz–1.3GHz to 5.8GHz Wireless Relay – Oscar Liang](https://oscarliang.com/1-3ghz-5-8ghz-relay/) — Oscar Liang. Comprehensive guide to the basic relay concept: 5.8 GHz VRX out → 1.3 GHz VTX in. Confirms latency is ~1–2 ms, 20 km+ range achieved.
- [A Simple 1.3 GHz Ground Relay Station – RF Eng FPV](https://rfengfpv.wordpress.com/2018/04/12/a-simple-1-3-ghz-ground-relay-station/) — RF Eng FPV. DIY relay station design, good reference for analog relay hardware considerations.

## Tracker prior art — all require cooperative target (kept for context)
- [Automatic Antenna Tracking PTZ System – Xingkai Tech](https://xingkaitech.com/automatic-antenna-tracking-ptz-system-for-vtol-fix-wing-uav-airborne-aviation-tracking-antennas-for-long-range-communications/) — Commercial airborne PTZ antenna tracker requiring GPS lat/lon/alt of target. Architectural reference for the mechanical envelope; not applicable to passive case.
- [MFD Mini AAT Auto Tracking PTZ Antenna – UAVMODEL](https://www.uavmodel.com/products/mfd-mini-aat-automatic-tracking-antenna-tracking-ptz) — Compact ground-station PTZ tracker; relevant only for gimbal mechanism design.
- [ArduPilot AntennaTracker](https://ardupilot.org/antennatracker/) — Open-source MAVLink-driven tracker. Cooperative-target only.
- [open360tracker – GitHub](https://github.com/SamuelBrucksch/open360tracker) — Open-source 360° tracker, multi-protocol; cooperative-target only.

## Survey + airborne pointing
- [Design and Evaluation: Onboard Antenna Pointing Control for Wireless Relay UAV – MDPI Aerospace](https://www.mdpi.com/2226-4310/10/4/323) — Airborne relay antenna pointing on fixed-wing UAV; GPS-cooperative but useful for gimbal control loop discussion.
- [Recent Advancement in UAV Tracking Antenna – MDPI Sensors](https://www.mdpi.com/1424-8220/21/16/5662) — Survey of beam-steering methods (phased arrays, mechanical PTZ, switched beam, monopulse).

## Passive tracking — the monopulse path (new)
- [Skolnik, *Introduction to Radar Systems*, 3rd ed., Ch. 9 (Tracking Radar)] — Canonical reference for amplitude-comparison monopulse. The Δ/Σ formulation and squinted-feed geometry come from here.
- [Sherman & Barton, *Monopulse Principles and Techniques*, 2nd ed., 2011 (Artech House)] — Deeper monopulse text; covers amplitude vs phase monopulse, error-slope calibration, multipath handling.
- [Analog Devices AD8318 datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/AD8318.pdf) — 1 MHz–8 GHz log-detector, 60 dB dynamic range, ~10 ns response. The core of the monopulse front-end; native log output means Δ-in-dB falls out of a simple subtraction.
- [Mini-Circuits ZX10Q-2-12-S+ (or PD-0R413) Wilkinson power combiner](https://www.minicircuits.com/) — Reference part for the 4-way Σ combiner at 5.8 GHz; custom PCB Wilkinson is also viable.
- [Skyworks SKY13414-485LF SP4T RF switch datasheet](https://www.skyworksinc.com/Products/Switches/SKY13414-485LF) — Antenna switching between cluster Σ, finder, and omni paths.
- [Bosch BNO055 datasheet](https://www.bosch-sensortec.com/products/smart-sensor-systems/bno055/) — 9-DOF IMU with on-chip sensor fusion; magnetometer-aided absolute yaw is required to hold the cluster in world frame.

## EO sensor-fusion path (optional)
- [OpenMV H7 R2 / H7 Plus documentation](https://docs.openmv.io/) — Onboard CV camera platform; runs MicroPython, outputs bearing over UART. Suitable for tracking the FPV drone as a dark dot against sky.
- [Bar-Shalom, Li & Kirubarajan, *Estimation with Applications to Tracking and Navigation*, 2001] — Reference for Kalman/EKF fusion of RF AoA and EO bearing measurements.

## Polarization, filters, RF design
- [Balanis, *Antenna Theory: Analysis and Design*, 4th ed., Ch. 14 (CP patches)] — CP patch design at 5.8 GHz; truncated-corner and single-feed CP topologies.
- [Mini-Circuits cavity bandpass filters – 5.8 GHz](https://www.minicircuits.com/) — Reference parts for keeping 1.3 GHz TX harmonics out of the 5.8 GHz RX path.

## Background reading kept from earlier phase
- [OpenIPC Wiki – FPV](https://github.com/OpenIPC/wiki/blob/master/en/fpv.md) — Digital FPV with native MAVLink telemetry. Kept for reference; **not applicable** here since the emitter is non-cooperative and presents only analog video.
