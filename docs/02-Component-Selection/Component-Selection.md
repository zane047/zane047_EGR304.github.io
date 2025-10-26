<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Component Selection — Team 206: TechMinds</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <style>
    :root{
      --border:#aaa; --bg:#f2f2f2; --ink:#111; --muted:#1f4e79;
    }
    *{box-sizing:border-box}
    body{
      font-family: Arial, Helvetica, sans-serif;
      color: var(--ink);
      margin: 24px;
      line-height: 1.45;
    }
    h1{margin: 0 0 16px 0; font-size: 28px;}
    h2{margin: 28px 0 12px 0; font-size: 20px;}
    table{
      width:100%;
      border-collapse: collapse;
      margin-bottom: 12px;
    }
    thead{
      background: var(--bg);
    }
    th, td{
      border: 1px solid var(--border);
      padding: 10px;
      vertical-align: top;
    }
    th{
      text-align: left;
    }
    .muted{ color: var(--muted); }
    .solution img{
      max-width: 220px; height: auto;
      border: 1px solid #ddd; padding: 4px; margin-top: 6px;
      display: block;
    }
    .choice{ margin: 8px 0 24px 0; }
  </style>
</head>
<body>
  <h1>Component Selection</h1>

  <!-- ===================== 5 V, 1.5 A Regulator ===================== -->
  <h2>5 V, 1.5 A Regulator</h2>
  <table>
    <thead>
      <tr>
        <th>Solution</th>
        <th>Pros</th>
        <th>Cons</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="solution">
          <strong>Option 1 — 7805 Linear Regulator (TO-220)</strong><br>
          Dropout ≈ 2 V<br>
          Simple 2-capacitor application<br>
          <span class="muted">Price: ~$1–$2 (qty 1)</span><br>
          <span class="muted">Product page:</span>
          <a href="https://www.digikey.com/en/products/filter/linear-voltage-regulators/699">https://www.digikey.com/en/products/filter/linear-voltage-regulators/699</a>
          <div>
            <img src="Thing_1.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Very simple and low-noise output<br>
          &#8226; Ubiquitous; easy to source; short lead time<br>
          &#8226; Excellent line/load regulation for analog stages
        </td>
        <td>
          &#8226; Inefficient from 12 V → 5 V (wastes (Vin−5V)×I as heat)<br>
          &#8226; Heatsink typically needed above ~200–300 mA<br>
          &#8226; Dropout ≈ 2 V limits low-Vin operation
        </td>
      </tr>
      <tr>
        <td class="solution">
          <strong>Option 2 — 5 V Buck Module (e.g., MP1584 / LM2596)</strong><br>
          High-efficiency step-down DC/DC<br>
          <span class="muted">Price: ~$2–$6</span><br>
          <span class="muted">Product page:</span>
          <a href="https://www.digikey.com/en/products/filter/dc-dc-converters/882">https://www.digikey.com/en/products/filter/dc-dc-converters/882</a>
          <div>
            <img src="Thing_2.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; High efficiency → runs cool from 12 V<br>
          &#8226; Handles higher load current with margin<br>
          &#8226; Wide input range; adjustable output available
        </td>
        <td>
          &#8226; Switching ripple/noise requires decoupling &amp; filtering<br>
          &#8226; PCB layout/EMI are more critical<br>
          &#8226; More components; taller solution height
        </td>
      </tr>
      <tr>
        <td class="solution">
          <strong>Option 3 — MCP1825S-5002 (5 V LDO)</strong><br>
          1 A LDO, lower dropout than 7805<br>
          <span class="muted">Price: ~$1–$2</span><br>
          <span class="muted">Product page:</span>
          <a href="https://www.digikey.com/en/products/detail/microchip-technology/MCP1825S-5002E-AB/1505941">https://www.digikey.com/en/products/detail/microchip-technology/MCP1825S-5002E-AB/1505941</a>
          <div>
            <img src="Thing_3.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Lower dropout than 7805<br>
          &#8226; Quieter output than a buck<br>
          &#8226; Simple BOM (input/output caps)
        </td>
        <td>
          &#8226; Still linear → wastes heat at higher load from 12 V<br>
          &#8226; 1 A limit (below 1.5 A target)<br>
          &#8226; Thermal design required above a few hundred mA
        </td>
      </tr>
    </tbody>
  </table>
  <p class="choice"><strong>Choice:</strong> Option 2 — 5 V buck module<br>
  <strong>Rationale:</strong> Best thermals/efficiency from a 12 V source while delivering up to ~1.5 A.</p>

  <!-- ===================== Temperature Sensor ===================== -->
  <h2>Temperature Sensor (Analog Output)</h2>
  <table>
    <thead>
      <tr>
        <th>Solution</th>
        <th>Pros</th>
        <th>Cons</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="solution">
          <strong>Option 1 — LM35 (analog 10 mV/°C)</strong><br>
          TO-92 or SOIC package<br>
          <span class="muted">Price: ~$1–$3</span><br>
          <span class="muted">TI product page:</span>
          <a href="https://www.ti.com/product/LM35">https://www.ti.com/product/LM35</a>
          <div>
            <img src="Thing_4.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Linear 10 mV/°C scaling (≈0–1 V over 0–100 °C)<br>
          &#8226; Operates from 4–30 V; low current; minimal external parts<br>
          &#8226; Well-documented; easy firmware conversion to °C
        </td>
        <td>
          &#8226; Analog output susceptible to noise; needs filtering<br>
          &#8226; Long leads may require op-amp buffering<br>
          &#8226; Absolute accuracy modest without calibration
        </td>
      </tr>
      <tr>
        <td class="solution">
          <strong>Option 2 — TMP36 (analog with offset)</strong><br>
          2.7–5.5 V supply; 750 mV at 25 °C<br>
          <span class="muted">Price: ~$1–$2</span><br>
          <span class="muted">Analog Devices page:</span>
          <a href="https://www.analog.com/en/products/tmp36.html">https://www.analog.com/en/products/tmp36.html</a>
          <div>
            <img src="Thing_5.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Works directly from 5 V rail<br>
          &#8226; Low power; easy ADC interface<br>
          &#8226; Wide temp range (−40 to 125 °C)
        </td>
        <td>
          &#8226; Accuracy typically ±2 °C (worse than digital ICs)<br>
          &#8226; Output has offset that firmware must subtract<br>
          &#8226; Analog output requires decoupling/averaging
        </td>
      </tr>
      <tr>
        <td class="solution">
          <strong>Option 3 — MCP9700 (analog)</strong><br>
          Slope 10 mV/°C with 500 mV offset<br>
          <span class="muted">Price: &lt;$1</span><br>
          <span class="muted">Microchip page:</span>
          <a href="https://www.microchip.com/en-us/product/MCP9700">https://www.microchip.com/en-us/product/MCP9700</a>
          <div>
            <img src="Thing_6.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Very low cost for basic sensing<br>
          &#8226; Operates at 5 V with minimal parts<br>
          &#8226; Compatible voltage levels for PIC ADC
        </td>
        <td>
          &#8226; Offset introduces extra math/possible error<br>
          &#8226; Accuracy ±2 °C typical; drift with time<br>
          &#8226; Still analog → noise/EMI sensitivity
        </td>
      </tr>
    </tbody>
  </table>
  <p class="choice"><strong>Choice:</strong> Option 1 — LM35<br>
  <strong>Rationale:</strong> Direct °C scaling keeps firmware trivial; buffer with MCP6002 if cabling is long.</p>

  <!-- ===================== Op-Amp ===================== -->
  <h2>Op-Amp (Buffer / Gain Stage at 5 V)</h2>
  <table>
    <thead>
      <tr>
        <th>Solution</th>
        <th>Pros</th>
        <th>Cons</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="solution">
          <strong>Option 1 — MCP6002 (dual, rail-to-rail I/O)</strong><br>
          1.8–5.5 V, 1 MHz GBW<br>
          <span class="muted">Price: ~$0.50–$1.50</span><br>
          <span class="muted">Microchip page:</span>
          <a href="https://www.microchip.com/en-us/product/MCP6002">https://www.microchip.com/en-us/product/MCP6002</a>
          <div>
            <img src="Thing_7.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Rail-to-rail input and output at 5 V<br>
          &#8226; Very low quiescent current; good for low-power<br>
          &#8226; Unity-gain stable; dual channels per package
        </td>
        <td>
          &#8226; Limited bandwidth (1 MHz) for higher-speed signals<br>
          &#8226; Modest slew rate; limits fast steps at higher gain<br>
          &#8226; Limited output drive for heavy loads
        </td>
      </tr>
      <tr>
        <td class="solution">
          <strong>Option 2 — TLV2462 (dual, rail-to-rail I/O)</strong><br>
          2.7–6 V, 6.4 MHz GBW<br>
          <span class="muted">Price: ~$1–$2</span><br>
          <span class="muted">TI page:</span>
          <a href="https://www.ti.com/product/TLV2462">https://www.ti.com/product/TLV2462</a>
          <div>
            <img src="Thing_8.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Higher bandwidth and slew rate vs. MCP6002<br>
          &#8226; Rail-to-rail input and output on 5 V<br>
          &#8226; Good choice if more dynamic range needed
        </td>
        <td>
          &#8226; Higher quiescent current than MCP6002<br>
          &#8226; More sensitive to layout at high gain (oscillation risk)<br>
          &#8226; Slightly higher cost
        </td>
      </tr>
      <tr>
        <td class="solution">
          <strong>Option 3 — LM358 (dual, non-rail-to-rail)</strong><br>
          3–32 V, classic op-amp<br>
          <span class="muted">Price: ~$0.25–$0.60</span><br>
          <span class="muted">TI page:</span>
          <a href="https://www.ti.com/product/LM358">https://www.ti.com/product/LM358</a>
          <div>
            <img src="Thing_9.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Very common and low cost<br>
          &#8226; Wide supply range; tolerant of 5 V logic systems<br>
          &#8226; Available everywhere in DIP/SOIC
        </td>
        <td>
          &#8226; Input CM range and output swing don’t reach rails at 5 V<br>
          &#8226; Lower bandwidth/slew than modern R-R devices<br>
          &#8226; Requires headroom → complicates 0–5 V buffering
        </td>
      </tr>
    </tbody>
  </table>
  <p class="choice"><strong>Choice:</strong> Option 1 — MCP6002<br>
  <strong>Rationale:</strong> Matches 5 V rail with rail-to-rail behavior and low power; bandwidth is adequate for slow analog conditioning.</p>

  <!-- ===================== LED ===================== -->
  <h2>Red LED Indicator (5 V MCU Pin + Resistor)</h2>
  <table>
    <thead>
      <tr>
        <th>Solution</th>
        <th>Pros</th>
        <th>Cons</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="solution">
          <strong>Option 1 — 5 mm Red LED (THT)</strong><br>
          Vf ≈ 2.0 V @ 20 mA<br>
          <span class="muted">Price: &lt;$0.10</span><br>
          <span class="muted">Category page:</span>
          <a href="https://www.digikey.com/en/products/filter/led-indication-discrete/105">https://www.digikey.com/en/products/filter/led-indication-discrete/105</a>
          <div>
            <img src="Thing_10.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Bright and easy to see in enclosures<br>
          &#8226; Breadboard-friendly; fast to prototype<br>
          &#8226; Robust leads; easy polarity identification
        </td>
        <td>
          &#8226; Large footprint; not ideal for compact PCBs<br>
          &#8226; Requires drilling/holes (through-hole)<br>
          &#8226; Protrudes above PCB; more mechanical risk
        </td>
      </tr>
      <tr>
        <td class="solution">
          <strong>Option 2 — 0805 SMD Red LED</strong><br>
          Vf ≈ 2.0 V typical<br>
          <span class="muted">Price: &lt;$0.10</span><br>
          <span class="muted">Category page:</span>
          <a href="https://www.digikey.com/en/products/filter/led-indication-discrete/105">https://www.digikey.com/en/products/filter/led-indication-discrete/105</a>
          <div>
            <img src="Thing_11.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Compact footprint for tight PCBs<br>
          &#8226; Suited to automated assembly (reflow)<br>
          &#8226; Lower parasitics; easy to place near logic pins
        </td>
        <td>
          &#8226; Small pads can be tricky to hand-solder<br>
          &#8226; Less visible off-axis than large THT domes<br>
          &#8226; Needs silkscreen polarity marking to avoid errors
        </td>
      </tr>
      <tr>
        <td class="solution">
          <strong>Option 3 — 1206 SMD Red LED</strong><br>
          Vf ≈ 2.0 V typical<br>
          <span class="muted">Price: &lt;$0.10</span><br>
          <span class="muted">Category page:</span>
          <a href="https://www.mouser.com/c/optoelectronics/leds/standard-leds-single-color/">https://www.mouser.com/c/optoelectronics/leds/standard-leds-single-color/</a>
          <div>
            <img src="Thing_13.png" alt="5V Buck Module">
          </div>
        </td>
        <td>
          &#8226; Bigger pads than 0805 (easier hand-soldering)<br>
          &#8226; Still compact for production PCBs<br>
          &#8226; Good visibility at modest current
        </td>
        <td>
          &#8226; SMD still needs current-limit resistor &amp; stencil<br>
          &#8226; Slightly larger than 0805 (board area)<br>
          &#8226; May be taller than needed for low-profile builds
        </td>
      </tr>
    </tbody>
  </table>
  <p class="choice"><strong>Choice:</strong> Option 2 — 0805 SMD Red LED with 330 Ω resistor<br>
  <strong>Rationale:</strong> Compact, assembly-friendly, and bright enough at ~9 mA (5 V → 330 Ω with ~2.0 V LED Vf).</p>

  <!-- ===================== Button (User Interface Subsystem) ===================== -->
  <h2>Button (User Interface Subsystem)</h2>
  <table>
    <thead>
      <tr>
        <th>Solution</th>
        <th>Pros</th>
        <th>Cons</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="solution">
          <strong>Option 1 — PTS645SL43-2 LFS Tactile Switch</strong><br>
          Basic push button, low cost, easy to integrate<br>
          <span class="muted">Price: $0.24/each</span><br>
          <span class="muted">Product page:</span>
          <a href="https://www.digikey.com/en/products/detail/c-k/PTS645SL43-2-LFS/1146755">https://www.digikey.com/en/products/detail/c-k/PTS645SL43-2-LFS/1146755</a><br>
          <span class="muted">Datasheet:</span>
          <a href="https://www.ckswitches.com/media/1471/pts645.pdf">https://www.ckswitches.com/media/1471/pts645.pdf</a>
          <div>
            <img src="PTS645SL43-2LFS (1).jpeg" alt="PTS645SL43-2 LFS Tactile Switch">
          </div>
        </td>
        <td>
          &#8226; Very low cost<br>
          &#8226; Easy to use
        </td>
        <td>
          &#8226; Short lifespan<br>
          &#8226; Less tactile feedback
        </td>
      </tr>

      <tr>
        <td class="solution">
          <strong>Option 2 — Omron B3F Series Tactile Switch</strong><br>
          High-quality tactile push button, reliable, long life<br>
          <span class="muted">Price: $0.24/each</span><br>
          <span class="muted">Product page:</span>
          <a href="https://www.digikey.com/en/products/detail/omron-electronics-inc-emc-div/B3F-1000/33150">https://www.digikey.com/en/products/detail/omron-electronics-inc-emc-div/B3F-1000/33150</a><br>
          <span class="muted">Datasheet:</span>
          <a href="https://omronfs.omron.com/en_US/ecb/products/pdf/en-b3f.pdf">https://omronfs.omron.com/en_US/ecb/products/pdf/en-b3f.pdf</a>
          <div>
            <img src="B3F (1).jpeg" alt="Omron B3F Series Tactile Switch">
          </div>
        </td>
        <td>
          &#8226; Long lifespan (~1M presses)<br>
          &#8226; Reliable actuation<br>
          &#8226; Consistent tactile feel
        </td>
        <td>
          &#8226; Slightly higher cost<br>
          &#8226; Requires careful soldering
        </td>
      </tr>

      <tr>
        <td class="solution">
          <strong>Option 3 — Adafruit Mini Tactile Switch</strong><br>
          Compact, low profile, low cost<br>
          <span class="muted">Price: $0.75/each</span><br>
          <span class="muted">Product page:</span>
          <a href="https://www.adafruit.com/product/367">https://www.adafruit.com/product/367</a><br>
          <span class="muted">Datasheet:</span>
          <a href="https://cdn-shop.adafruit.com/datasheets/B3F-1000-Omron.pdf">https://cdn-shop.adafruit.com/datasheets/B3F-1000-Omron.pdf</a>
          <div>
            <img src="Adafruit_mini_tactile (1).jpg" alt="Adafruit Mini Tactile Switch">
          </div>
        </td>
        <td>
          &#8226; Compact<br>
          &#8226; Low cost<br>
          &#8226; Easy to use
        </td>
        <td>
          &#8226; Shorter lifespan<br>
          &#8226; Less tactile feel
        </td>
      </tr>
    </tbody>
  </table>
  <p class="choice"><strong>Choice:</strong> Option 2 — Omron B3F Series Tactile Switch<br>
  <strong>Rationale:</strong> The Omron B3F is durable (~1M presses) and provides reliable tactile feedback, making it suitable for frequent user interaction. Cheaper alternatives lack lifespan and consistent feel, which could degrade user experience over time.</p>




</body>
</html>
