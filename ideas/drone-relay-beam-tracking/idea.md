# Drone Video Relay Add-On with Beam-Tracking Antenna
Date: 2026-05-23

## Summary
A relay add-on module for drones that receives video at 5.8 GHz and retransmits at 1.3 GHz. Key feature is beam-tracking capability using two RX antennas: an omnidirectional "lollypop" antenna and a directional patch antenna mounted on a PTZ mechanism for active beam steering.

## Research Findings (Phase 3)

### What already exists
- Basic 5.8→1.3GHz relay is a solved, well-documented FPV technique (Oscar Liang, RF Eng FPV). Hardware: VRX audio/video out wired to VTX in. ~1–2ms added latency.
- PTZ antenna trackers exist commercially (MFD Mini AAT, Foxtech Archer, Xingkai XK-FT300) but are ALL designed for ground stations pointing UP. None designed for airborne-above-target geometry.
- ArduPilot AntennaTracker + open360tracker are open-source options using MAVLink telemetry to calculate pan/tilt angles from GPS positions.
- One MDPI academic paper directly addresses airborne relay antenna pointing control on fixed-wing UAVs — closest academic reference.
- OpenIPC/WFB-ng digital FPV stack has native MAVLink telemetry integration.

### Key geometry insight
Relay is ABOVE the FPV drone. Most PTZ trackers expect elevation 0–90° (looking up). Here the patch looks mostly downward; elevation range is 0° (horizontal) to -90° (straight down). This inverts the PTZ tilt axis and affects gimbal mechanical design significantly.

### Position data challenge
All tracking approaches require knowing the FPV drone's GPS position in real time. Three paths: (1) telemetry link from FPV drone to relay drone, (2) RSSI-based scanning/triangulation (no GPS needed), (3) embedded OSD telemetry decoded from the received video.

## Reflected Summary (confirmed by user)
1. Omni-only relay gave insufficient link quality — not enough gain at oblique angles when FPV drone is far away.
2. Relay hovers 100–300 m above FPV drone; FPV can be up to 1 km laterally — patch must cover a wide downward cone.
3. PTZ-mounted patch actively tracks FPV drone position, steering beam for high-gain lock.
4. Lollypop kept as omni fallback; patch is primary high-gain RX when beam lock is achieved.
5. Core challenge: relay must know FPV drone position at all times to point the patch.
