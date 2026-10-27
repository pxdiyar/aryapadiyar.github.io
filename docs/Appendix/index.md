---
title: Appendix
---

**Arya Padiyar | Team 102 | Board B: Cap / Lid Sensing**

## A. Photoresistor measurements (Resistive Sensor Lab)

These are the photoresistor readings from the Resistive Sensor Lab. Board B's divider resistor, comparator threshold and gain stage are all calculated from them.

| Light condition | Measured resistance | Divider output with R19 = 3.9 kΩ on 3.3 V |
|---|---|---|
| Fully covered (cap closed) | 46.3 kΩ | 0.26 V |
| Low indoor light | 11.07 kΩ | 0.86 V |
| Fully lit room | 1.54 kΩ | 2.37 V |
| Direct light / under a bulb | 66.5 Ω | 3.24 V |

**Divider resistor:** R = √(R_low × R_room) = √(11.07 kΩ × 1.54 kΩ) ≈ 4.1 kΩ → 3.9 kΩ standard value.

**Comparator threshold:** 3.3 V × 10 kΩ / (51 kΩ + 10 kΩ) ≈ 0.54 V, between the closed (0.26 V) and dimmest open (0.86 V) readings.

## B. Revision history

| Rev | Date | Change |
|---|---|---|
| 1 | Oct 5, 2026 | First block diagram (Hall-effect sensor + magnet) |
| 2 | Oct 19, 2026 | Schematic matched to the team block diagram; UART added on J1 pins 4 and 7 |
| 3 | Oct 27, 2026 | Hall-effect sensor replaced by a photoresistor (teaching-team recommendation); gain, VREF and threshold resistors recalculated; power budget completed |
