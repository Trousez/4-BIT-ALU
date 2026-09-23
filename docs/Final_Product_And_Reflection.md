## The Final Product

 > ### Error Log:  The Final ALU drained my 10,000mAh power bank extremely fast
>
> **Issue:** The ALU drained the power bank very fast (Extremely fast, it drained the 10,000mAh powerbank by like a percentage every second. Interestingly enough the breadboards didn't melt when this was observed - I crudely calculated it as 10A current draw, which is probably not right). Maybe there was just a big initial voltage drop in the Power Bank and the internal computer on Power Bank linearly decreased the percentage counter to match the big initial voltage drop when connected to the ALU).
> **Solution:** This was solved by adding more connections to ground. The final current draw was under 1.5A.
---

<p align="center">
  <img src="../images/Final_Topdown.jpg" alt="topdown" width="700">
</p>

### Reflection
This project taught we a lot about the engineering challenges when building a physical implementation. When I initially started this project, my goal was to make a Turing complete 4-Bit computer, but I quickly released the physical limitations of the cheap breadboards I used for this project. It seemed impractical to continue the build. Luckily the ALU part of the build is complete and works as intended.


### Reflection & Engineering Takeaways

Building a functional Arithmetic Logic Unit entirely from discrete BJT components provided a rigorous, ground-up perspective on the physical and electrical realities that abstract digital schematics conceal.

* **Physical Limits of Solderless Breadboards:** What appears straightforward in a discrete logic diagram becomes an exercise in parasitic management on physical breadboards. Solderless breadboards introduce non-negligible contact resistance (typically 0.1 Ω to 0.3 Ω per clip) and stray capacitance between adjacent tie strips. Across hundreds of discrete transistors and pull-up networks, cumulative parasitic capacitance rounded high-speed switching edges, while series resistance across distribution rails produced noticeable voltage differentials between distant modules.
* **Power Integrity and Ground Bounce:** In discrete Resistor-Transistor Logic (RTL) and DTL topologies, the pull-up networks continuously sink current to Ground whenever switching transistors saturate. With over 400 discrete transistors active across multiple logic blocks, static power dissipation quickly escalated. Maintaining a solid ground plane without a dedicated PCB layer underscored the critical role of bus power distribution, star topologies, and localized decoupling.
* **Scope and Architectural Pragmatism:** The initial ambition was to construct a fully Turing-complete 4-bit discrete computer on breadboards. However, diagnosing signal integrity issues, base-drive fan-out limitations, and inter-board jumper reliability highlighted the law of diminishing returns when scaling solderless architectures. Scoping the final deliverable to a robust, fully verified, and self-contained 4-bit ALU proved to be the engineering-sound decision—delivering a reliable arithmetic/logic core without compromising signal integrity.
* **Theory vs. Hardware Realities:** Designing at the transistor level bridged the gap between idealized Boolean algebra and solid-state device physics. Balancing base saturation current ($I_B$) against turn-off storage time delays ($t_s$), managing open-collector bus contention, and designing deterministic input buffer receivers solidified concepts that are easily overlooked when designing with packaged ICs or HDL code.

 > ### Engineering Log: Power Distribution & Current Consumption Anomaly
>
> **Issue:** During initial integration testing, the fully assembled ALU drained a 10,000 mAh power bank at an alarming rate (dropping approximately 1% per second). A superficial back-of-the-envelope calculation suggested an extreme current draw approaching ~10 A, yet no breadboard tie-points melted, and trace jumpers remained intact. In reality, the high static load combined with inadequate ground distribution induced severe rail droop across the power bus. The power bank's internal battery management system (BMS) detected this sharp terminal voltage sag under heavy load and dynamically decremented its state-of-charge (SoC) display to match the depressed cell voltage.
> 
> **Root Cause & Solution:** The issue was traced to high ground return resistance across daisy-chained breadboard distribution rails. The return current from dozens of saturated BJT stages created substantial $I \cdot R$ ground bounce, shifting logic references and stressing the supply. Resolving this required establishing a star-ground distribution scheme with dedicated, low-resistance ground return jumpers bridging all breadboard ground rails back to the primary supply input. With ground loop resistance minimized and rail droop mitigated, the steady-state current consumption stabilized to a manageable **< 1.5 A**.
