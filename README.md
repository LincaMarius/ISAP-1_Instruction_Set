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

4 bits instruction code + 4 bits operand (memory address)

## The original instruction set of the SAP-1 computer is:

### LDA n\
0000 nnnn\
A ← [n]\
Loads the numeric value from address n into the accumulator.

### ADD\
0001 nnnn\
A ← A + [n]\
Adds the numeric value at address n with the numeric value stored in the accumulator and stores the result in the accumulator.





