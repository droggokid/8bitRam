# 8bit RAM Module

RAM is built from individual data cells. For instance:

- **2 GB RAM** = 2,000,000,000 bytes
- **2,000,000,000 bytes** = 16,000,000,000 bits

Thus, a 2 GB RAM would have **16,000,000,000 data cells**.

For this weekend project, I will design a basic 2-word, 8-bit RAM module, demonstrating the essential building blocks of RAM.

set regselect <0 or 1>: Changes the register selection (0 for Register A, 1 for Register B).

set bus <value>: Sets the 8-bit value (0–255) on the data bus.

write: Triggers the write operation. When this command is entered, writeRequested becomes true, and on the next simulation cycle the selected register is written with the current bus value.

print: Prints the mux output (i.e., the current read value from the selected register).

quit: Exits the simulation.

---

## Designing Data Cells

Each data cell is implemented using a **D-Type Flip-Flop** circuit, which is the fundamental memory element that can store a single bit.

### D-Type Flip-Flop Diagram

![D-Type Flip-Flop Diagram](diagrams/Datacell.png)  
_Figure 1: Schematic diagram of a D-Type Flip-Flop and Master-Slave Flip-Flop with a rising edge trigger_

### How the D-Type Flip-Flop Works

In this design, the flip-flop uses a Clock signal (CLK) to control data capture:

- **CLK high (Enable active):** The circuit is transparent, so the output (Q) follows the input (D).
- **CLK low (Enable inactive):** The flip-flop holds the previously captured value, regardless of changes in D.

This behavior ensures that data is reliably stored on the clock’s rising edge.

---

## Master-Slave Flip-Flop

A Master-Slave configuration improves reliability by using two flip-flops in sequence.

### Master-Slave Flip-Flop with Rising Edge Trigger

- The **master** flip-flop captures the input on the rising edge of the clock.
- The **slave** flip-flop updates its output on the falling edge.

This two-stage process prevents glitches due to propagation delays, ensuring the stored data remains stable until the next update.

---

## Video Demonstration of Data Cells

Watch this video to see the data cell (flip-flop) operation in action:  
[Data Cells Video](https://github.com/user-attachments/assets/422db3e2-5637-44bd-833b-c0d6cd82c5c4)

---

## Combining Data Cells to Form an 8-bit Register

Multiple flip-flops are grouped together to form a register that stores an 8-bit word.

### 8-bit Register Diagram

![8 bit register](https://github.com/user-attachments/assets/1051aaf2-aa8b-4828-9486-75ce46b43e9f)  
_Figure 2: Schematic diagram of an 8-bit register created by combining 8 D flip-flops_

### Video Demonstration of the 8-bit Register

View the demonstration:  
[8-bit Register Video](https://github.com/user-attachments/assets/da56aa87-3fd6-410b-afdf-152ff97ced3d)

---

## Write Mechanism

To write data to our RAM module, we need a way to select which register (word) to update.

### 1 to 2 Decoder

A 1-to-2 decoder uses one address bit to generate two outputs, each enabling one register.  
![Decoder and Registers](https://github.com/user-attachments/assets/a65fe184-0c07-4886-93b5-807342e8f5e9)

### Data Bus for Registers

A common 8-bit data bus feeds the same data to both registers. With the decoder controlling the write-enable, only the selected register will store the data during a clock pulse.  
![Data bus](https://github.com/user-attachments/assets/bfef1074-eaac-4a4c-bc5c-a2561ba6a2e7)

---

## Read Mechanism

To retrieve stored data, we use a multiplexer to select the output from the correct register.

### 2 to 1 Multiplexer

A 2-to-1 multiplexer, built from basic logic components, selects one of two 8-bit outputs (one from each register) based on a single select signal.  
![2 to 1 MUX](https://github.com/user-attachments/assets/58e679a3-ee45-4698-a7de-04d6c3522253)

---

## Complete Module

The complete RAM module integrates the data cells (registers), decoder for writing, and multiplexer for reading into a cohesive 2-word, 8-bit RAM system.  
![Complete circuit](https://github.com/user-attachments/assets/9b9e4865-793e-4204-bc82-2e0986ff8a8a)

---

## Complete Module video

https://github.com/user-attachments/assets/f9848a3d-4bcf-469e-83ea-c6e5bb603f0c

## In this video, I begin by setting the address to 0, which corresponds to Register A, and setting the data bus to 11110000. I then perform a write operation, and the output becomes 11110000. Next, I reset the data write signal and change the address to 1 (corresponding to Register B), then reset the data bus. I set the data bus to 00001111 and perform another write operation, which outputs 00001111. Finally, I reset the data bus again and switch back to address 0, demonstrating that the circuit's output returns to 11110000, this is the data stored in Register A.

This design demonstrates the core principles of memory:

- **Storage:** Using D-type flip-flops to store individual bits.
- **Organization:** Combining flip-flops into registers to form words.
- **Addressing:** Using a decoder to control write operations.
- **Retrieval:** Using a multiplexer to read out the correct word based on the address.

Feel free to expand or modify the design for additional memory words or bit widths.
