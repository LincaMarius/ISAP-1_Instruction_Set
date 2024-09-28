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

## LDA instruction – Load the Accumulator
Binary form:    0000 nnnn \
Operation:      A ← [n] \
Loads the numeric value from Address n into the Accumulator. \
Example: LDA 9h 

The timing diagram for the LDA instruction is as follows:

![ Figure 5 ](/Pictures/Figure5.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 1 ](/Pictures/Table1.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS. 

<code style="color : red">If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.</code>

The Boolean equations for the signals that are active when the LDA instruction is executed are:
-	EP = LDA * T1
-	LAR = LDA * T1 + LDA * T3 = LDA * ( T1 + T3 )
-	CP = LDA * T2
-	PM = LDA * T2 + LDA * T4 = LDA * ( T2 + T4 )
-	LI = LDA * T2
-	EI = LDA * T3
-	LA = LDA * T4

<code style="color : red">If we implement the Control Block using Combinational Logic we will use these equations.</code>

## ADD instruction – Add to accumulator
Binary form:  0001 nnnn\
Operation:    A ← A + [n]\
Adds the numeric value at address n with the numeric value stored in the accumulator and stores the result in the accumulator. \
Example: ADD 8h 

The timing diagram for the ADD instruction is as follows:

![ Figure 6 ](/Pictures/Figure6.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 2 ](/Pictures/Table2.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

<code style="color : red">If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.</code>
-	EP = ADD * T1
-	LAR = ADD * T1 + ADD * T3 = ADD * ( T1 + T3 )
-	CP = ADD * T2
-	PM = ADD * T2 + ADD * T4 = ADD * ( T2 + T4 )
-	LI = ADD * T2
-	EI = ADD * T3
-	LB = ADD * T4
-	EU = ADD * T5
-	LA = ADD * T5

<code style="color : red">If we implement the Control Block using Combinational Logic we will use these equations.</code>

## SUB Instruction – Subtract from accumulator
Binary form:  0010 nnnn \
Operation:  A ← A – [n] \
Subtracts the numeric value at address n from the numeric value stored in the accumulator and stores the result in the accumulator. \
Example: SUB 5h

The timing diagram for the SUB instruction is as follows:

![ Figure 7 ](/Pictures/Figure7.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 3 ](/Pictures/Table3.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

<code style="color : red">If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.</code>

