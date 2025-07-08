# ISAP-1 Computer Instruction Set
The improved version of the SAP-1 computer.

This is the process of optimizing the functionality of the SAP-1 computer at the Instruction Set level, which I went through, presented step by step.

By: Lincă Marius Gheorghe,

https://github.com/LincaMarius

## About the project, brief description

The goal of this project is to create a more efficient version of the SAP (Simple As Possible) computer, but with minimal changes so that the instruction format remains the same.

This project is part of another project made by me: \
https://github.com/LincaMarius/ISAP-1_Computer_Project

where I optimized the SAP-1 calculator step by step to create my own version called ISAP-1 (Improved SAP-1).

## ISAP-1 Computer Instruction Set Revision 1
The original structure of the SAP-1 computer is identical to the structure of the ISAP-1 computer:

![ Figure 1 ](/Pictures/Figure1.png)

The original Block Diagram of the Central Processing Unit of the SAP-1 computer is identical to that of the ISAP-1 computer:

![ Figure 2 ](/Pictures/Figure2.png)

### Instruction format
The original format of the SAP-1 computer instructions is:

| 4 bits instruction code   | 4 bits operand (memory address)          |
|---------------------------|------------------------------------------|

We notice that the upper nibble is used to encode an instruction.

So, any instruction is encoded on 4 bits.

Thus, we can have a maximum of 2 ^ 4 = 16 instructions.

### The Instruction Set of the SAP-1 computer
The original instruction set of the SAP-1 computer is:

| Mnemonic  | Opcode | Operation                                  |
|-----------|--------|--------------------------------------------|
| LDA n     | 0000   | Load RAM data into Accumulator             |
| ADD n     | 0001   | Add RAM data to Accumulator                |
| SUB n     | 0010   | Substract RAM data from Accumulator        |
| OUT *     | 1110   | Load Accumulator data into Output Register |
| HLT *     | 1111   | Stop processing                            |

We can see that the following opcodes: 0011, 0100, 0101, 0110, 0111, 1000, 1001, 1010, 1011, 1100, 1101 are not used. So, we can add 11 more new instructions.

These codes are treated by the SAP-1 computer as NOP instructions, the previous table can be completed as follows:

| Mnemonic | Opcode | Operation                                  |
|----------|--------|--------------------------------------------|
| LDA      | 0000   | Load RAM data into Accumulator             |
| ADD      | 0001   | Add RAM data to Accumulator                |
| SUB      | 0010   | Substract RAM data from accumulator        |
| NOP      | 0011   | No Operation                               |
| NOP      | 0100   | No Operation                               |
| NOP      | 0101   | No Operation                               |
| NOP      | 0110   | No Operation                               |
| NOP      | 0111   | No Operation                               |
| NOP      | 1000   | No Operation                               |
| NOP      | 1001   | No Operation                               |
| NOP      | 1010   | No Operation                               |
| NOP      | 1011   | No Operation                               |
| NOP      | 1100   | No Operation                               |
| NOP      | 1101   | No Operation                               |
| OUT      | 1110   | Load Accumulator data into Output Register |
| HLT      | 1111   | Stop processing                            |

*This is the complete Instruction Set for the SAP-1 computer and for the ISAP-1 computer version 1.0*

So we have the instructions encoded in the first 4 bits, leaving the next 4 bits for encoding the instruction parameter which represents a memory address where the operand is located in the case of the SAP-1 computer.

### NOP instruction – No OPeration
Binary form:  **** **** \
Operation:  no operation \
Example: NOP

The NOP instruction has only the Fetch portion (present in all instruction), but has nothing in the execution portion of the instruction.

We do not have the NOP instruction on the SAP-1 computer but we need to study it because all 11 unimplemented instruction codes will be treated by the Control Unit and implicitly by the SAP-1 computer as the NOP instruction.

The original timing diagram for the NOP instruction of the SAP-1 computer is:

![ Figure 3 ](/Pictures/Figure3.png)

All instructions of the SAP-1 computer are executed in 6 steps marked in the diagram and in the electrical diagram T1 - T6. The first 3 steps are the Fetch portion and the last 3 are the Execution portion of the instruction. The Fetch portion of the instruction is identical for all instructions. The Execution portion is specific to each individual instruction.

ALL UNIMPLEMENTED INSTRUCTIONS WILL BE TREATED BY THE SAP-1 CPU AS A NOP INSTRUCTION

We can summarize the value of the control signals over time presented in these diagrams in the following table:

![ Table 1 ](/Tables/Table1.png)

Signals represented in Red: *are active when data is written to the Data BUS* \
Signals represented in Green: *are active when reading data from the Data BUS* \
Signals shown in Black: *their activation has no influence on the Data BUS*

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the NOP instruction is executed for computer SAP-1 are:
-	EP = NOP * T1
-	LAR = NOP * T1
-	CP = NOP * T2
-	PM = NOP * T3
-	LI = NOP * T3

Since steps T1, T2 and T3 are present and identical in any instruction we can say that they are independent of the executed instruction so we can rewrite the equations as follows:
-	EP = T1
-	LAR = T1
-	CP = T2
-	PM = T3
-	LI = T3

*If we implement the Control Block using Combinational Logic we will use these equations.*
