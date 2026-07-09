## Two's Complement Generator

- *The Concept: This is the mathematical rule: To make a number negative, invert all bits and add 1*
- The Hardware: Controlled Inverter (XOR Gates)

    - **Invert:** The **XOR gates** act as programmable inverters. When the `SUBTRACT` line is On, they flip the B-input bits.
        
    - **Add One:** The same `SUBTRACT` line connects to the **Carry-In** ($C_{in}$) of the first Adder. This adds the necessary "$+1$" to the calculation.
 
     ![two-complement](../images/Subtract_1.png)

     ![two-complement](../images/two-complement.jpeg)

- Note that the Subtract input signal is divided into two input buffer to "divide and conquer". One input buffer drive bit 0 and 1. The other drives bit 2 and 3.
- The subtract input signal also goes to the carry in of the 4-bit adder. This acts as the "+1" operation of the Two's complement
      ![ALU](../images/ALU_1.png)
      ![ALU](../images/ALU_1.png)

- Stress testing the ALU with the calulation 0 - 1 = 15 shows that the ALU circuit draws roughly 100mA.
- With the bus and Tri state buffer it is 130 mA.

  
    
