# Component Selection — Team 206: TechMinds

---

## 5 V, 1.5 A Regulator

| Solution | Pros | Cons |
|-----------|------|------|
| **Option 1 — 7805 Linear Regulator (TO-220)**<br>Dropout ≈ 2 V<br>Simple 2-capacitor application<br>Price: ~$1–$2 (qty 1)<br>[Product Page](https://www.digikey.com/en/products/filter/linear-voltage-regulators/699)<br>![LM7805](Thing_1.png) | • Very simple and low-noise output<br>• Easy to source and short lead time<br>• Excellent line/load regulation | • Inefficient from 12 V → 5 V<br>• Needs heatsink above ~300 mA<br>• Dropout ≈ 2 V limits low-Vin |
| **Option 2 — 5 V Buck Module (e.g., MP1584 / LM2596)**<br>High-efficiency step-down DC/DC<br>Price: ~$2–$6<br>[Product Page](https://www.digikey.com/en/products/filter/dc-dc-converters/882)<br>![Buck Module](Thing_2.png) | • High efficiency → runs cool from 12 V<br>• Handles higher load current<br>• Wide input range and adjustable output | • Switching ripple/noise requires filtering<br>• PCB layout and EMI critical<br>• More components; taller height |
| **Option 3 — MCP1825S-5002 (5 V LDO)**<br>1 A LDO, lower dropout than 7805<br>Price: ~$1–$2<br>[Product Page](https://www.digikey.com/en/products/detail/microchip-technology/MCP1825S-5002E-AB/1505941)<br>![MCP1825S](Thing_3.png) | • Lower dropout than 7805<br>• Quieter output than buck<br>• Simple BOM | • Still linear → wastes heat<br>• 1 A limit<br>• Thermal design needed above a few hundred mA |

**Choice:** Option 1 — LM7805  
**Rationale:** Has a Low noise output as well as myself personally having familiarity with this product as it was used all semsester in my EGR 304 project class.

---

## Temperature Sensor (Analog Output)

| Solution | Pros | Cons |
|-----------|------|------|
| **Option 1 — LM35 (analog 10 mV/°C)**<br>TO-92 or SOIC<br>Price: ~$1–$3<br>[TI Product Page](https://www.ti.com/product/LM35)<br>![LM35](Thing_4.png) | • Linear 10 mV/°C scaling<br>• Low current; minimal external parts<br>• Well-documented | • Analog output susceptible to noise<br>• Needs buffering for long leads<br>• Accuracy modest without calibration |
| **Option 2 — TMP36 (analog with offset)**<br>2.7–5.5 V supply (750 mV @ 25 °C)<br>Price: ~$1–$2<br>[Analog Devices Page](https://www.analog.com/en/products/tmp36.html)<br>![TMP36](Thing_5.png) | • Works from 5 V rail<br>• Low power and easy ADC interface<br>• Wide temp range (−40 to 125 °C) | • Accuracy ±2 °C<br>• Offset must be subtracted in firmware<br>• Analog output needs filtering |
| **Option 3 — MCP9700 (analog)**<br>Slope 10 mV/°C with 500 mV offset<br>Price: < $1<br>[Microchip Page](https://www.microchip.com/en-us/product/MCP9700)<br>![MCP9700](Thing_6.png) | • Low cost<br>• Operates at 5 V<br>• Compatible with PIC ADC | • Offset adds math/error<br>• Accuracy ±2 °C typical<br>• Still analog → noise/EMI sensitive |

**Choice:** Option 2 — TM36  
**Rationale:** Works from a 5V rail as well as has a low power and easy ADC interface.

---

## Op-Amp (Buffer / Gain Stage @ 5 V)

| Solution | Pros | Cons |
|-----------|------|------|
| **Option 1 — MCP6002 (dual, rail-to-rail I/O)**<br>1.8–5.5 V, 1 MHz GBW<br>Price: ~$0.50–$1.50<br>[Microchip Page](https://www.microchip.com/en-us/product/MCP6002)<br>![MCP6002](Thing_7.png) | • Rail-to-rail I/O at 5 V<br>• Low quiescent current<br>• Unity-gain stable | • Limited bandwidth (1 MHz)<br>• Modest slew rate<br>• Low output drive |
| **Option 2 — TLV2462 (dual, rail-to-rail I/O)**<br>2.7–6 V, 6.4 MHz GBW<br>Price: ~$1–$2<br>[TI Page](https://www.ti.com/product/TLV2462)<br>![TLV2462](Thing_8.png) | • Higher bandwidth and slew rate<br>• Rail-to-rail I/O on 5 V<br>• Good dynamic range | • Higher quiescent current<br>• More layout-sensitive<br>• Slightly higher cost |
| **Option 3 — LM358 (dual, non-R-R)**<br>3–32 V classic op-amp<br>Price: ~$0.25–$0.60<br>[TI Page](https://www.ti.com/product/LM358)<br>![LM358](Thing_9.png) | • Low cost and common<br>• Wide supply range<br>• Available everywhere | • Input/output don’t reach rails<br>• Lower bandwidth and slew<br>• Needs headroom |

**Choice:** Option 1 — MCP6002  
**Rationale:** Matches 5 V rail with rail-to-rail behavior and low power; bandwidth is adequate.

---

## Red LED Indicator (5 V MCU Pin + Resistor)

| Solution | Pros | Cons |
|-----------|------|------|
| **Option 1 — 5 mm Red LED (THT)**<br>Vf ≈ 2.0 V @ 20 mA<br>Price: < $0.10<br>[Digikey Page](https://www.digikey.com/en/products/filter/led-indication-discrete/105)<br>![5mm LED](Thing_10.png) | • Bright and easy to see<br>• Breadboard-friendly<br>• Durable leads | • Large footprint<br>• Requires holes (through-hole)<br>• Protrudes above PCB |
| **Option 2 — 0805 SMD Red LED**<br>Vf ≈ 2.0 V typical<br>Price: < $0.10<br>[Digikey Page](https://www.digikey.com/en/products/filter/led-indication-discrete/105)<br>![0805 LED](Thing_11.png) | • Compact for tight PCBs<br>• Suited for reflow assembly<br>• Low parasitics | • Tricky to hand-solder<br>• Less visible off-axis<br>• Needs silkscreen polarity |
| **Option 3 — 1206 SMD Red LED**<br>Vf ≈ 2.0 V typical<br>Price: < $0.10<br>[Mouser Page](https://www.mouser.com/c/optoelectronics/leds/standard-leds-single-color/)<br>![1206 LED](Thing_13.png) | • Bigger pads (easier hand-solder)<br>• Still compact<br>• Good visibility | • Needs resistor & stencil<br>• Slightly larger area<br>• Taller profile |

**Choice:** Option 2 — 0805 SMD Red LED with 330 Ω resistor  
**Rationale:** Compact, assembly-friendly, and bright enough at ~9 mA.

---

## Button (User Interface Subsystem)

| Solution | Pros | Cons |
|-----------|------|------|
| **Option 1 — PTS645SL43-2 LFS Tactile Switch**<br>Basic push button, low cost, easy to integrate<br>Price: $0.24/each<br>[Product Page](https://www.digikey.com/en/products/detail/c-k/PTS645SL43-2-LFS/1146755)<br>[Datasheet](https://www.ckswitches.com/media/1471/pts645.pdf)<br>![PTS645SL43-2LFS](PTS645SL43-2LFS%20(1).jpeg) | • Very low cost<br>• Easy to use | • Short lifespan<br>• Less tactile feedback |
| **Option 2 — Omron B3F Series Tactile Switch**<br>High-quality tactile push button, reliable, long life<br>Price: $0.24/each<br>[Product Page](https://www.digikey.com/en/products/detail/omron-electronics-inc-emc-div/B3F-1000/33150)<br>[Datasheet](https://omronfs.omron.com/en_US/ecb/products/pdf/en-b3f.pdf)<br>![B3F](B3F%20(1).jpeg) | • Long lifespan (~1M presses)<br>• Reliable actuation<br>• Consistent tactile feel | • Slightly higher cost<br>• Requires careful soldering |
| **Option 3 — Adafruit Mini Tactile Switch**<br>Compact, low profile, low cost<br>Price: $0.75/each<br>[Product Page](https://www.adafruit.com/product/367)<br>[Datasheet](https://cdn-shop.adafruit.com/datasheets/B3F-1000-Omron.pdf)<br>![Adafruit Mini](Adafruit_mini_tactile%20(1).jpg) | • Compact<br>• Low cost<br>• Easy to use | • Shorter lifespan<br>• Less tactile feel |

**Choice:** Option 2 — Omron B3F Series Tactile Switch  
**Rationale:** The Omron B3F is durable (~1M presses) and provides reliable tactile feedback, making it suitable for frequent user interaction. Cheaper alternatives lack lifespan and consistent feel, which could degrade user experience over time.


---
