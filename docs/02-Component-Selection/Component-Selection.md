# Component Selection — Team 206: TechMinds

---

## 1. 5 V, 1.5 A Regulator

| Solution | Pros | Cons |
|-----------|------|------|
| **Option 1 — 7805 Linear Regulator (TO-220)**<br>Dropout ≈ 2 V<br>Simple 2-capacitor application<br>Price: ~$1–$2 (qty 1)<br>[Product Page](https://www.digikey.com/en/products/filter/linear-voltage-regulators/699)<br>![LM7805](Thing_1.png) | • Very simple and low-noise output<br>• Easy to source and short lead time<br>• Excellent line/load regulation | • Inefficient from 12 V → 5 V<br>• Needs heatsink above ~300 mA<br>• Dropout ≈ 2 V limits low-Vin |
| **Option 2 — 5 V Buck Module (e.g., MP1584 / LM2596)**<br>High-efficiency step-down DC/DC<br>Price: ~$2–$6<br>[Product Page](https://www.digikey.com/en/products/filter/dc-dc-converters/882)<br>![Buck Module](Thing_2.png) | • High efficiency → runs cool from 12 V<br>• Handles higher load current<br>• Wide input range and adjustable output | • Switching ripple/noise requires filtering<br>• PCB layout and EMI critical<br>• More components; taller height |
| **Option 3 — MCP1825S-5002 (5 V LDO)**<br>1 A LDO, lower dropout than 7805<br>Price: ~$1–$2<br>[Product Page](https://www.digikey.com/en/products/detail/microchip-technology/MCP1825S-5002E-AB/1505941)<br>![MCP1825S](Thing_3.png) | • Lower dropout than 7805<br>• Quieter output than buck<br>• Simple BOM | • Still linear → wastes heat<br>• 1 A limit<br>• Thermal design needed above a few hundred mA |

**Choice:** Option 1 — LM7805  
**Rationale:** We choose the LM7805 linear regulator based on providing a stable, low-noise 5 V output to be used for powering sensitive analog circuits or microcontrollers. Its two-capacitor application provides minimal required external parts and a straightforward PCB footprint, thus lowering the design complexity and potential points of failure on the PCB. Having used the LM7805 already in previous course work throughout the semester, the design team is accustomed to its behavior — less risky at stages of layout and debugging. Switching regulators (buck modules) might yield much higher efficiencies and lower power dissipation, particularly if the conversion is between 9 V or 12 V and 5 V, but such systems also present switching noise, require a careful PCB layout and filtering, and contribute additional complexity, which might not be justified for a fairly small current draw (~1.5 A max). That noise could interfere with analog signals (e.g., from sensors).

---

## 2. Temperature Sensor (Analog Output)

| Solution | Pros | Cons |
|-----------|------|------|
| **Option 1 — LM35 (analog 10 mV/°C)**<br>TO-92 or SOIC<br>Price: ~$1–$3<br>[TI Product Page](https://www.ti.com/product/LM35)<br>![LM35](Thing_4.png) | • Linear 10 mV/°C scaling<br>• Low current; minimal external parts<br>• Well-documented | • Analog output susceptible to noise<br>• Needs buffering for long leads<br>• Accuracy modest without calibration |
| **Option 2 — TMP36 (analog with offset)**<br>2.7–5.5 V supply (750 mV @ 25 °C)<br>Price: ~$1–$2<br>[Analog Devices Page](https://www.analog.com/en/products/tmp36.html)<br>![TMP36](Thing_5.png) | • Works from 5 V rail<br>• Low power and easy ADC interface<br>• Wide temp range (−40 to 125 °C) | • Accuracy ±2 °C<br>• Offset must be subtracted in firmware<br>• Analog output needs filtering |
| **Option 3 — MCP9700 (analog)**<br>Slope 10 mV/°C with 500 mV offset<br>Price: < $1<br>[Microchip Page](https://www.microchip.com/en-us/product/MCP9700)<br>![MCP9700](Thing_6.png) | • Low cost<br>• Operates at 5 V<br>• Compatible with PIC ADC | • Offset adds math/error<br>• Accuracy ±2 °C typical<br>• Still analog → noise/EMI sensitive |

**Choice:** Option 2 — TM36  
**Rationale:** The TMP36 was selected because it runs directly from a 5 V rail (the same for the rest of our MCU and analog circuitry), making the power supply design simple and eliminating the need for additional regulators. Its analog output makes it easy to connect to the ADC in the user’s microcontroller without needing the heavy digital communication overhead (such as I²C or SPI) that comes along with the requirement for complex digital communication, which reduces firmware complexity and the chances of communication bugs. Its low power consumption and temperature range of –40 °C to 125 °C makes it versatile for many different working conditions. Analog sensors are inherently more susceptible to noise and EMI compared to digital sensors, but the TMP36 demonstrates well-documented stable behavior; consequently any incoming noise can be mitigated along with proper grounding, filtering, and layout practices.

---

## 3. Op-Amp (Buffer / Gain Stage @ 5 V)

| Solution | Pros | Cons |
|-----------|------|------|
| **Option 1 — MCP6002 (dual, rail-to-rail I/O)**<br>1.8–5.5 V, 1 MHz GBW<br>Price: ~$0.50–$1.50<br>[Microchip Page](https://www.microchip.com/en-us/product/MCP6002)<br>![MCP6002](Thing_7.png) | • Rail-to-rail I/O at 5 V<br>• Low quiescent current<br>• Unity-gain stable | • Limited bandwidth (1 MHz)<br>• Modest slew rate<br>• Low output drive |
| **Option 2 — TLV2462 (dual, rail-to-rail I/O)**<br>2.7–6 V, 6.4 MHz GBW<br>Price: ~$1–$2<br>[TI Page](https://www.ti.com/product/TLV2462)<br>![TLV2462](Thing_8.png) | • Higher bandwidth and slew rate<br>• Rail-to-rail I/O on 5 V<br>• Good dynamic range | • Higher quiescent current<br>• More layout-sensitive<br>• Slightly higher cost |
| **Option 3 — LM358 (dual, non-R-R)**<br>3–32 V classic op-amp<br>Price: ~$0.25–$0.60<br>[TI Page](https://www.ti.com/product/LM358)<br>![LM358](Thing_9.png) | • Low cost and common<br>• Wide supply range<br>• Available everywhere | • Input/output don’t reach rails<br>• Lower bandwidth and slew<br>• Needs headroom |

**Choice:** Option 1 — MCP6002  
**Rationale:** Since all of our system is powered at 5 V, the MCP6002 provides rail-to-rail input and output on a 5 V supply (ideal according to our design) and prevents saturation/clipping problems that arise if the op-amp lacked rail-to-rail capability. The low quiescent current helps minimize power waste and heating — a problem if the system is working nonstop. Also, due to the MCP6002 being unity-gain stable and having a modest ~1 MHz bandwidth, it is beneficial for buffering DC or low-frequency sensor signals without adding unwanted gain or noise. Since our project probably uses relatively slow-changing analog signals (perhaps from the temperature sensor or other slow sensors), the limited bandwidth shouldn’t be an issue — and given anticipated loads the lower slew rate and more limited drive should be good enough.

---

## 4. Red LED Indicator (5 V MCU Pin + Resistor)

| Solution | Pros | Cons |
|-----------|------|------|
| **Option 1 — 5 mm Red LED (THT)**<br>Vf ≈ 2.0 V @ 20 mA<br>Price: < $0.10<br>[Digikey Page](https://www.digikey.com/en/products/filter/led-indication-discrete/105)<br>![5mm LED](Thing_10.png) | • Bright and easy to see<br>• Breadboard-friendly<br>• Durable leads | • Large footprint<br>• Requires holes (through-hole)<br>• Protrudes above PCB |
| **Option 2 — 0805 SMD Red LED**<br>Vf ≈ 2.0 V typical<br>Price: < $0.10<br>[Digikey Page](https://www.digikey.com/en/products/filter/led-indication-discrete/105)<br>![0805 LED](Thing_11.png) | • Compact for tight PCBs<br>• Suited for reflow assembly<br>• Low parasitics | • Tricky to hand-solder<br>• Less visible off-axis<br>• Needs silkscreen polarity |
| **Option 3 — 1206 SMD Red LED**<br>Vf ≈ 2.0 V typical<br>Price: < $0.10<br>[Mouser Page](https://www.mouser.com/c/optoelectronics/leds/standard-leds-single-color/)<br>![1206 LED](Thing_13.png) | • Bigger pads (easier hand-solder)<br>• Still compact<br>• Good visibility | • Needs resistor & stencil<br>• Slightly larger area<br>• Taller profile |
 
**Choice:** Option 1 — 5 mm Red LED (THT) 
**Rationale:** I chose to utilize a 5 mm through-hole (THT) red LED as it is bright and can be visible from different directions — which is good for your user experience for status indications or alerts. For a development or proof-of-concept board, visibility is often more critical than compactness, and THT LEDs can certainly serve as a practical aid for debugging, early prototype, or demonstration builds. The through-hole form factor also makes soldering and rework for prototypes or low-volume builds easier, and the components can be replaced and/or adjusted by hand if they need to be replaced or modified.

---

## 5. Button (User Interface Subsystem)

| Solution | Pros | Cons |
|-----------|------|------|
| **Option 1 — PTS645SL43-2 LFS Tactile Switch**<br>Basic push button, low cost, easy to integrate<br>Price: $0.24/each<br>[Product Page](https://www.digikey.com/en/products/detail/c-k/PTS645SL43-2-LFS/1146755)<br>[Datasheet](https://www.ckswitches.com/media/1471/pts645.pdf)<br>![PTS645SL43-2LFS](PTS645SL43-2LFS%20(1).jpeg) | • Very low cost<br>• Easy to use | • Short lifespan<br>• Less tactile feedback |
| **Option 2 — Omron B3F Series Tactile Switch**<br>High-quality tactile push button, reliable, long life<br>Price: $0.24/each<br>[Product Page](https://www.digikey.com/en/products/detail/omron-electronics-inc-emc-div/B3F-1000/33150)<br>[Datasheet](https://omronfs.omron.com/en_US/ecb/products/pdf/en-b3f.pdf)<br>![B3F](B3F%20(1).jpeg) | • Long lifespan (~1M presses)<br>• Reliable actuation<br>• Consistent tactile feel | • Slightly higher cost<br>• Requires careful soldering |
| **Option 3 — Adafruit Mini Tactile Switch**<br>Compact, low profile, low cost<br>Price: $0.75/each<br>[Product Page](https://www.adafruit.com/product/367)<br>[Datasheet](https://cdn-shop.adafruit.com/datasheets/B3F-1000-Omron.pdf)<br>![Adafruit Mini](Adafruit_mini_tactile%20(1).jpg) | • Compact<br>• Low cost<br>• Easy to use | • Shorter lifespan<br>• Less tactile feel |

**Choice:** Option 2 — Omron B3F Series Tactile Switch  
**Rationale:** We included the Omron B3F tactile switch due to a high-quality tactile feel, a reliable actuation, and a long lifetime (on the order of 1 million actuations). For a user interface — where human interaction, button press feel, and durability matter — these features show a substantial difference in usability and user satisfaction. A button that feels cheap or fails after a few uses can really degrade the perceived quality of the product. Though there are also cost-effective competitors like simple tactile switches or compact mini-switches, these alternatives can often cause poor tactile feedback, inconsistency in the actuation force applied, shorter lifespan, and potentially unreliable switching over time. Even for a project that might be heavily tested through manual testing, debugging, or user interaction (particularly in a lab or classroom), this can be a major drawback.

## 6. 9V 3A Unregulated Power Supply (**Power Subsystem**)

### Option 1

| Solution | Pros | Cons |
|----------|------|------|
| **Mean Well GST40A09-P1J**<br>![GST40A](206C4.png)<br>90W unregulated 9V DC power supply, high reliability<br>Price: $17.30/each<br>[Product Page](https://www.digikey.com/en/products/detail/mean-well-usa-inc/gst40a09-p1j/7703703)<br>[Datasheet](https://www.meanwellusa.com/upload/pdf/GST40A/GST40A-spec.pdf) | - High reliability<br>- Stable output<br>- Protects circuits | - Bulkier<br>- Expensive |

### Option 2

| Solution | Pros | Cons |
|----------|------|------|
| **Amazon Basics 9V 3A AC/DC Adapter (B09ZTKTLGW)**<br>![B09ZTKTLGW](206C2.png)<br>regulated output, compact wall-plug design<br>Price: $4.99/each<br>[Product Page](https://www.amazon.com/gp/product/B09ZTKTLGW/) | - Affordable and widely available<br>- Compact, plug-in wall adapter (saves space)<br>- Regulated output ensures stable voltage<br>- Includes standard barrel plug (5.5mm x 2.1mm)<br>- Suitable for continuous operation | - Lower build quality compared to industrial brands<br>- Limited protection features (no explicit overcurrent/short-circuit protection)<br>- Not designed for harsh environments or high-reliability applications<br>- May run warm under full load |

### Option 3

| Solution | Pros | Cons |
|----------|------|------|
| **Mean‑Well SGA40E09‑P1J**<br>![SGA40E09](206C3.png)<br>Mean Well SGA40E09-P1J – 40W wall-mount (plug-in) AC/DC adapter, 9 V output, ~4.44 A max<br>Price: $21.47/each<br>[Product Page](https://www.mouser.com/ProductDetail/MEAN-WELL/SGA40E09-P1J?qs=kU9BrJCShyk7JuwjBVtOlQ%3D%3D&srsltid=AfmBOooYPFy-o8z2TKsX1w-nQ8iGEcE8ENDtLzemfdFMs3mg4elY-K3U&utm_source=chatgpt.com)<br>[Datasheet](https://www.stathisnet.gr/image/SpecsUpload/028888.pdf?utm_source=chatgpt.com) | - Slim wall-mounted adapter (plug-in) form factor – simpler installation<br>- 9 V × 4.44 A gives ~40W, plenty margin above 3 A requirement<br>- High efficiency (reduces heat) and modern protections: Overcurrent, Overvoltage, Short-circuit built in | - Being plug-in, less modular for non-standard connector scenarios<br>- Slightly higher cost compared to generic adapters<br>- If input plug standard different (US vs EU), may require adapter or variant|

**Choice:** Option 2: Amazon Basics 9V 3A AC/DC Adapter (B09ZTKTLGW)  
**Rationale:** the Amazon Basics 9 V 3 A plug-in adapter because it is cost-effective, widely available, and straightforward to integrate into the design. As a wall-plug adapter, it simplifies the power architecture by avoiding bulky external power bricks or custom power supply enclosures. For prototyping, testing, and typical indoor use — which are the expected operating conditions for this system — a compact, plug-and-play adapter provides an excellent balance between convenience, simplicity, and sufficient power delivery. The regulated 9 V output ensures consistent input for downstream regulators or circuitry, helping to maintain stable operation even if wall outlet voltage fluctuates. Although this adapter lacks the industrial-grade protections (like overcurrent, over-voltage, short-circuit protection) and the ruggedness of a supply like the Mean Well GST40A09-P1J, such protections may not be strictly necessary for a classroom or home use prototype — especially when we are in control of the load and environment.

## Final Major Components Selected 

| Solution | Pros | Cons |
|-----------|------|------|
| **7805 Linear Regulator (TO-220)**<br>Dropout ≈ 2 V<br>Simple 2-capacitor application<br>Price: ~$1–$2 (qty 1)<br>[Product Page](https://www.digikey.com/en/products/filter/linear-voltage-regulators/699)<br>![LM7805](Thing_1.png) | • Very simple and low-noise output<br>• Easy to source and short lead time<br>• Excellent line/load regulation | • Inefficient from 12 V → 5 V<br>• Needs heatsink above ~300 mA<br>• Dropout ≈ 2 V limits low-Vin |
| **TMP36 (analog with offset)**<br>2.7–5.5 V supply (750 mV @ 25 °C)<br>Price: ~$1–$2<br>[Analog Devices Page](https://www.analog.com/en/products/tmp36.html)<br>![TMP36](Thing_5.png) | • Works from 5 V rail<br>• Low power and easy ADC interface<br>• Wide temp range (−40 to 125 °C) | • Accuracy ±2 °C<br>• Offset must be subtracted in firmware<br>• Analog output needs filtering |
| **MCP6002 (dual, rail-to-rail I/O)**<br>1.8–5.5 V, 1 MHz GBW<br>Price: ~$0.50–$1.50<br>[Microchip Page](https://www.microchip.com/en-us/product/MCP6002)<br>![MCP6002](Thing_7.png) | • Rail-to-rail I/O at 5 V<br>• Low quiescent current<br>• Unity-gain stable | • Limited bandwidth (1 MHz)<br>• Modest slew rate<br>• Low output drive |
| **5 mm Red LED (THT)**<br>Vf ≈ 2.0 V @ 20 mA<br>Price: < $0.10<br>[Digikey Page](https://www.digikey.com/en/products/filter/led-indication-discrete/105)<br>![5mm LED](Thing_10.png) | • Bright and easy to see<br>• Breadboard-friendly<br>• Durable leads | • Large footprint<br>• Requires holes (through-hole)<br>• Protrudes above PCB |
| **Omron B3F Series Tactile Switch**<br>High-quality tactile push button, reliable, long life<br>Price: $0.24/each<br>[Product Page](https://www.digikey.com/en/products/detail/omron-electronics-inc-emc-div/B3F-1000/33150)<br>[Datasheet](https://omronfs.omron.com/en_US/ecb/products/pdf/en-b3f.pdf)<br>![B3F](B3F%20(1).jpeg) | • Long lifespan (~1M presses)<br>• Reliable actuation<br>• Consistent tactile feel | • Slightly higher cost<br>• Requires careful soldering |
| **Amazon Basics 9V 3A AC/DC Adapter (B09ZTKTLGW)**<br>![B09ZTKTLGW](206C2.png)<br>regulated output, compact wall-plug design<br>Price: $4.99/each<br>[Product Page](https://www.amazon.com/gp/product/B09ZTKTLGW/) | - Affordable and widely available<br>- Compact, plug-in wall adapter (saves space)<br>- Regulated output ensures stable voltage<br>- Includes standard barrel plug (5.5mm x 2.1mm)<br>- Suitable for continuous operation | - Lower build quality compared to industrial brands<br>- Limited protection features (no explicit overcurrent/short-circuit protection)<br>- Not designed for harsh environments or high-reliability applications<br>- May run warm under full load |
---
