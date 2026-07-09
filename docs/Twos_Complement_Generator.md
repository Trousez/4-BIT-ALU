## Two's Complement Generator

- *The Concept: This is the mathematical rule: To make a number negative, invert all bits and add 1*
- The Hardware: Controlled Inverter (XOR Gates)

    - **Invert:** The **XOR gates** act as programmable inverters. When the `SUBTRACT` line is On, they flip the B-input bits.
        
    - **Add One:** The same `SUBTRACT` line connects to the **Carry-In** ($C_{in}$) of the first Adder. This adds the necessary "$+1$" to the calculation.
 
    
