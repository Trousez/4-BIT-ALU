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

