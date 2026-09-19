## Clock

The clock is a Astable multivibrator. It includes Output buffer/Inverters to cleanly output a square pulse to the Bus, and hence to the rest of the circuits.

### How It Oscillates:
* It has no stable state (astable). It continuously flip-flops between two quasi-stable states through capacitive feedback.
* When one transistor turns on, its collector drops to ground, driving the opposite transistor’s base negative via the cross-coupled $10\ \mu\text{F}$ capacitor, cutting it off.
* The capacitor then slowly charges up toward $5\text{ V}$ through the $100\text{ k}\Omega$ base resistor until the base reaches $\sim 0.7\text{ V}$, turning that transistor back on.
* This action forces the first transistor off, repeating the cycle on the other side.

<p align="center">
  <img src="../images/Clock_LTSpice.jpg" alt="Clock LTspice" width="800">
</p>


The Green wave indicates the `CLK` Output, while the Blue wave indicates the inverted `CLK_INV` output. The simulations indicate a period of 1.34s (0.74Hz) and a 50% duty cycle. This low Hz was chosen for easy debugging during the final stages of assembly.
<p align="center">
  <img src="../images/Clock_Timing.jpg" alt="Clock timing diagram" width="900">
</p>

 > ### Error Log: Bit 4 gave same output as clock
>
> **Issue:**  I used a jumper wire for Ground but it didn't make a good connection. I swapped it with a solid-core wire and this fixed the issue.
---

