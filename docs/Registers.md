## Registers / Gated D-Latch

Each register bit consists of four discrete NAND gates configured as a level-triggered (transparent) storage cell, preceded by an input buffer and terminated with an output inverter.

This component is known as a Gated D-Latch. Here is breakdown of the component:
- **Enable/Clock (The middle input):** When this signal is `ON` (1), the latch is "transparent" and accepts new data. When it is `OFF` (0), the latch ignores new data and holds its current state
- **Data (Top Input):** The signal we want to store.
- **Left side NANDs:** The two NAND gates on the left control the inputs to the memory element. Notice how the top-left NAND output feeds directly into the input of the bottom-left NAND: this is a cost effective way to get an inverted output, instead of traditionally building it with an inverter (If you search on Google "D-latch", you will see it traditionally build with a inverter), this is because if clock is High, the output of NAND gate is just the inversion of our `DATA` input The inverted input ensures the memory element never receive the invalid command "SET = `0` & RESET = `0`" which causes a race condition.
- **The Memory Element (Right Side):** The two cross coupled NAND gates form a SR-Latch. This is the part that actually remembers the stored bit.

This video [Latches and Flip-flops](https://www.youtube.com/watch?v=y7Zf7Bv_J74) explains the Gated D-latch wonderfully.

<p align="center">
  <img src="../images/Register-2.png" alt="Register Gatekeeper and Bus Interface" width="600">
</p>


 <p align="center">
  <img src="../images/gated-d-latch.jpeg" alt="Discrete Gated D-Latch" width="550">
</p>

> ### Error Log: Light always 1 on the register 
>
> **Issue:** The jumper wire that goes from the output of one NAND gate to the input of the other NAND didn't make a good connection, there was no continuity

> ### Error Log: Unable to store a 1, can only store a 0 correctly
>
> **Issue:**  I used two types of transistors when building this ALU. Ones brought from RS Components (High A grade). And ones from Temu (possible grade C). Although the gain of the two transistors are the same, the switching speed/slew rates may differ. I used a cheap transistor with the NAND gates in the D latch. This caused it to not be able to store a 1 (it defaulted to `OFF`). When I switched the transistors indicated in the sketch below, it correctly stored the values. One can use the ring oscillator test (with an Arduino) to test the speed of transistors. Since I don't have an oscilloscope to read the frequency, I can either read the amount of current used by system, or use an Arduino that has a frequency pin and library that can complete the same task as the oscilloscope ("relatively the same task").
> 
> Placing slow transistors in the feedback NAND path created asymmetric internal propagation delays. When attempting to latch a `1`, the slower gate could not settle before the enable line decayed, causing the latch to collapse to its default low state (this is the only cause I could come up with)
> 
> **Resolution:** Use high-speed transistors exclusively within the internal bistable feedback loop.

<p align="center">
  <img src="../images/Indicated_Transistor_Problems.png" alt="Transistor problems" width="600">
</p>

> ### Error Log: Better transistors at the Enable pins give incorrect outputs?
>
> **Symptom:** When I connected the common enable rail together of the 4 bit register, depending on what transistor I used I got "erratic" latching .
> 
> **Issue:** It looks like if I use the cheap, slow transistors (From my trusty friend Temu) as the enable transistors, the 4 latches works perfectly, however, ironically, if I use the more expensive ones (From RS components) at the enable pins, in tangent with the other cheaps ones it doesn't work.
> 
> **Why does this happen?:** Absolutely no clue, it cannot be slew rates like the previous error log since the enable pins aren't depended on the slew rates like the interconnected NANDs were in the latch (or maybe can it?). Online resources (My other trusty friend the LLM) says it can be inductive Kickback. " Fast transistors draw their base drive current in nanoseconds ($\text{high } \frac{di}{dt}$), exciting an undamped $LC$ resonance between wire inductance and transistor input capacitances. **Resulting Ringing:** The enable rail oscillates violently during transitions. The latches register these transient spikes as rapid multiple clock edges, capturing unstable bus data. **The Anomaly:** Slower transistors on the enable input mitigate this effect because their gradual transition ($\text{lower } \frac{di}{dt}$) suppresses ringing without exciting high-frequency resonance""
> 
> Somehow I feel the like the LLMs answer isn't quite right.
>
> **Solution:** Used the cheaper "slower" transistors at the enable pins for all the Latches

[The "Ring Oscillator" Test transistors](https://hackaday.io/project/184912-8-bit-transistor-computer/log/205741-gates-ring-oscillator-speed-tests)


### The Control Logic: The "AND" Gate

You do not connect the Clock directly to the Latch. You use a "Gatekeeper" circuit to ensure the register only updates when you want it to.

- **The Circuit:** A single **2-Input AND Gate**.
- **Input A:** `System Clock` 
- **Input B:** `Load Enable` (The specific command wire, e.g., "Load Reg B").
- **Output:** Goes to the **Enable/G** pins of all 4 bits in the register.

 <p align="center">
  <img src="../images/Register-2.png" alt="Register Gatekeeper and Bus Interface" width="600">
</p>

---
### Output Register
  
Captures (Only when Enabled) the value on the Bus.

> ### Error Log: - Bit 4 always stayed ON
>
> **Issue:**  The Resistor was loose, so a new resistor was placed and this fixed the issue

<p align="center">
  <img src="../images/output-register.png" alt="output-register" width="800">
</p>

 ---
 ## Accumalator Register

The Accumulator is a specialized 4-bit register that serves as both the primary operand source (Input $A$) and the destination register for the Arithmetic Logic Unit (ALU).

Because the Accumulator output feeds directly back into the ALU inputs while simultaneously latching ALU computation results, a transparent latch cannot be used here. Doing so would form an asynchronous feedback race condition where outputs oscillate uncontrollably during the high clock phase. To ensure deterministic execution, the Accumulator is implemented using **edge-triggered Master-Slave D Flip-Flops** (specifically a falling-edge triggered one).

<p align="center">
  <img src="../images/Accumulator_diagram.jpg" alt="Accumulator" width="600">
</p>

### Master-Slave Architecture 

Each bit of the Accumulator is constructed by cascading two discrete gated D-latches in series, controlled by complementary clock phases.

1. **Master Latch (Input Stage):** Receives raw data from the input buffers and is clocked directly by the gated clock line ($\text{CLK}_{\text{GATED}}$).
2. **Slave Latch (Output Stage):** Receives the internal state of the Master latch and is clocked through an inverter (`CLK INV`).
   
#### Trigger Polarity Derivation
* **Inverter on Slave Enable (Current Design):** 
  * While $\text{CLK} = 1$, the Master is transparent (tracking data) and the Slave is latched shut.
  * On the **falling edge ($\text{CLK} \downarrow$)**, the Master locks its state shut, and the Slave immediately opens, propagating the settled Master value to the output.(making it Falling-Edge Triggered)
 
|Clock Signal|Data Input|Load Enable|Master Internal|Slave Output (Q)|Meaning|
|---|---|---|---|---|---|
|**High (1)**|**Change**|**1**|**Follows Input**|**No Change**|**"Thinking Time"** (ALU calculates, Master listens, Output is stable).|
|**Falling (**$\downarrow$**)**|**Stable**|**1**|**Locks**|**Updates to Input**|**"Latching Time"** (The update happens here, at the end).|
|**Low (0)**|X|X|**Locked**|**Stable**|**Safe State** (Waiting for next cycle).|



<p align="center">
  <img src="../images/accumulator.jpeg" alt="Accumulator" width="600">
</p>

The decision to configure the Accumulator for **Falling-Edge Triggering** directly resolves timing hazards inherent to discrete Ripple-Carry Adders:

1. **The Carry Propagation Hazard:**
   * During this propagation window, intermediate output lines glitch through invalid arithmetic states.(e.g. In a previous Error Log we observed the brief flicker of lights, which indicate the ALU was still in a state of computation)
   * If the register sampled on the **Rising Edge ($\text{CLK} \uparrow$)**, it would capture data at the beginning of the clock cycle, latching corrupt, unsettled intermediate logic before carry propagation finishes.

2. **Falling-Edge Settlement ("Wait-and-See"):**
   * Setting the Master latch to be transparent during $\text{CLK} = 1$ turns the entire high clock half-period into an integrated settlement buffer.
   * The Master latch observes the initial ripple glitches, but because the Slave latch remains locked ($E_{\text{Slave}} = 0$), these transient glitches never reach the output lines or ALU input $A$.
   * When the clock transitions low ($\text{CLK} \downarrow$), carry propagation is complete. The Master isolates the verified final sum, and the Slave presents it cleanly to the system.
  
Thus this creates a "safety buffer" in time before it "commits" the memory.


The next section details the Clock building process and assembly.  
  *   ➡️ **[Clock](../docs/Clock.md):**
---

  
