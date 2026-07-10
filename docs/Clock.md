## Clock
![Clock](../images/clock-schematic.png)
![Clock](../images/clock.jpeg)

- If NOT gate goes to the master, it is rising edge triggered.
-  If NOT gate goes to slave it is falling edge triggered.

uth Table: Falling Edge Accumulator (End-of-Cycle)

|Clock Signal|Data Input|Load Enable|Master Internal|Slave Output (Q)|Meaning|
|---|---|---|---|---|---|
|**High (1)**|**Change**|**1**|**Follows Input**|**No Change**|**"Thinking Time"** (ALU calculates, Master listens, Output is stable).|
|**Falling (**$\downarrow$**)**|**Stable**|**1**|**Locks**|**Updates to Input**|**"Latching Time"** (The update happens here, at the end).|
|**Low (0)**|X|X|**Locked**|**Stable**|**Safe State** (Waiting for next cycle).|
The decision to use **Falling Edge** triggering for the Accumulator is a specific engineering choice designed to solve the problem of **"ALU Ripple Delay."**

Here is the breakdown of why this strategy is smarter for a discrete breadboard computer.

### 1. The Problem: The ALU is "Slow"

Your ALU is a Ripple Carry Adder. This means it doesn't calculate the answer instantly. It calculates Bit 0, then passes a carry to Bit 1, then to Bit 2, then to Bit 3.

- **During the Calculation (The Glitch Phase):** While the carry signal is moving through the bits, the ALU output is **wrong**.
    
    - _Example:_ If calculating `1 + 1`, the output might briefly flash `3` or `0` before settling on `2`.
        
- **The Risk:** If you grab the data too early (e.g., on the Rising Edge at the start of the cycle), you might accidentally save one of these intermediate "glitch" numbers instead of the final answer.
    

### 2. The Solution: "Wait and See" (Falling Edge)

By using the **Falling Edge** logic shown in your Truth Table:

- **Clock HIGH ("Thinking Time"):** The Master Latch stays **OPEN**. It acts like a window. As the ALU calculates and glitches (`0... 3... 2...`), the Master Latch sees all of it. It effectively says, _"I'm watching, but I'm not saving yet."_
    
    - This uses the entire duration of the clock pulse (e.g., 0.5 seconds) to let the electricity settle down.
        
- **Clock FALLING ("The Decision"):** The clock turns off. The Master Latch **SLAMS SHUT**.
    
    - By this time, the ALU is definitely finished. The number sitting in the latch is guaranteed to be the stable, correct answer (e.g., `2`).
        
    - The Slave Latch opens and outputs this stable `2` to the rest of the computer.
        

### Summary

He uses Falling Edge because it automatically creates a **"Safety Buffer"** of time. It forces the computer to wait until the very end of the heartbeat—when everything is calm and stable—before committing the answer to memory.

 > [!example]- Bit 4 gave same output as clock (Des 15)
>
> **Issue:**  I use jumper wire for ground dind make nice connection---
---

