---
title: Block Diagram
tags:
- block diagram
- Board B
---

## Overview

**Arya Padiyar — Team 102: Automatic Pill Dispenser**
**Subsystem: Board B — Cap / Lid Sensing**

This block diagram shows how my subsystem, Board B, is laid out and how it connects to the team's hub board.

- **Power levels:** A 9V unregulated wall adapter enters through a DC barrel jack. An L7805CV linear regulator steps it down to 5V, which powers the PIC18F57Q43 Curiosity Nano through VBUS. The Nano's on-board LDO supplies 3.3V to the sensor, comparator and all logic.
- **Sensor:**
