# Decisions — drone-relay-beam-tracking

## Mission constraints
- FPV is a non-cooperative military device — no GPS broadcast, no telemetry, no protocol negotiation
- All sensing is passive, from the FPV's 5.8 GHz video emission alone
- RX frequency: 5.8 GHz — fixed by the FPV's VTX
- TX frequency: 1.3 GHz — better range and ground penetration for downlink to pilot

## Tracking method — amplitude-comparison monopulse
- 4× CP patches arranged in a 2×2 cluster, each squinted ~15° outward from cluster boresight
- 4× AD8318 log-detectors → ESP32-S3 ADC
- Σ = A+B+C+D ; Δaz = (B+D)−(A+C) ; Δel = (A+B)−(C+D)
- Δ/Σ is the instantaneous angular error in each axis — no dither, no scan during track
- Σ doubles as RX into the 1.3 GHz relay TX chain (no separate RX path)

## Cold acquisition
- 5th wider-beam CP patch (~70° HPBW) co-mounted on the gimbal as a "finder"
- Coarse spiral over the downward hemisphere (6–8 cells, ~250 ms dwell each) until finder Σ > noise floor + 6 dB
- Hand off bearing to the monopulse cluster, which acquires in one sample
- Total cold-acquisition budget: <3 s

## Polarization
- All RX patches CP, sense matched to the FPV TX (assume RHCP unless intel says otherwise)
- Linear patches rejected — automatic 3 dB loss, 20–30 dB loss on wrong-sense CP

## EO sensor fusion (optional but recommended)
- OpenMV H7 camera bore-sighted with the patch cluster
- Computer vision tracks the FPV drone as a dark dot against sky
- Fused AoA: RF + EO agreement = high-confidence track; disagreement flags multipath/jamming
- Camera bearing reported over UART to the main MCU

## Gimbal
- 2× KST DS215MG metal-gear digital servos (pan and tilt — same part both axes)
- Pan range: ±135° via 270° servo; full coverage via relay-drone yaw when needed
- Tilt range mapped directly to 0° → −90° (servo endpoints, no off-center mounting)
- Servo bandwidth is the slow link; monopulse error update is essentially instantaneous

## Attitude reference
- BNO055 9-DOF IMU (gyro + accel + magnetometer, on-chip sensor fusion)
- Magnetometer required for absolute yaw — without it, the cluster drifts in world frame
- Fused quaternion read over I²C; world-frame target bearing → servo angles

## RF front-end
- Bandpass filter on each patch element to reject 1.3 GHz TX harmonics
- Σ channel: 4-way Wilkinson combiner → 5.8 GHz LNA → 1.3 GHz VTX input
- Physical shielding between TX and RX assemblies; antennas separated by ≥10 cm

## Loss recovery / link continuity
- Δ/Σ saturation or Σ below threshold for >500 ms → declare track lost
- Switch RX path from cluster Σ to omni "lollypop" via SPDT RF switch
- Re-enter acquisition mode while video continues on the omni

## Processing unit
- ESP32-S3 — 8 ADC channels, fast PWM, I²C, UART
- Sufficient compute for monopulse arithmetic (log-domain subtraction) and PID at >100 Hz

## Out of scope
- Phased-array electronic beam steering — too heavy/expensive for this add-on
- Phase-interferometry AoA — requires phase-coherent multi-RX chains, not feasible at this weight
- Modifying the FPV drone in any way
