## Buffer and Inverter


![Buffer and Inverter](../images/buffer-and-inverter-built.jpeg)
![Input Buffer](../images/input-buffer-diagram.png)

- Buffer re-outputs a clean, strong signal. This is used when signals start to get weak or is heavily loaded. Wires (especially jumper cables) induce parasitic capacitance that rounds the square waves. Buffers restore this since it uses its own $V_{CC}$ and ground rails to source or sink current. This can be mathimatically represented as follows:

$$A = A$$

- One also gets Tri-state buffers (which will be looked at in a later section), that is used to prevent circuits from shorting when interconnected i.e. connected on a bus.

### Buffer 2
   ![Buffer 2](../images/buffer-2.png)

Buffers can be created by connecting two inverters in series.

---

### OR GATE

| A   | B   | Output |
| --- | --- | ------ |
| 0   | 0   | 0      |
| 0   | 1   | 1      |
| 1   | 0   | 1      |
| 1   | 1   | 1      |

An OR Gate can be mathematically represented as follows:

$$A+B$$


![OR 1](../images/OR_1.jpeg)
![OR 2](../images/OR_2.jpeg)

- Notice that there are two OR gates shown. Number 1 looks simpler (uses less transistors) but OR gate number 2 is much more useful when using it in tangent with other logic gates (when other logic gates are connected to it)
- Notice in OR 2 the LED's cathode is directly connected in ground, while in OR 1 the cathode is connected to the collector of the transistor, this characteristic is show with all the other gates since with OR gate 2, one can easiliy connect it to the input of another logic gate.
---
### AND GATE 

| A   | B   | Output |
| --- | --- | ------ |
| 0   | 0   | 0      |
| 0   | 1   | 0      |
| 1   | 0   | 0      |
| 1   | 1   | 1      |

![AND1_GATE](../images/AND_1.jpeg)
![AND2_GATE](../images/AND_2.png)

---
### NAND GATE

| A   | B   | Output |
| --- | --- | ------ |
| 0   | 0   | 1      |
| 0   | 1   | 1      |
| 1   | 0   | 1      |
| 1   | 1   | 0      |
- A NAND Gate is an inverted AND gate. It is mathematically represent as:

$$(A \cdot B)'$$

- A NAND gate can be used to create any other type of logic gate, combinded with the fact that it only takes 2 transistors to built, most of the computer ( and most modern computer) consists entirely of NAND gate.


![NAND_GATE](../images/NAND.jpeg)

De Morgan's Laws provide the mathematical bridge that explains how a NAND gate converts into other operations (like OR and NOR):
$$\overline{A \cdot B} = \overline{A} + \overline{B}$$
$$\overline{A + B} = \overline{A} \cdot \overline{B}$$
In hardware synthesis, the first theorem transforms inverted-AND logic directly into negative-OR logic:NOT Gate: Tie the inputs together: $\overline{A \cdot A} = \overline{A}$.AND Gate: Invert a NAND output using a second NAND configured as an inverter: $\overline{\overline{A \cdot B}} = A \cdot B$.OR Gate (via De Morgan’s): Invert inputs $A$ and $B$ before feeding them into a NAND gate:$$\overline{\overline{A} \cdot \overline{B}} = \overline{\overline{A}} + \overline{\overline{B}} = A + B$$NOR Gate: Invert the output of the NAND-constructed OR gate.What Else to Include in an ALU Build LogTo make your documentation thorough and practical, add these hardware realities:XOR Gate Synthesis (Core of the ALU Adder):The arithmetic core of an ALU (the full adder) relies heavily on XOR logic for sum bits ($A \oplus B$). Building an XOR gate from NANDs takes exactly 4 NAND gates:$$A \oplus B = \overline{\overline{A \cdot \overline{A \cdot B}} \cdot \overline{B \cdot \overline{A \cdot B}}}$$

---
### XOR GATE
_Description: Crucial for the Adder circuit. Built using 6 transistors._

|  A  |  B  | Output |
| :-: | :-: | :----: |
|  0  |  0  |   0    |
|  0  |  1  |   1    |
|  1  |  0  |   1    |
|  1  |  1  |   0    |

![XOR_gate](../images/XOR.jpeg)

- With this Gate swwitches were not included, rather resistors were used to act as the input switches(ones simply removes thh resistors to "turn off" the switch) I did not include switches for this one. Pulling out the 2 resistors furhest to the left acts as the switch)

![Types_of_logic_gates](../images/logic-gates.png)


The next section details the Adder building process and assembly

➡️ [The Adder](docs/The_Adder.md)

---






