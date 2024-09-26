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

|4 bits instruction code | 4 bits operand (memory address)|

## The original instruction set of the SAP-1 computer is:

### LDA n 
|Mnemonic|Op code|Format|Data|Operation|Description|
|--------------------------------------------------|
|LDA|0000|0000 nnnn|Memory|A ← [n]|Loads the numeric value from address n into the accumulator|

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

We also notice that the instructions 0011, 0100, 0101, 0110, 0111, 1000, 1001, 1010, 1011, 1100, 1101 are not used. So we can add 11 more new instructions.

## The instruction to load an immediate numeric value into the Accumulator

The first instruction we can implement is one that allows us to load an immediate numeric value into the Accumulator.

But the size of the operand is 4 bits and thus we can load a numerical value that can only be represented on the 4 least significant bits. This number can have values ​​between 0 and 15.

I propose to implement two distinct instructions. The first loads the numeric value for the least significant 4 bits and the second loads the numeric value for the most significant 4 bits of the Accumulator register.

### LDL n
0011 nnnn\
A[4-0] ← Imm\
Loads the numeric value for the least significant 4 bits of the Accumulator

### LDH n
0100 nnnn\
A[7-5] ← Imm\
Loads the numeric value for the most significant 4 bits of the Accumulator
