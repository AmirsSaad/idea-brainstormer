# Drone Video Relay Add-On with Beam-Tracking Antenna
Date: 2026-05-23
Updated: 2026-05-23 (design pivot to amplitude-comparison monopulse)

## Summary
A relay add-on module mounted on a drone that receives FPV video at 5.8 GHz from a
non-cooperative military FPV drone below it, and retransmits at 1.3 GHz to a ground
pilot. The FPV emits only its 5.8 GHz video — no GPS, no telemetry, no protocol
cooperation. All tracking is passive.

Beam tracking uses **amplitude-comparison monopulse** on a 2×2 cluster of CP patches,
co-mounted on a 2-axis gimbal with a wide-beam "finder" patch for cold acquisition and
an optional bore-sighted EO camera for sensor fusion. An omnidirectional CP "lollypop"
remains in the design as link-continuity fallback when track is lost.

## Operating geometry
- Relay drone hovers 100–300 m above the FPV drone
- FPV operates up to 1 km laterally
- Patch cluster looks downward; look angles span 0° to –90° below horizon
- At 1 km lateral / 100 m altitude → ~84° below horizon — within the cluster's beam

## Why monopulse, not RSSI hill-climb (previous design)
The earlier RSSI-hill-climb plan was abandoned because:
- Single-antenna RSSI cannot tell direction; you must dither off-axis to estimate a
  gradient, which degrades the very link you are trying to maintain.
- Over a 1 km path at 5.8 GHz with a moving emitter, Rayleigh-style multipath fades
  of 10–20 dB occur on 100 ms timescales — indistinguishable from "off-beam" to a
  hill-climber. Phantom maxima dominate.
- VRX RSSI pins are slow, noisy, and AGC-dependent — wrong sensor for a tight loop.

Monopulse fixes all three:
- Direction is in the geometry of the cluster, not in scanning behaviour.
- Δ/Σ is a **ratio**; common-mode fades cancel.
- The angular error appears in **one sample**.

## Tracking method (one-line summary)
4× squinted CP patches → 4× AD8318 log-detectors → ESP32-S3 ADC computes Σ and two
Δ channels. Δ/Σ feeds a 2-axis PID that drives the gimbal. Wide-beam finder patch
covers cold acquisition. EO camera (OpenMV H7) provides redundant AoA. Lollypop omni
is the link-loss fallback.

## Constraints
- FPV is non-cooperative; no GPS, no telemetry — passive RF and EO only
- Polarization matched to the FPV TX (assume RHCP unless intel says otherwise)
- 1.3 GHz TX harmonics must be kept off the 5.8 GHz RX path (filters + shielding)
- All-up add-on weight target: <180 g

## Research findings (phase 3, preserved)

### What already exists
- Basic 5.8→1.3 GHz relay is a solved FPV technique (Oscar Liang, RF Eng FPV).
  Hardware: VRX video out wired to VTX in. ~1–2 ms added latency.
- PTZ antenna trackers exist commercially (MFD Mini AAT, Foxtech Archer, Xingkai
  XK-FT300) but are ALL designed for ground stations pointing UP, and ALL require
  GPS/MAVLink of the target. None apply to the no-cooperation case.
- ArduPilot AntennaTracker and open360tracker exist but assume MAVLink telemetry —
  inapplicable here.
- One MDPI paper directly addresses airborne relay antenna pointing on fixed-wing
  UAVs (also GPS-dependent).
- Monopulse and amplitude-comparison direction finding are mature techniques from
  radar/ESM; downscaling to drone weight is the engineering work.

### Key geometry insight
Relay is ABOVE the FPV drone. Most PTZ trackers expect elevation 0–90° (looking up).
Here the patch looks mostly downward; elevation range is 0° (horizontal) to –90°
(straight down). Tilt-axis mounting maps servo endpoints directly to this range.

### Position-data challenge (resolved)
Tracking requires the FPV drone's bearing. Three paths existed: telemetry link,
RSSI scanning, embedded OSD telemetry. None apply: the emitter is non-cooperative
and no telemetry is broadcast. The new design extracts bearing from the 5.8 GHz
emission itself, via the monopulse cluster.

## Reflected summary (revised)
1. Omni-only relay gave insufficient link quality — patch gain needed at oblique angles.
2. Relay hovers 100–300 m above FPV drone; FPV can be up to 1 km laterally.
3. Patch cluster actively tracks FPV bearing via amplitude-comparison monopulse.
4. Lollypop kept as omni fallback for link continuity during acquisition and loss recovery.
5. Optional EO camera provides redundant AoA and detects multipath/jamming via
   RF/EO disagreement.
