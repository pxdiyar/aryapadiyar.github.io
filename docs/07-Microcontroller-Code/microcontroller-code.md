---
title: Microcontroller Code
---

**Arya Padiyar | Team 102 | Board B: Cap / Lid Sensing | Rev 4A (as built)**

## Overview

This is the firmware I used to verify the Board B hardware for the Subsystem Hardware Verification assignment. It runs on the PIC18F57Q43 Curiosity Nano plugged into my Board B PCB, and it was built in MPLAB X IDE v6.35 with XC8 v3.10 and programmed over USB through the Nano's on-board debugger.

The code is written directly against the PIC registers (no MCC), so every pin and peripheral setting is visible in `main.c`.

**Download:** [MPLAB X project (packaged .zip)](AryaPadiyar_BoardB_Firmware.zip)

## What the code verifies

| Verification item | How it is shown |
|---|---|
| Microcontroller programmability | The project builds and programs over USB from MPLAB X ("Make and Program Device") |
| Board is running | Yellow heartbeat LED D3 (RF2) and HEARTBEAT_B (RD4, ribbon pin 5) toggle at 1 Hz |
| LEDs work | Power-up self test: D3, D4 and the Nano's LED0 all turn on for 1 s |
| Pre-amplified sensor signal | Photoresistor divider (LDR_OUT) measured with a DMM: dark vs. light |
| Post-amplified signal | MCP6004 buffer output (BUF_OUT / ADC_IN) measured with a DMM |
| Microcontroller reads the amplified signal | ADC on RA0 (ANA0) converts the light level to mV and prints it over the Nano's USB serial port every 0.5 s |
| Comparator | LM393 output read on RB0 and cross-checked against the ADC reading |
| Ribbon GPIO (hub link) | LID_OPEN (RD1, pin 2), LID_FAULT (RD2, pin 3) and HEARTBEAT_B (RD4, pin 5) are driven; DOSE_READY (RD0, pin 1) is read and printed; UART1 (RC2 TX pin 7, RC3 RX pin 4) sends `LID:x` and `FAULT:x` messages; pin 8 is ground |

## Pin configuration

| Pin | Direction | Signal | Ribbon J1 pin |
|---|---|---|---|
| RA0 (ANA0) | Analog in | ADC_IN: buffered, filtered light level | — |
| RB0 | In | CMP_OUT: LM393 output (1 = cap open) | — |
| RD3 | In | BTN_DBG: debug button SW1 | — |
| RD0 | In | DOSE_READY from hub | 1 |
| RD1 | Out | LID_OPEN to hub | 2 |
| RD2 | Out | LID_FAULT to hub | 3 |
| RC3 | UART1 RX | UART_RX from hub | 4 |
| RD4 | Out | HEARTBEAT_B to hub | 5 |
| RC2 | UART1 TX | UART_TX to hub | 7 |
| RC5 | Out | D4 red lid LED | — |
| RF2 | Out | D3 yellow heartbeat LED | — |
| RF3 | Out | Nano LED0 (active low), debug | — |
| RF0 / RF1 | UART3 TX / RX | Nano USB serial (debug output) | — |
| GND | — | Ground | 8 |

Clock: internal HFINTOSC at 64 MHz. ADC: 12-bit, FRC clock, VDD reference, right-justified. UART1 and UART3: 9600 baud, 8-N-1.

## How it works

1. **Self test:** at power-up D3, D4 and LED0 light together for 1 second.
2. **Main loop (every 10 ms):**
    - reads the light level on RA0 and converts it to millivolts;
    - decides whether the cap is open from the light level, with hysteresis (open above 650 mV, closed below 500 mV), confirmed over 20 ms so it cannot flicker;
    - reads the LM393 comparator on RB0 as a second, independent check;
    - drives D4 and LID_OPEN high while the cap is open, and flashes the Nano's LED0 as a debug indicator while open;
    - raises LID_FAULT if the comparator disagrees with the light reading for more than 1 second, or if the light reading is about 0 V (sensor disconnected);
    - toggles the D3 heartbeat and HEARTBEAT_B at 1 Hz;
    - sends `LID:0/1` and `FAULT:0/1` to the hub over UART1 whenever they change;
    - prints a status line in this format over the Nano's USB serial port every 0.5 s (view it in MPLAB Data Visualizer at 9600 baud):

```
light=<mV> mV  lid=OPEN|CLOSED  fault=<0|1>  btn=<0|1>  dose=<0|1>
```

**Note on SW1:** on my assembled board the PIC reads the debug button as permanently pressed (most likely SW1 is rotated 90° on its footprint). The firmware therefore ignores SW1 for now; it still prints its state (`btn=`) for debugging.

## Source code (main.c)

```c
/*
 * Board B - Cap / Lid Sensing   |   Team 102, EGR 304
 * PIC18F57Q43 Curiosity Nano, XC8 (no MCC)
 *
 * Pins (from the Board B schematic pin table):
 *   RA0  in   ADC_IN       buffered, filtered light level (MCP6004 U2B)
 *   RB0  in   CMP_OUT      LM393 output, 10k pull-up, 1 = cap open
 *   RD3  in   BTN_DBG      debug button SW1, 10k pull-up, 0 = pressed
 *   RD0  in   DOSE_READY   from hub (J1 pin 1)
 *   RC5  out  LED_LID      red lid LED D4
 *   RF2  out  LED_DBG      yellow heartbeat LED D3
 *   RD1  out  LID_OPEN     to hub (J1 pin 2)
 *   RD2  out  LID_FAULT    to hub (J1 pin 3)
 *   RD4  out  HEARTBEAT_B  to hub (J1 pin 5)
 *   RC2/RC3   UART1 TX/RX  to hub (J1 pins 7/4), 9600 8-N-1
 *   RF0/RF1   UART3 TX/RX  Nano USB serial (debug), 9600 8-N-1
 *   RF3  out  Nano LED0    (active low) debug: flashes while cap open
 *
 * Behaviour:
 *   Power-up self test: D3 + D4 + LED0 on together for 1 s.
 *   D3 / HEARTBEAT_B: 1 Hz heartbeat (on 0.5 s, off 0.5 s).
 *   Lid state = light reading on RA0 with hysteresis (open > 650 mV,
 *              closed < 500 mV), confirmed over 20 ms. Fast response.
 *   Nano LED0: flashes fast (debug) while the cap is open, off when closed.
 *   D4 + LID_OPEN: on while the cap is open.
 *   LID_FAULT: on if the comparator disagrees with the ADC for > 1 s,
 *              or if the light reading is ~0 V (sensor disconnected).
 *   SW1: ignored for now (reads as always pressed on this board).
 *   Hub UART1: sends "LID:0"/"LID:1" on every change and "FAULT:x".
 *   Debug UART3: prints a status line every 0.5 s.
 */

#pragma config FEXTOSC = OFF, RSTOSC = HFINTOSC_64MHZ
#pragma config CLKOUTEN = OFF, PR1WAY = OFF, CSWEN = ON, FCMEN = OFF
#pragma config MCLRE = EXTMCLR, PWRTS = PWRT_OFF, MVECEN = OFF, IVT1WAY = OFF
#pragma config LPBOREN = OFF, BOREN = SBORDIS, BORV = VBOR_1P9, ZCD = OFF
#pragma config PPS1WAY = OFF, STVREN = ON, LVP = ON, XINST = OFF
#pragma config WDTCPS = WDTCPS_31, WDTE = OFF
#pragma config WDTCWS = WDTCWS_7, WDTCCS = SC
#pragma config BBSIZE = BBSIZE_512, BBEN = OFF, SAFEN = OFF, DEBUG = OFF
#pragma config WRTB = OFF, WRTC = OFF, WRTD = OFF, WRTSAF = OFF, WRTAPP = OFF
#pragma config CP = OFF

#include <xc.h>
#include <stdio.h>
#include <stdint.h>

#define _XTAL_FREQ 64000000UL

/* Thresholds on the ADC (after the x1.33 gain stage) */
#define MV_OPEN_SURE    700u   /* clearly light: cap open   */
#define MV_CLOSED_SURE  400u   /* clearly dark: cap closed  */
#define MV_DISCONNECTED  30u   /* basically 0 V: sensor off */
#define MV_LID_OPEN     650u   /* ADC above this -> cap open   */
#define MV_LID_CLOSED   500u   /* ADC below this -> cap closed */

#define TICK_MS          10u

/* ---------- UARTs ---------- */
static uint8_t to_hub = 0;          /* 0 = putch goes to USB debug, 1 = to hub */

void putch(char c)
{
    if (to_hub) { while (U1FIFObits.TXBF) { } U1TXB = c; }
    else        { while (U3FIFObits.TXBF) { } U3TXB = c; }
}

static void uart_init(void)
{
    /* UART3 -> Nano USB serial: TX RF0, RX RF1 */
    ANSELFbits.ANSELF0 = 0;  TRISFbits.TRISF0 = 0;
    ANSELFbits.ANSELF1 = 0;  TRISFbits.TRISF1 = 1;
    RF0PPS  = 0x26;          /* U3TX */
    U3RXPPS = 0x29;          /* RF1  */
    U3BRGH = 0x01;  U3BRGL = 0xA0;   /* 9600 @ 64 MHz */
    U3CON0 = 0x30;  U3CON2 = 0x00;  U3CON1 = 0x80;

    /* UART1 -> hub: TX RC2, RX RC3 */
    ANSELCbits.ANSELC2 = 0;  TRISCbits.TRISC2 = 0;
    ANSELCbits.ANSELC3 = 0;  TRISCbits.TRISC3 = 1;
    RC2PPS  = 0x20;          /* U1TX */
    U1RXPPS = 0x13;          /* RC3  */
    U1BRGH = 0x01;  U1BRGL = 0xA0;
    U1CON0 = 0x30;  U1CON2 = 0x00;  U1CON1 = 0x80;
}

/* ---------- ADC ---------- */
static void adc_init(void)
{
    ANSELAbits.ANSELA0 = 1;  TRISAbits.TRISA0 = 1;
    ADREF  = 0x00;           /* VDD / VSS */
    ADPCH  = 0x00;           /* ANA0 */
    ADACQ  = 0x20;
    ADCON0 = 0x00;
    ADCON0bits.CS = 1;       /* FRC */
    ADCON0bits.FM = 1;       /* right justified */
    ADCON0bits.ON = 1;
}

static uint16_t adc_mv(void)
{
    ADCON0bits.GO = 1;
    while (ADCON0bits.GO) { }
    uint16_t raw = ((uint16_t)ADRESH << 8) | ADRESL;
    return (uint16_t)(((uint32_t)raw * 3300UL) / 4095UL);
}

/* ---------- GPIO ---------- */
static void io_init(void)
{
    ANSELBbits.ANSELB0 = 0;  TRISBbits.TRISB0 = 1;   /* CMP_OUT    */
    ANSELDbits.ANSELD3 = 0;  TRISDbits.TRISD3 = 1;   /* SW1        */
    ANSELDbits.ANSELD0 = 0;  TRISDbits.TRISD0 = 1;   /* DOSE_READY */

    LATCbits.LATC5 = 0;  ANSELCbits.ANSELC5 = 0;  TRISCbits.TRISC5 = 0;  /* D4 */
    LATFbits.LATF2 = 0;  ANSELFbits.ANSELF2 = 0;  TRISFbits.TRISF2 = 0;  /* D3 */
    LATFbits.LATF3 = 1;  ANSELFbits.ANSELF3 = 0;  TRISFbits.TRISF3 = 0;  /* LED0 off */
    LATDbits.LATD1 = 0;  ANSELDbits.ANSELD1 = 0;  TRISDbits.TRISD1 = 0;  /* LID_OPEN */
    LATDbits.LATD2 = 0;  ANSELDbits.ANSELD2 = 0;  TRISDbits.TRISD2 = 0;  /* LID_FAULT */
    LATDbits.LATD4 = 0;  ANSELDbits.ANSELD4 = 0;  TRISDbits.TRISD4 = 0;  /* HEARTBEAT_B */
}

static void self_test(void)
{
    LATFbits.LATF2 = 1;  LATCbits.LATC5 = 1;  LATFbits.LATF3 = 0;
    __delay_ms(1000);
    LATFbits.LATF2 = 0;  LATCbits.LATC5 = 0;  LATFbits.LATF3 = 1;
}

void main(void)
{
    io_init();
    adc_init();
    uart_init();
    self_test();

    printf("\r\nBoard B firmware - Team 102 (lid sensing)\r\n");

    uint16_t t_ms        = 0;   /* free-running ms counter          */
    uint8_t  lid_open    = 0;
    uint8_t  last_raw    = lid_open;
    uint8_t  stable_cnt  = 0;
    uint16_t disagree_ms = 0;
    uint8_t  fault       = 0, last_fault = 0, last_lid = 0xFF;
    uint16_t mv          = 0;

    while (1)
    {
        /* --- read inputs --- */
        mv = adc_mv();
        uint8_t raw_cmp = PORTBbits.RB0;
        uint8_t btn     = !PORTDbits.RD3;          /* 1 = pressed */

        /* --- lid state from the light reading (ADC), with hysteresis,
         *     confirmed over 2 ticks (20 ms). The comparator is kept as
         *     a cross-check for LID_FAULT. --- */
        uint8_t raw_lid = lid_open;
        if (mv > MV_LID_OPEN)        raw_lid = 1;
        else if (mv < MV_LID_CLOSED) raw_lid = 0;
        if (raw_lid == last_raw) { if (stable_cnt < 2) stable_cnt++; }
        else                     { stable_cnt = 0; last_raw = raw_lid; }
        if (stable_cnt >= 2) lid_open = raw_lid;

        /* --- fault: ADC and comparator disagree for > 1 s, or sensor off --- */
        uint8_t disagree = (raw_cmp != lid_open);
        if (disagree) { if (disagree_ms < 2000) disagree_ms += TICK_MS; }
        else          { disagree_ms = 0; }
        fault = (disagree_ms > 1000) || (mv < MV_DISCONNECTED);

        /* --- outputs --- */
        uint8_t beat = (t_ms % 1000) < 500;        /* 1 Hz heartbeat */
        LATFbits.LATF2 = beat;
        LATDbits.LATD4 = beat;

        LATCbits.LATC5 = lid_open;                 /* D4: on while cap open (SW1 ignored) */

        LATDbits.LATD1 = lid_open;
        LATDbits.LATD2 = fault;
        LATFbits.LATF3 = (lid_open && ((t_ms % 200) < 100)) ? 0 : 1;  /* Nano LED0: fast debug flash while open */

        /* --- messages to hub on change --- */
        if (lid_open != last_lid) {
            to_hub = 1; printf("LID:%u\r\n", lid_open); to_hub = 0;
            last_lid = lid_open;
        }
        if (fault != last_fault) {
            to_hub = 1; printf("FAULT:%u\r\n", fault); to_hub = 0;
            last_fault = fault;
        }

        /* --- debug print every 500 ms --- */
        if ((t_ms % 500) == 0) {
            printf("light=%4u mV  lid=%s  fault=%u  btn=%u  dose=%u\r\n",
                   mv, lid_open ? "OPEN  " : "CLOSED", fault, btn,
                   (unsigned)PORTDbits.RD0);
        }

        __delay_ms(TICK_MS);
        t_ms += TICK_MS;
        if (t_ms >= 60000u) t_ms = 0;
    }
}
```
