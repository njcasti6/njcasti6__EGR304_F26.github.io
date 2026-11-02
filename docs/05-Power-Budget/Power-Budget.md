---
title: Power Budget
---

## Overview
The power budget was created to estimate the total power, current of the subsystem, and verify that the selected power supply and voltage regulator can safely support the design. The power budget considers the electrical components used throughout the subsystem, including the PIC18F57Q43 Curiosity Nano, TAL221 load cell, AD623 instrumentation amplifier, and an indicator LED.

The requirements of each component were evaluated using information from their datasheets. These values were used to estimate the total current and power required by the subsystem during operation. Developing the power budget helped verify that the selected 9 V power supply and 5 V voltage regulator provide sufficient power. It also provides a reference that can be updated as the hardware design continues to develop.


![budget1](SS1){style width:"350" height:"300;"}

![budget2](SS2){style width:"350" height:"300;"}

## Conclusions
The estimated power requirements of the subsystem were compared with the capabilities of the selected power supply and voltage regulator. The results of the power budget show whether the available power is sufficient to operate the Curiosity Nano, load cell, instrumentation amplifier, LED, and supporting circuitry simultaneously.

The power budget also helped identify the amount of available capacity between the expected subsystem demand and the maximum capability of the power system. This is important because the design should not operate continuously at the maximum rating of the power supply or regulator. It helped confirm the suitability of the selected power architecture and provides a useful reference as the subsystem progresses from prototyping to the final PCB.


## Resouces

The power budget as a PDF download is available [*here*](PowerBudgetNatalia.xps), and a Microsoft Excel Sheet [*here*](PowerBudgetNatalia.xps).
