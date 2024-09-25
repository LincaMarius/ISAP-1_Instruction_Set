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

### LDA n 
0000 nnnn\
A ← [n]\
Loads the numeric value from address n into the accumulator.

### ADD n 
0001 nnnn\
A ← A + [n]\
Adds the numeric value at address n with the numeric value stored in the accumulator and stores the result in the accumulator.

### SUB n 
0010 nnnn\
A ← A – [n]\
Subtracts the numeric value at address n from the numeric value stored in the accumulator and stores the result in the accumulator.

### OUT x 
1110 xxxx\
OUT_Reg ← A\
Transfers the numeric value stored in the accumulator to the output register. It has no parameters, x can have any value and is treated as don’t care.

### HLT x 
1111 xxxx\
Stops further execution of computer instructions. It has no parameters, x can have any value and is treated as unimportant.

So, we have the instructions coded on the first 4 bits, leaving the next 4 bits to code the address of the operand in the case of the SAP-1 computer.

This means that a maximum of 2 ^ 4 = 16 memory locations can be accessed

We also notice that the instructions 0011, 0100, 0101, 0110, 0111, 1000, 1001, 1010, 1011, 1100, 1101 are not used




