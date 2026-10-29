---
title: Bill of Materials
tags:
- BOM
- Board B
---

**Arya Padiyar | Team 102 | Board B: Cap / Lid Sensing | Rev 3**

## Overview

This is the bill of materials for Board B, generated from the Rev 3 [schematic](../04-Schematic/schematic.md) (KiCad *Generate BOM*, grouped by value) and checked against the [block diagram](../01-Block-Diagram/Block-Diagram.md) and [component selection](../02-Component-Selection/Component-Selection.md). Reference designators match the schematic. Every part on the board is listed, including the ones I'm taking from Peralta 109 stock or the course kit.

The table is sorted by where each part comes from. Only four parts have to be bought; everything else (passives, LEDs, headers, jumpers, test points, IC sockets, the barrel jack, fuse and 1N5819) is in Peralta 109 general stock, and the Curiosity Nano is from the course kit.

## Bill of Materials

*Table 1: Board B bill of materials (Rev 3). Prices are Mouser / Adafruit single-quantity prices checked on October 29, 2026.*

| **Part / Description** | **Qty on Board** | **Qty to Order** | **Unit Cost** | **Total Cost** | **Manufacturer** | **Manufacturer #** | **Source / Vendor Link** | **Datasheet** | **Schematic Reference Designators** |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| 5 V linear regulator, 1.5 A, TO-220 | 1 | 3 | $1.08 | $3.24 | STMicroelectronics | L7805CV | [Mouser](https://www.mouser.com/ProductDetail/511-L7805CV) | [PDF](https://www.st.com/resource/en/datasheet/l78.pdf) | U3 |
| Dual voltage comparator, DIP-8 | 1 | 4 | $0.53 | $2.12 | Texas Instruments | LM393P | [Mouser](https://www.mouser.com/ProductDetail/595-LM393P) | [PDF](https://www.ti.com/lit/ds/symlink/lm393.pdf) | U5 |
| CdS photoresistor, 5 mm ceramic (light-lab part) | 1 | 4 | $1.42 | $5.68 | Advanced Photonix | PDV-P8001 | [Mouser](https://www.mouser.com/ProductDetail/846-PDV-P8001) | [PDF](https://faculty-web.msoe.edu/prust/EE2920/EE2920Resources/PDV-P8001.pdf) | R18 |
| 9 V 1 A regulated wall adapter, 5.5/2.1 mm plug | 1 | 1 | $8.95 | $8.95 | Adafruit | 63 | [Adafruit](https://www.adafruit.com/product/63) | — | (plugs into J2) |
| Microcontroller board, PIC18F57Q43 Curiosity Nano | 1 | — | — | — | Microchip | DM164150 | Course kit | [Product page](https://www.microchip.com/en-us/development-tool/DM164150) | U1 |
| Quad op-amp, rail-to-rail, DIP-14 | 1 | — | — | — | Microchip | MCP6004-I/P | Peralta 109 stock | [Product page](https://www.microchip.com/en-us/product/MCP6004) | U2 |
| DC barrel jack, 5.5/2.1 mm | 1 | — | — | — | Same Sky | PJ-102AH | Peralta 109 stock | — | J2 |
| Fuse, 500 mA 250 V slow-blow, TR5 + socket | 1 | — | — | — | Littelfuse | TR5 series | Peralta 109 stock | — | F1 |
| Schottky diode, 40 V 1 A, DO-41 | 1 | — | — | — | — | 1N5819 | Peralta 109 stock | — | D1 |
| LED, green (power good), 0805 | 1 | — | — | — | — | — | Peralta 109 stock | — | D2 |
| LED, yellow (heartbeat), 0805 | 1 | — | — | — | — | — | Peralta 109 stock | — | D3 |
| LED, red (lid status), 0805 | 1 | — | — | — | — | — | Peralta 109 stock | — | D4 |
| Capacitor 0.1 µF 16 V, 0805 | 8 | — | — | — | — | — | Peralta 109 stock | — | C2, C4, C5, C6, C8, C9, C10, C11 |
| Capacitor 0.33 µF 25 V, 0805 | 1 | — | — | — | — | — | Peralta 109 stock | — | C1 |
| Capacitor 1 µF 16 V, 0805 | 1 | — | — | — | — | — | Peralta 109 stock | — | C7 |
| Electrolytic capacitor 10 µF 25 V, radial 5 mm | 1 | — | — | — | — | — | Peralta 109 stock | — | C3 |
| Resistor 10 kΩ 1/8 W, 0805 | 10 | — | — | — | — | — | Peralta 109 stock | — | R2, R4, R6, R9, R11, R13, R14, R15, R16, R17 |
| Resistor 3.9 kΩ 1/8 W, 0805 (LDR divider) | 1 | — | — | — | — | — | Peralta 109 stock | — | R19 |
| Resistor 3.3 kΩ 1/8 W, 0805 (gain) | 1 | — | — | — | — | — | Peralta 109 stock | — | R3 |
| Resistor 120 kΩ 1/8 W, 0805 (VREF) | 1 | — | — | — | — | — | Peralta 109 stock | — | R8 |
| Resistor 51 kΩ 1/8 W, 0805 (threshold) | 1 | — | — | — | — | — | Peralta 109 stock | — | R10 |
| Resistor 470 kΩ 1/8 W, 0805 (hysteresis) | 1 | — | — | — | — | — | Peralta 109 stock | — | R12 |
| Resistor 1 kΩ 1/8 W, 0805 | 1 | — | — | — | — | — | Peralta 109 stock | — | R1 |
| Resistor 330 Ω 1/8 W, 0805 | 2 | — | — | — | — | — | Peralta 109 stock | — | R5, R7 |
| Tactile push button, 6 mm THT | 1 | — | — | — | — | — | Peralta 109 stock | — | SW1 |
| IDC header 2×4, 2.54 mm (ribbon to hub) | 1 | — | — | — | — | — | Peralta 109 stock | — | J1 |
| Pin header 1×4, 2.54 mm | 1 | — | — | — | — | — | Peralta 109 stock | — | J3 |
| Pin header 1×3, 2.54 mm (remote LDR) | 1 | — | — | — | — | — | Peralta 109 stock | — | J4 |
| Pin header 1×8, 2.54 mm (expansion) | 3 | — | — | — | — | — | Peralta 109 stock | — | J5, J6, J7 |
| Pin header 1×2 + jumper shunt | 3 | — | — | — | — | — | Peralta 109 stock | — | JP1, JP2, JP3 |
| Test point, THT pad 2.0 mm | 9 | — | — | — | — | — | Peralta 109 stock | — | TP1–TP9 |
| IC socket, DIP-8 (for U5 on the PCB) | 1 | — | — | — | — | — | Peralta 109 stock | — | U5 socket |
| IC socket, DIP-14 (for U2 on the PCB) | 1 | — | — | — | — | — | Peralta 109 stock | — | U2 socket |

**Total to order (before tax and shipping):**

- Mouser: $11.04
- Adafruit: $8.95
- **Total: $19.99**

Download the spreadsheet version: [BoardB_BOM.xlsx](BoardB_BOM.xlsx)

## Order quantities

Following the course ordering rules, each ordered part covers **one breadboard prototype, one subsystem PCB, and one or two spares** in case a part gets damaged:

| Part | Breadboard | PCB | Spares | Ordered |
|:---|:---|:---|:---|:---|
| L7805CV regulator | 1 | 1 | 1 | 3 |
| LM393P comparator | 1 | 1 | 2 | 4 |
| PDV-P8001 photoresistor | 1 | 1 | 2 | 4 |
| Adafruit #63 9 V adapter | shared | shared | 0 | 1 |

All three ICs are through-hole (TO-220 / DIP-8 / 5 mm), so the same part goes on the breadboard and on the PCB, and nothing ordered is smaller than 0805. The wall adapter just plugs into J2, so one adapter covers both builds. No breakout modules, tools or general lab supplies are being requested.

## Hardware orders

One purchase request per vendor was made from the class order form template and emailed to Zane Reynolds as .xlsx files for approval:

| Vendor | Items | Subtotal | Stock when checked (Oct 29) | Purchase request |
|:---|:---|:---|:---|:---|
| Mouser | 3× 511-L7805CV, 4× 595-LM393P, 4× 846-PDV-P8001 | $11.04 | 576 / 26,940 / 304 in stock | [PDF](AryaPadiyar_Team102_PurchaseRequest_Mouser.pdf) |
| Adafruit | 1× #63 9 VDC 1 A power adapter | $8.95 | 84 in stock | [PDF](AryaPadiyar_Team102_PurchaseRequest_Adafruit.pdf) |

The adapter is ordered straight from Adafruit because Mouser only had one unit in stock. Everything was in stock and ships immediately, so nothing on the order is back-ordered or obsolete.

## Debugging and expandability hardware

These parts are on the board only to make it easier to test, debug and extend:

- **Test points TP1–TP9** on every supply rail and each analog stage (divider, amplifier, filter, comparator).
- **Jumpers JP1–JP3** to isolate the 5 V feed into the Nano, the photoresistor, and the comparator output.
- **Headers:** J3 (spare 9 V / 5 V / 3.3 V / GND), J4 (lets the photoresistor sit next to the cap on wires), J5–J7 (unused PIC pins for expansion). 1×40 header strips from Peralta will be snapped to length.
- **IC sockets** for U2 and U5 so the op-amp and comparator can be swapped without desoldering.
- **Fuse F1 and diode D1** protect the board from shorts and a reversed adapter.
- **Heartbeat LED D3 and debug button SW1** to confirm the firmware is running.
