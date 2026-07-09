## ALU TRI-STATE BUFFERS

| Enable Pin   | Data Input | Output Voltage | Logic State               |
| ------------ | ---------- | -------------- | ------------------------- |
| **5V** (OFF) | Any        | **5V**         | **High-Z** (Disconnected) |
| **0V** (ON)  | 0V         | **5V**         | **Logic 1** (Disconnected) |
| **0V** (ON)  | 5V         | **16.9 mV**     | **Logic 0** (Grounded)     |

- 16.9mV found according to LTSpice

- **Logic 1:** The gate connects the wire directly to **5V**. (Strong push).
- **Logic 0:** The gate connects the wire directly to **Ground**. (Strong pull).
![LTSpice-tri-state](../images/Ltspice-tri-state.png)

