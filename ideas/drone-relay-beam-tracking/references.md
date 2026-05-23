# References — drone-relay-beam-tracking

- [1.2GHz–1.3GHz to 5.8GHz Wireless Relay – Oscar Liang](https://oscarliang.com/1-3ghz-5-8ghz-relay/) — Oscar Liang. Comprehensive guide to the basic relay concept: 5.8GHz VRX out → 1.3GHz VTX in. Confirms latency is ~1–2ms, 20km+ range achieved.

- [A Simple 1.3 GHz Ground Relay Station – RF Eng FPV](https://rfengfpv.wordpress.com/2018/04/12/a-simple-1-3-ghz-ground-relay-station/) — RF Eng FPV. DIY relay station design, good reference for analog relay hardware considerations.

- [Automatic Antenna Tracking PTZ System – Xingkai Tech](https://xingkaitech.com/automatic-antenna-tracking-ptz-system-for-vtol-fix-wing-uav-airborne-aviation-tracking-antennas-for-long-range-communications/) — Xingkai Tech. Commercial airborne PTZ antenna tracker requiring GPS lat/lon/alt of target at every moment.

- [MFD Mini AAT Auto Tracking PTZ Antenna – UAVMODEL](https://www.uavmodel.com/products/mfd-mini-aat-automatic-tracking-antenna-tracking-ptz) — UAVMODEL. Compact commercial PTZ antenna tracker product; ground-station focused but relevant to mechanism design.

- [ArduPilot AntennaTracker](https://ardupilot.org/antennatracker/) — ArduPilot. Open-source antenna tracker firmware using MAVLink telemetry; drives pan/tilt servos from GPS position data. Could be adapted for airborne use.

- [open360tracker – GitHub](https://github.com/SamuelBrucksch/open360tracker) — Samuel Brucksch. Open source 360° continuous rotation antenna tracker supporting MAVLink, MultiWii, ArduPilot telemetry protocols.

- [Design and Evaluation: Onboard Antenna Pointing Control for Wireless Relay UAV – MDPI](https://www.mdpi.com/2226-4310/10/4/323) — MDPI Aerospace journal. Academic paper directly on airborne relay antenna pointing control system — most relevant academic reference found.

- [Recent Advancement in UAV Tracking Antenna – MDPI Sensors](https://www.mdpi.com/1424-8220/21/16/5662) — MDPI Sensors. Review paper covering state of the art in UAV tracking antennas, beam steering methods (phased arrays, mechanical PTZ, switched beam).

- [OpenIPC Wiki – FPV](https://github.com/OpenIPC/wiki/blob/master/en/fpv.md) — OpenIPC. Open firmware for IP cameras used as FPV systems; integrates MAVLink telemetry output natively — a potential position-data source for the tracker.
