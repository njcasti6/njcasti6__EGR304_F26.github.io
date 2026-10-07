---
title: Individual Block Diagram
tags:
- block-diagram
- weight-sensing
---

## Overview
This is the individual block diagram for the weight sensing board of Team 102's Project Aurora (Natalia Castillo-Diaz). The board detects when a dose is lifted out of the cup using a 100 g load cell. It has a sensor and no actuator, and it reports status to the hub (Reminder & Alerts) board over an 8-pin ribbon cable.

* Power source: a 9 V barrel jack (Same Sky PJ-102AH) regulated down to 5 V by an STMicroelectronics L7805CV. The PIC18F57Q43 Curiosity Nano produces the 3.3 V logic supply.
* Sensor: an HT Sensor Technology TAL221 100 g load cell, read through an Analog Devices AD623ANZ instrumentation amplifier and an RC low-pass filter into the ADC on RA0.
* Other input: a tare / calibrate button on RB0.
* Team connections: one ribbon cable (J1 to the hub's J1) carrying five digital signals, one analog signal and ground. See the pin tables below.

## Block Diagram

![Weight sensing block diagram, Team 102 Project Aurora](individual-block-diagram.png)

Arrows point toward the receiving block. Gray connector pins are spare. Click the image to enlarge it.

## Parts

| Block | Manufacturer | Part number | Supply |
|---|---|---|---|
| Barrel jack | Same Sky | PJ-102AH | 9 V |
| 5 V regulator | STMicroelectronics | L7805CV | 9 V in, 5 V out |
| 100 g load cell | HT Sensor Technology | TAL221 | 3.3 V |
| Instrumentation amplifier | Analog Devices | AD623ANZ | 3.3 V |
| RC low-pass filter | Passive | R and C values to be chosen | n/a |
| Tare / calibrate button | To be chosen | To be chosen | n/a |
| Microcontroller board | Microchip | PIC18F57Q43 Curiosity Nano (DM164150) | 5 V in, 3.3 V logic |

## Power

| Supply | Regulated | Maximum current |
|---|---|---|
| 9 V | No | Set by the wall adapter |
| 5 V | Yes (L7805CV) | 1.5 A |
| 3.3 V | Yes (Curiosity Nano) | See the Curiosity Nano user guide |

## Microcontroller Pins

| Peripheral | Pin | Signal | Direction |
|---|---|---|---|
| ADC | RA0 | Filtered load cell signal | In |
| DI | RB0 | Tare / calibrate button | In |
| DI | RD0 | DOSE_READY | In |
| DO | RD1 | DOSE_REMOVED | Out |
| DO | RD2 | WEIGHT_FAULT | Out |
| DI | RD3 | TARE_REQ | In |
| DO | RD4 | HEARTBEAT_A | Out |
| DAC | RA2 | WEIGHT_LEVEL (0 to 3.3 V = 0 to 100 g) | Out |

## Ribbon Cable: Weight J1 to Hub J1
All board-to-board signals are 3.3 V logic and active HIGH. Pins 1 to 5 are digital, pins 6 and 7 are analog, and pin 8 is ground.

| Pin | Signal | From | Weight board pin | Meaning |
|---|---|---|---|---|
| 1 | DOSE_READY | Hub | RD0 | Dose is due |
| 2 | DOSE_REMOVED | Weight | RD1 | Dose lifted out |
| 3 | WEIGHT_FAULT | Weight | RD2 | Bad reading |
| 4 | TARE_REQ | Hub | RD3 | Re-zero scale |
| 5 | HEARTBEAT_A | Weight | RD4 | 1 Hz "alive" |
| 6 | WEIGHT_LEVEL | Weight | RA2 | 0 to 3.3 V = 0 to 100 g |
| 7 | Spare | n/a | n/a | Not used |
| 8 | GND | n/a | GND | Ground |
