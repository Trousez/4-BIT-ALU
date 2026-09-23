## ALU Tri-State Buffers & Bus Architecture

The Arithmetic Logic Unit (ALU) interfaces with the shared system bus using active-low, open-collector buffers. This architecture provides high-impedance isolation when disabled, preventing catastrophic bus contention and ensuring deterministic logic levels across all connected modules.

---
### Bus Contention & Open-collector Architecture

Connecting multiple modules (such as the ALU and Accumulator Register) directly to a shared bus using, for example, push-pull outputs introduces severe hardware risk:

* **Bus Contention:** If Module A outputs a high state ($5\text{ V}$) while Module B simultaneously asserts a low state ($0\text{ V}$), a dead short from power to ground occurs. The resulting overcurrent spike destroys output transistors and collapses rail voltages.
  
 **Open-Collector Solution:** 
 Instead of actively sourcing $5\text{ V}$, the buffer only actively sinks current to ground (**Logic 0**) or floats entirely (**High-Z**).
- **Passive Bus Pull-Ups:** Dedicated pull-up resistors on the shared bus define the default quiescent state at $5\text{ V}$ (**Logic 1**). A module transmits by releasing the line to float high or clamping it to ground.

### ALU Tri-State Buffer (Active-Low, Inverting)

| $\overline{\text{Enable}}$ Pin | Data Input | Output Transistor State | Bus Voltage ($V_{\text{BUS}}$) | Resulting Bus Logic | Drive Mode |
| :---: | :---: | :---: | :---: | :---: | :--- |
| **$5\text{ V}$ (Disabled)** | Any | Cutoff (High-Z) | $5.0\text{ V}$ | *Released* | Disconnected (Passive Pull-Up) |
| **$0\text{ V}$ (Enabled)** | **$5\text{ V}$ (Logic 1)** | Cutoff (High-Z) | $5.0\text{ V}$ | **Logic 1** | Module Releases Line (Bus Pulls High) |
| **$0\text{ V}$ (Enabled)** | **$0\text{ V}$ (Logic 0)** | Saturated to GND | $16.9\text{ mV}$* | **Logic 0** | Active Sink ($V_{\text{CE(sat)}}$ via LTspice) |

- 16.9mV found according to LTSpice*

<p align="center">
  <img src="../images/Ltspice-tri-state.png" alt="LTSpice Tri-State Buffer Simulation" width="600">
</p>

The buffer uses an active-low enable control pin ($\overline{\text{Enable}}$). The output stage translates binary ALU calculations into bus-safe states:

* **Active-Low Enable ($\overline{\text{Enable}} = 0\text{ V}$):** The buffer is active. An input of $5\text{ V}$ cuts off the output transistor, allowing the bus pull-up resistor to pull the line to $5.0\text{ V}$ (**Logic 1**). An input of $0\text{ V}$ drives the output transistor into saturation, clamping the line to $V_{\text{CE(sat)}} \approx 16.9\text{ mV}$ (**Logic 0**).
* **Active-Low Disable ($\overline{\text{Enable}} = 5\text{ V}$):** The output transistor is forced into cutoff (High-Z) regardless of data inputs, releasing the bus line for other functional units.

- All Bus data lines' initial state is `ON`, because of Pull-up resistors connected to Bus lines (thus providing a path to ground turns the data line off)
        
>### Error Log: Thermal Bridging and Coupled Logic on Bits 4 and -8
>
> **Issue:** The bus bits 4 and -8 display the exact same result, as if they are connected somehow.
>
> **Solution/Cause:** Using a multimeter confirmed there is continuity between these two bits (meaning a short circuit). After further inspection, it revealed the breadboard melted, causing a short circuit. The place where the breadboard melted was the same place a jumper cable was placed. I assume the jumper didn't make sufficient contact, which caused high-resistance mechanical termination. This created excessive heat under load and melted the surrounding plastic, causing a short circuit (at least that is the only cause I can think of). I replaced the breadboard and used solid core wire instead, ensuring a proper connection.

<p align="center">
  <img src="../images/tri-state.png" alt="Discrete Tri-State Buffer Schematic" width="400">
</p>


The next section details the Register building process and assembly.

 ➡️ **[Registers](../docs/Registers.md):** 
 
---


