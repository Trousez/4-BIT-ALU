## Buffer and Inverter


![Buffer and Inverter](../images/buffer-and-inverter-built.jpeg)
![Input Buffer](../images/input-buffer-diagram.png)

- Buffer reoutputs a clean, strong singal. This is used when signal start to get weak or is heavily loaded.

- **The Problem:** The data bus is "weak." It relies on pull-up resistors to hold the voltage at 5V. If you connect the bus directly to a complex circuit like the ALU, the ALU draws so much current that it sucks the voltage down (e.g., to 2.5V).
    
- **The Buffer's Job:** It acts as a **Power Amplifier**. It gently "tastes" the weak bus voltage (High Impedance) and uses its own direct connection to the battery to blast a strong, high-current copy of that signal into the ALU.

### Buffer 2


