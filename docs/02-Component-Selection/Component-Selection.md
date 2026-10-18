# Component Selection

**Team 102 – Project Aurora · Member A – Weight Sensing (Natalia Castillo-Diaz)**

This page compares at least three solutions for each major component on the Weight Sensing board. Every component below belongs to the **Weight Sensing subsystem**. Excluded per the assignment: the microcontroller (PIC18F57Q43 Curiosity Nano), passives (including the RC low-pass filter), the tare pushbutton, and the PJ-102AH barrel jack connector.

> Items marked **TODO** must be checked before submitting (prices change by site and day; images should be saved into the repo).

---

## 1. Load Cell (100 g) — **Weight Sensing**

### Option 1. SparkFun 14727 / HT Sensor Technology TAL221, 100 g straight-bar load cell
**$14.50 each** · [DigiKey: SparkFun 14727](https://www.digikey.com/en/products/detail/sparkfun-electronics/14727/9555601) · [TAL221 datasheet](https://cdn.sparkfun.com/assets/9/9/a/f/3/TAL221.pdf)

![SparkFun 14727 / TAL221 load cell](https://mm.digikey.com/Volume0/opasdata/d220001/derivates/1/200/444/149/MFG_SEN-14727_sml.jpg)

| Pros | Cons |
|---|---|
| Real manufacturer datasheet (TAL221), as the assignment requires | Highest unit cost of the three load cells that fit our range ($14.50) |
| Rated at 0.10 kgf (100 g), matching the dose-removal weight range | DigiKey lists a 12-week standard lead time (365 in stock, so it ships now, but this is a risk if stock runs out) |
| Listed accuracy of ±0.05% with an analog bridge output that feeds the instrumentation amp directly | Operating range is only −10 to 40 °C, so it cannot go in a hot or cold enclosure |
| Already the part on our block diagram, so no redesign is needed | Listed for a 6 V supply, so 5 V excitation leaves little headroom |
| Sold through a stocked distributor (DigiKey) | Fragile: easy to overload past the 100 g rating |

### Option 2. Phidgets 3139_0, 100 g single-point load cell
**$7.00 each** (discounts at quantity) · [Phidgets 3139_0](https://www.phidgets.com/?prodid=230) · [mechanical drawing](https://www.phidgets.com/productfiles/3139/3139_0/Documentation/3139_0_Mechanical_2.pdf)

![Phidgets 3139_0](https://cdn.phidgets.com/productfiles/3139/3139_0/Images/551x-/20/3139_0.jpg)

| Pros | Cons |
|---|---|
| About half the price of Option 1 ($7.00) | Rated output is only 600 µV/V, so full scale is about 3 mV at 5 V excitation, which needs a gain near 1000 and leaves little margin |
| Wide 3 to 10 V supply range, which fits the 5 V rail | "Consumer grade" accuracy class, with ±50 mg repeatability and 100 mg/hr creep |
| Detailed spec page, mechanical drawing, and 3D STEP file available | No formal datasheet PDF, only the spec page |
| IP67 rating and a 120 g overload limit give some protection | Ships from Canada, which adds shipping time and cost |
| Small (about 35 mm), with dual mounting holes and a pre-attached 110 mm cable | |

### Option 3. Phidgets FRC4130_0, 300 g single-point load cell (C3 accuracy class)
**$25.00 each** (discounts at 5+) · [Phidgets FRC4130_0](https://www.phidgets.com/?prodid=1299)

![Phidgets FRC4130_0](https://cdn.phidgets.com/productfiles/FRC4130/FRC4130_0/Images/551x-/20/FRC4130_0.jpg)

| Pros | Cons |
|---|---|
| C3 accuracy grade, with ±45 mg repeatability, 60 mg non-linearity, and 45 mg hysteresis | $25.00 each, the most expensive load cell option |
| 300 g capacity gives headroom for a cup plus dose, with a 450 g overload limit | A 300 g range is 3× the 100 g we need, so there is less signal per gram |
| Rated output of 1 mV/V with a 350 Ω bridge, about 5 mV full scale at 5 V excitation | ±15 g zero balance means a large offset, so the tare button and software tare are required |
| IP65 rating, aluminum body, and M3 mounting holes | Only 70 in stock, and it ships from Canada |
| Lower creep and better temperature effect than the consumer-grade cell | About 70 mm long, so it takes more space than the smaller cells |

**Choice: Option 1, SparkFun 14727 / TAL221.**

**Rationale:** The TAL221 has a real manufacturer datasheet, a 100 g rating that matches our dose-removal sensing, and it is already the part the rest of the board is designed around. The Phidgets 3139_0 saves about $7.50, but its lower rated output (600 µV/V) pushes the amplifier gain near the AD623's limit and leaves little margin for noise and offset. The FRC4130 has better accuracy, but its 300 g range wastes signal on a 100 g job and it costs far more. The TAL221 is the best balance of range, documentation, and cost for this project.

---

## 2. Instrumentation Amplifier — **Weight Sensing**

### Option 1. Analog Devices AD623ANZ, 8-PDIP
**$9.87 each** ($7.69 at 10+) · [DigiKey: AD623ANZ](https://www.digikey.com/en/products/detail/analog-devices-inc/AD623ANZ/750974) · [AD623 datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/AD623.pdf)

![AD623ANZ](https://mm.digikey.com/Volume0/opasdata/d220001/derivates/1/010/930/391/505%7EN-8%7EN%7E8-Top_sml.jpg)

| Pros | Cons |
|---|---|
| Rail-to-rail output, so it can swing close to 0 V and the supply, which suits a 0 to 3.3 V ADC input | Maximum supply is only 12 V, though that is fine for a 5 V rail |
| Single supply from 2.7 to 12 V, so it runs from our 5 V rail | Gain tops out around 1000 (datasheet), which is tight for a low-output cell like the Phidgets 3139 |
| Low input offset of 25 µV and 17 nA input bias, good for a few-millivolt bridge signal | Slew rate of only 0.3 V/µs, though that is irrelevant for weight |
| Only 375 µA supply current, the lowest of the three options | Gain resistor must be chosen to match the load cell output |
| 8-pin through-hole DIP, easy to solder and compact | 10-week lead time if stock runs out (2,068 in stock at the time of writing) |
| Inexpensive at $9.87, dropping to $7.69 at 10+ | |

### Option 2. Texas Instruments INA125P, 16-PDIP
**$12.78 each** ($10.04 at 10+) · [DigiKey: INA125P](https://www.digikey.com/en/products/detail/texas-instruments/INA125P/254670) · [TI INA125 datasheet](https://www.ti.com/lit/gpn/ina125)

![INA125P](https://mm.digikey.com/Volume0/opasdata/d220001/derivates/1/300/729/762/296%7E4040049%7EN%7E16_sml.jpg)

| Pros | Cons |
|---|---|
| Single supply from 2.7 to 36 V, so it works from the 5 V or 3.3 V rail | $12.78 each, about 30% more than the AD623 |
| Low input offset of 50 µV and 10 nA input bias, good for a few-millivolt bridge signal | 16-pin DIP takes more board space than 8-pin parts |
| Low supply current of 460 µA, which helps the power budget | Listed at a 20-week lead time (134 in stock at the time of writing) |
| Through-hole 16-DIP package is easy to solder | Output does not swing fully to the rails (datasheet), so check it against the 0 to 3.3 V ADC input |
| Wide −40 to 85 °C operating range | Slew rate of 0.2 V/µs and 150 kHz bandwidth are plenty for weight, but slow for fast signals |
| Gain set with a single external resistor over a very wide range, and a built-in bridge reference (datasheet) | |

### Option 3. Custom: three-op-amp instrumentation amplifier built from a Microchip MCP6004-I/P quad op amp, 14-PDIP
**$0.59 each** ($0.50 at 25) · [DigiKey: MCP6004-I/P](https://www.digikey.com/en/products/detail/microchip-technology/MCP6004-I-P/523060) (datasheet linked on the product page)

<!-- TODO: save a product photo from the DigiKey page into images/mcp6004.jpg -->
![MCP6004-I/P](images/mcp6004.jpg)

| Pros | Cons |
|---|---|
| Very inexpensive at $0.59, versus $9.87 for the AD623 | Input offset is up to 4.5 mV, about the same size as the load cell's full-scale signal, so a high-gain first stage would saturate or need heavy offset trimming |
| Three op amps form the instrumentation amp and the fourth can build an active low-pass filter, replacing the RC filter block | Needs tightly matched resistors (0.1% or better), or common-mode rejection suffers |
| Rail-to-rail input and output on a single supply of 1.8 to 6 V, so it runs from the 5 V rail | 6 V maximum supply leaves little headroom above a 5 V rail |
| 1 MHz gain-bandwidth, 1 pA input bias, and low current of about 100 µA | Much more design and tuning work than a purpose-built part |
| Through-hole 14-DIP, easy to solder and already familiar from class | Stock listings are inconsistent between DigiKey sites, with a 6 to 7 week lead time |

**Choice: Option 1, Analog Devices AD623ANZ.**

**Rationale:** Compared with the INA125P, the AD623 has lower input offset (25 µV vs 50 µV), lower supply current (375 µA vs 460 µA), a faster 800 kHz bandwidth (vs 150 kHz), rail-to-rail output, a smaller 8-pin package, a lower price ($9.87 vs $12.78), and a shorter lead time (10 vs 20 weeks). Its rail-to-rail output matches the 3.3 V ADC input of the microcontroller. The custom op amp build is the least expensive, but its 4.5 mV input offset is comparable to the load cell's signal and its gain and common-mode rejection would depend on matched resistors, whereas the AD623 has about 25 µV offset and a built-in gain-setting scheme that suits the TAL221's few-millivolt output.

---

## 3. 5 V Voltage Regulator — **Weight Sensing**

> **TODO:** confirm the current US unit price and stock for each regulator on its DigiKey page.

### Option 1. STMicroelectronics L7805CV, linear regulator, TO-220
**Price: TODO (about $0.50–$0.80 in the listings checked)** · [DigiKey: L7805CV](https://www.digikey.com/en/products/detail/stmicroelectronics/L7805CV/585964) (datasheet linked on the product page)

<!-- TODO: save a product photo from the DigiKey page into images/l7805cv.jpg -->
![L7805CV](images/l7805cv.jpg)

| Pros | Cons |
|---|---|
| 5 V at up to 1.5 A, far more than the board needs, which gives a large 25% margin | 8 mA quiescent current, the highest idle draw of the three |
| Accepts input up to 35 V, so a 9 V wall supply is well within range | Dropout is about 2 V at 1 A, so it needs at least roughly 7 V in |
| Over-temperature and short-circuit protection built in | Linear regulator wastes the voltage difference as heat: (9 V − 5 V) × load current |
| Through-hole TO-220 package is easy to solder and a heatsink can be added | Only moderate 62 dB ripple rejection |
| The part used in class and already on the block diagram | |

### Option 2. Texas Instruments LM2940CT-5.0/NOPB, low-dropout regulator, TO-220
**Price: TODO (about $1.74 each at a major distributor)** · [DigiKey: LM2940CT-5.0](https://www.digikey.com/en/products/detail/texas-instruments/LM2940CT-5-0-NOPB/32575) (datasheet linked on the product page)

<!-- TODO: save a product photo from the DigiKey page into images/lm2940.jpg -->
![LM2940CT-5.0](images/lm2940.jpg)

| Pros | Cons |
|---|---|
| Low dropout of about 1 V at 1 A (DigiKey lists max), so it works even with a lower input | Costs more than twice the L7805CV |
| Better 72 dB ripple rejection | Higher 15 mA quiescent current |
| Over-temperature, short-circuit, and reverse-polarity protection | Maximum input is only 26 V, though that is fine for 9 V |
| Through-hole TO-220 package | Rated for only 1 A, and needs a specific output capacitor to stay stable (check the datasheet) |

### Option 3. RECOM R-78E5.0-0.5, switching regulator module, 3-SIP
**Price: TODO (about $2.50–$4 each depending on the site)** · [DigiKey: R-78E5.0-0.5](https://www.digikey.com/en/products/detail/recom-power/R-78E5-0-0-5/2834904) (datasheet linked on the product page)

<!-- TODO: save a product photo from the DigiKey page into images/r78e5.jpg -->
![R-78E5.0-0.5](images/r78e5.jpg)

| Pros | Cons |
|---|---|
| 92% efficiency, so almost no wasted heat compared to a linear part | 500 mA maximum, much lower than the L7805CV |
| Pin-compatible with the 78xx family and needs no heatsink | About 4 to 6 times the price of the L7805CV |
| Wide 7 to 28 V input range, with short-circuit protection | Switching noise could disturb the millivolt load cell signal |
| −40 to 85 °C operating range | Mostly sold in tubes of 42 on some sites, which is awkward to order |

**Choice: Option 1, STMicroelectronics L7805CV.**

**Rationale:** The weight board draws only a few tens of milliamps, so the linear regulator's heat is small (for example, 4 V × 0.1 A = 0.4 W). The L7805CV is inexpensive, familiar from class, and has no switching noise, which matters for the few-millivolt load cell signal. The LM2940 and RECOM parts offer lower dropout or better efficiency, but the extra cost is not justified here.

---

## 4. 9 V External Power Source (Wall Adapter) — **Weight Sensing**

> **TODO:** confirm current prices on each page, and confirm the adapter plug fits the PJ-102AH jack. The PJ-102AH mates with a **2.0 mm center-hole × 5.5 mm outer-diameter** plug, while the adapters below use **2.1 mm** plugs.
>
> **TODO (optional):** add the wall adapter you already use as an extra option (model, voltage, current, plug size from its label).

### Option 1. Triad Magnetics WSU090-0800, 9 V, 800 mA wall adapter
**$7.35 each** ([DigiKey: WSU090-0800](https://www.digikey.com/en/products/detail/triad-magnetics/WSU090-0800/3094975)) · datasheet on the product page

<!-- TODO: save a product photo from the DigiKey page into images/wsu090-0800.jpg -->
![WSU090-0800](images/wsu090-0800.jpg)

| Pros | Cons |
|---|---|
| Inexpensive at $7.35, with 2,649 in stock | 800 mA maximum is plenty for this board but far lower than the other options |
| Regulated 9 V output, positive-center | Fixed US blades only, no international plug |
| 72 in (1.83 m) cord and Level VI efficiency | 2.1 mm plug, not the 2.0 mm the jack expects |
| 7.2 W, with 0 to 40 °C operating range | |

### Option 2. CUI SWI18-9-N-P5, 9 V, 2.2 A, 20 W wall adapter
**Price: TODO** ([DigiKey: SWI18-9-N-P5](https://www.digikey.com/en/products/detail/cui-inc/SWI18-9-N-P5/5417740)) · datasheet on the product page

<!-- TODO: save a product photo from the DigiKey page into images/swi18-9-n-p5.jpg -->
![SWI18-9-N-P5](images/swi18-9-n-p5.jpg)

| Pros | Cons |
|---|---|
| 2.2 A capacity gives huge headroom | Much higher current than needed, so the extra capacity is wasted |
| Positive-center and Level VI efficiency | Listed at a 28-week lead time and costs more than Option 1 |
| Meets the safety standards listed, including UL and FCC | 2.1 mm plug, same fit question |

### Option 3. Phihong PSAC12R-090-R, 9 V, 1.33 A, 12 W wall adapter
**Price: TODO** ([DigiKey: PSAC12R-090-R](https://www.digikey.com/en/products/detail/phihong-usa/PSAC12R-090-R/5418510)) · datasheet on the product page

<!-- TODO: save a product photo from the DigiKey page into images/psac12r-090-r.jpg -->
![PSAC12R-090-R](images/psac12r-090-r.jpg)

| Pros | Cons |
|---|---|
| 1.33 A capacity with a comfortable margin | Multi-blade input is sold separately, so a plug set must be ordered |
| Works worldwide with 90 to 264 VAC input | Listed with a 22-week lead time |
| Positive-center, with a 1.5 m cord | 2.1 mm plug again |

**Choice: Option 1, Triad Magnetics WSU090-0800** (or the adapter already used in class, if its plug fits the jack).

**Rationale:** The board's total current is well below 0.1 A, so the 800 mA rating leaves far more than the required 25% margin. The WSU090-0800 is the least expensive option and is in stock, and its regulated 9 V output feeds the L7805CV with enough headroom above the 7 V needed. The higher-current adapters add cost and lead time without any benefit for this load.

---

## 5. Interface Pin Table (Weight board → Hub J1)

> **TODO:** verify against the final block diagram.

| J1 pin | Signal | MCU pin | Direction / type | Notes |
|---|---|---|---|---|
| 1 | DOSE_READY | RD0 | Digital input | From Hub |
| 2 | DOSE_REMOVED | RD1 | Digital output | To Hub |
| 3 | WEIGHT_FAULT | RD2 | Digital output | To Hub |
| 4 | TARE_REQ | RD3 | Digital input | From Hub |
| 5 | HEARTBEAT_A | RD4 | Digital output | To Hub |
| 6 | WEIGHT_LEVEL | RA2 (DAC1) | Analog output, 0–3.3 V | To Hub |
| 7 | (unused) | — | — | Greyed out in the diagram |
| 8 | GND | — | Ground | |

Other board connections: load cell amplifier output after the RC filter → **RA0 (ADC)**; tare / calibrate button → **RB0 (digital input)**; 5 V regulator output → Curiosity Nano 5 V power in.
