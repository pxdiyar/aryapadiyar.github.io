---
title: Welcome
tags:
- Team 102
- Board B
---
<center>
<font size= "6">Arya Padiyar Datasheet</font><br>
as part of<br>
<font size= "8">Automatic Pill Dispenser</font><br>
for<br>
<font size= "5">Team 102</font><br>

<b>Submission: October 28, 2026 (External Design Review)</b>
</center>

## Introduction

This datasheet documents **Board B: Cap / Lid Sensing**, my subsystem of Team 102's Automatic Pill Dispenser for EGR 304 (Embedded Systems Design) at Arizona State University. It collects the block diagram, component selection, bill of materials, schematic and power budget for the board, so a reader can see what the board does, why each part was chosen, and how it connects to the rest of the dispenser.

### Project Summary

Missed doses and accidental double doses are a common problem for people managing daily medication. Team 102 (Arya Padiyar, Natalia Diaz, Samandar Kholmuminov and Taylor Calli) is building an automatic pill dispenser that presents the right dose at the right time and makes it hard to take an extra or early dose. A central hub board coordinates four subsystem boards, each built around a PIC18F57Q43 Curiosity Nano with its own sensor or actuator and analog signal chain: a carousel drive with jam detection, optical dose-confirmation sensing, audible alerts, and lid sensing. The boards share data over an 8-pin ribbon cable.

### My Contribution

I designed **Board B**, which tells the hub when the pill bottle cap is opened. A photoresistor under the cap is dark while the cap is closed; when the cap opens, light reaches it and the divider voltage rises. An MCP6004 op-amp chain amplifies and filters that signal for the PIC's ADC, and an LM393 comparator gives a clean open/closed signal. Board B reports LID_OPEN, LID_FAULT and a heartbeat to the hub, receives DOSE_READY, and also has a UART link. It runs from a 9 V wall adapter through an L7805CV regulator.

> **Rev 3 (October 27, 2026):** on the teaching team's recommendation, the original Hall-effect sensor and cap magnet were replaced by a photoresistor. Every page below has been updated to match.

## Datasheet Pages

| Page | What it covers |
|---|---|
| [Block Diagram](01-Block-Diagram/Block-Diagram.md) | Board B blocks, power domains, signals and the J1 ribbon pinout |
| [Component Selection](02-Component-Selection/Component-Selection.md) | Sensor, comparator, regulator and power-source options, pin table and MCC setup |
| [Bill of Materials](03-BOM/BOM.md) | Every part on the schematic with quantities and part numbers |
| [Schematic](04-Schematic/schematic.md) | Rev 3 KiCad schematic, design calculations, PDF and project download |
| [Power Budget](05-Power-Budget/Power-Budget.md) | Current on each rail, spreadsheet download and conclusions |
| [Appendix](Appendix/index.md) | Photoresistor lab measurements used in the design |
