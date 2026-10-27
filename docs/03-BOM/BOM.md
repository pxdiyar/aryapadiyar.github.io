---
title: Bill of Materials
tags:
- BOM
- Board B
---

**Arya Padiyar | Team 102 | Board B: Cap / Lid Sensing | Rev 3**

## Overview

This is the bill of materials for Board B, taken directly from the Rev 3 [schematic](../04-Schematic/schematic.md). Reference designators match the schematic. Unit costs are listed for the parts priced in [Component Selection](../02-Component-Selection/Component-Selection.md); passives, headers and test points come from Peralta lab stock.

## Bill of Materials

*Table 1: Board B bill of materials (Rev 3).*

| **Part / Description** | **Qty** | **Unit Cost** | **Manufacturer** | **Manufacturer #** | **Vendor / Datasheet** | **Schematic Reference Designators** |
|:---|:---|:---|:---|:---|:---|:---|
| Microcontroller board, PIC18F57Q43 Curiosity Nano | 1 | Course kit | Microchip | DM164150 | [Microchip](https://www.microchip.com/en-us/development-tool/DM164150) | U1 |
| Quad op-amp, rail-to-rail, DIP-14 | 1 | Lab stock | Microchip | MCP6004-I/P | [Mouser](https://www.mouser.com/c/?q=MCP6004-I%2FP) | U2 |
| 5 V linear regulator, 1.5 A, TO-220 | 1 | $0.60 | STMicroelectronics | L7805CV | [Mouser](https://www.mouser.com/ProductDetail/511-L7805CV) | U3 |
| Dual voltage comparator, DIP-8 | 1 | $0.53 | Texas Instruments | LM393P | [Mouser](https://www.mouser.com/ProductDetail/595-LM393P) | U5 |
| CdS photoresistor, 5 mm (light-lab part) | 1 | $1.42 | Advanced Photonix (equivalent) | PDV-P8001 | [Mouser](https://www.mouser.com/ProductDetail/846-PDV-P8001) | R18 |
| 9 V 1 A regulated wall adapter, 5.5/2.1 mm plug | 1 | $8.95 | Adafruit | #63 | [Adafruit](https://www.adafruit.com/product/63) | (plugs into J2) |
| DC barrel jack, 5.5/2.1 mm | 1 | Lab stock | Same Sky | PJ-102AH | — | J2 |
| Fuse, 500 mA 250 V slow-blow, TR5 + socket | 1 | Lab stock | Littelfuse | TR5 series | — | F1 |
| Schottky diode, 40 V 1 A, DO-41 | 1 | Lab stock | — | 1N5819 | — | D1 |
| LED, green (power good), 0805 | 1 | Lab stock | — | — | — | D2 |
| LED, yellow (heartbeat), 0805 | 1 | Lab stock | — | — | — | D3 |
| LED, red (lid status), 0805 | 1 | Lab stock | — | — | — | D4 |
| Capacitor 0.1 µF 16 V, 0805 | 8 | Lab stock | — | — | — | C2, C4, C5, C6, C8, C9, C10, C11 |
| Capacitor 0.33 µF 25 V, 0805 | 1 | Lab stock | — | — | — | C1 |
| Capacitor 1 µF 16 V, 0805 | 1 | Lab stock | — | — | — | C7 |
| Electrolytic capacitor 10 µF 25 V, radial 5 mm | 1 | Lab stock | — | — | — | C3 |
| Resistor 10 kΩ 1/8 W, 0805 | 10 | Lab stock | — | — | — | R2, R4, R6, R9, R11, R13, R14, R15, R16, R17 |
| Resistor 3.9 kΩ 1/8 W, 0805 (LDR divider) | 1 | Lab stock | — | — | — | R19 |
| Resistor 3.3 kΩ 1/8 W, 0805 (gain) | 1 | Lab stock | — | — | — | R3 |
| Resistor 120 kΩ 1/8 W, 0805 (VREF) | 1 | Lab stock | — | — | — | R8 |
| Resistor 51 kΩ 1/8 W, 0805 (threshold) | 1 | Lab stock | — | — | — | R10 |
| Resistor 470 kΩ 1/8 W, 0805 (hysteresis) | 1 | Lab stock | — | — | — | R12 |
| Resistor 1 kΩ 1/8 W, 0805 | 1 | Lab stock | — | — | — | R1 |
| Resistor 330 Ω 1/8 W, 0805 | 2 | Lab stock | — | — | — | R5, R7 |
| Tactile push button, 6 mm THT | 1 | Lab stock | — | — | — | SW1 |
| IDC header 2×4, 2.54 mm (ribbon to hub) | 1 | Lab stock | — | — | — | J1 |
| Pin header 1×4, 2.54 mm | 1 | Lab stock | — | — | — | J3 |
| Pin header 1×3, 2.54 mm (remote LDR) | 1 | Lab stock | — | — | — | J4 |
| Pin header 1×8, 2.54 mm (expansion) | 3 | Lab stock | — | — | — | J5, J6, J7 |
| Pin header 1×2 + jumper shunt | 3 | Lab stock | — | — | — | JP1, JP2, JP3 |
| Test point, THT pad 2.0 mm | 9 | Lab stock | — | — | — | TP1–TP9 |

**Purchased parts total:** $11.50 (L7805CV + LM393P + photoresistor + wall adapter; the course kit and lab-stock parts are not included).
