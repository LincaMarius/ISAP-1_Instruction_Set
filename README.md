# ISAP-1_Computer_Instruction_Set

This is the process of optimizing the functionality of the SAP-1 computer at the instruction set level, which I went through.

By: Lincă Marius Gheorghe.

Pitești, Argeș, România, Europe.

https://github.com/LincaMarius/ISAP-1_Computer_Instruction_Set

## About the project, brief description

The goal of this project is to create a more efficient version of the SAP (Simple As Possible) computer, but with minimal changes so that the instruction format remains the same.

This project is a continuation of another project made by me:\
https://github.com/LincaMarius/ISAP-Computer

where I optimized the SAP-1 calculator at the block diagram level.
I obtained four block diagrams:

The final block diagram of the CPU part of the system is: 

![ Figure 1 ](/Pictures/Figure1.png)

The final block diagram of the Computer Memory system is:

![ Figure 2 ](/Pictures/Figure2.png)

The final block diagram of the Input system of this computer is:

![ Figure 3 ](/Pictures/Figure3.png)

We will have an input device in the form of a binary keyboard.
Program memory bank register is an input-output device.

The final block diagram of the Output system of this computer is:

![ Figure 4 ](/Pictures/Figure4.png)

The output device is the old Output Register, but modified by having access to the address bus.
Thus, we can control the display format (unsigned, signed, hexadecimal, text), by sending commands to the output device on port 1, and data is sent on port 0.

## The original format of the SAP-1 computer instructions is:

| 4 bits instruction code   | 4 bits operand (memory address)          |
|---------------------------|------------------------------------------|


## The original instruction set of the SAP-1 computer is:

| Mnemonic | Opcode | Operation                                  |
|----------|--------|--------------------------------------------|
| LDA      | 0000   | Load RAM data into Accumulator             |
| ADD      | 0001   | Add RAM data to Accumulator                |
| SUB      | 0010   | Substract RAM data from accumulator        |
| OUT      | 1110   | Load Accumulator data into Output Register |
| HLT      | 1111   | Stop processing                            |

So, we have the instructions coded on the first 4 bits, leaving the next 4 bits to code the address of the operand in the case of the SAP-1 computer.

This means that a maximum of 2 ^ 4 = 16 memory locations can be accessed.

We also notice that the instructions Opcodes: 0011, 0100, 0101, 0110, 0111, 1000, 1001, 1010, 1011, 1100, 1101 are not used. So we can add 11 more new instructions.

### LDA instruction – Load the Accumulator
0000 nnnn\
A ← [n]\
Loads the numeric value from address n into the accumulator.\
Example: LDA 9h\








