# ISAP-1 Computer Instruction Set

This is the process of optimizing the functionality of the SAP-1 computer at the Instruction Set level, which I went through, presented step by step.

By: Lincă Marius Gheorghe.

Pitești, Argeș, România, Europe.

https://github.com/LincaMarius

## About the project, brief description

The goal of this project is to create a more efficient version of the SAP (Simple As Possible) computer, but with minimal changes so that the instruction format remains the same.

This Mini Project is part of a Project made by me:
https://github.com/LincaMarius/ISAP-1_Computer_Project

where I optimized the SAP-1 calculator step by step to create my own version called ISAP-1 (Improved SAP-1)

## Version 1
The original structure of the SAP-1 computer is:

![ Figure 1 ](/Pictures/Figure1.png)

The block diagram of the Central Processing Unit of the SAP-1 computer is:

![ Figure 2 ](/Pictures/Figure2.png)


### The original format of the SAP-1 computer instructions is:

| 4 bits instruction code   | 4 bits operand (memory address)          |
|---------------------------|------------------------------------------|

We notice that the upper nibble is used to encode an instruction.

So, any instruction is encoded on 4 bits.

Thus, we can have a maximum of 2 ^ 4 = 16 instructions.

### The original instruction set of the SAP-1 computer is:

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

This is the Complete Instructions Setfor the SAP-1 computer.

## NOP instruction – No OPeration
Binary form:  **** **** \
Operation:  no operation \
Example: NOP

The NOP instruction has only the Fetch portion present in all instructions, but has nothing in the execution portion of the instruction. \

The timing diagram for the NOP instruction is:

![ Figure 3 ](/Pictures/Figure3.png)

ALL UNIMPLEMENTED INSTRUCTIONS WILL BE TREATED BY THE SAP-1 CPU AS A NOP INSTRUCTION

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 1 ](/Tables/Table1.png)

Signals represented in Red: are active when data is written to the Data BUS \
Signals represented in Green: are active when reading data from the Data BUS \
Signals shown in Black: their activation has no influence on the Data BUS

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the NOP instruction is executed are:
-	EP = NOP * T1
-	LAR = NOP * T1
-	CP = NOP * T2
-	PM = NOP * T3
-	LI = NOP * T3

Since steps T1, T2 and T3 are present and identical in any instruction we can say that they are independent of the executed instruction so we can rewrite the instructions as follows:
-	EP = T1
-	LAR = T1
-	CP = T2
-	PM = T3
-	LI = T3

*If we implement the Control Block using Combinational Logic we will use these equations.*

## LDA instruction – LoaD the Accumulator
Binary form:    0000 nnnn \
Operation:      A ← [n] \
Example: LDA 9h 

Loads the numeric value from Address [n] into the Accumulator.

The timing diagram for the LDA instruction is as follows:

![ Figure 4 ](/Pictures/Figure4.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 2 ](/Tables/Table2.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS. 

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the LDA instruction is executed are:
-	EP = T1
-	LAR = T1 + LDA * T4
-	CP = T2
-	PM = T3 + LDA * T5
-	LI = T3
-	EI = LDA * T4
-	LA = LDA * T5

*If we implement the Control Block using Combinational Logic we will use these equations.*

## ADD instruction – ADD to accumulator
Binary form:  0001 nnnn \
Operation:    A ← A + [n] \
Example: ADD 8h 

Adds the numeric value at Address [n] with the numeric value stored in the Accumulator and stores the result in the Accumulator.

The timing diagram for the ADD instruction is as follows:

![ Figure 5 ](/Pictures/Figure5.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 3 ](/Tables/Table3.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the ADD instruction is executed are:
-	EP = T1
-	LAR = T1 + ADD * T4
-	CP = T2
-	PM = T3 + ADD * T5
-	LI = T3
-	EI = ADD * T4
-	LB = ADD * T5
-	EU = ADD * T6
-	LA = ADD * T6

*If we implement the Control Block using Combinational Logic we will use these equations.*

## SUB Instruction – SUBtract from accumulator
Binary form:  0010 nnnn \
Operation:  A ← A – [n] \
Example: SUB 5h

Subtracts the numeric value at address [n] from the numeric value stored in the Acumulator and stores the result in the Accumulator.

The timing diagram for the SUB instruction is as follows:

![ Figure 6 ](/Pictures/Figure6.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 4 ](/Tables/Table4.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the SUB instruction is executed are:
-	EP = T1
-	LAR = T1 + SUB * T4
-	CP = T2
-	PM = T3 + SUB * T5
-	LI = T3
-	EI = SUB * T4
-	LB = SUB * T5
-	EU = SUB * T6
-	LA = SUB * T6
-	SU = SUB * T6

*If we implement the Control Block using Combinational Logic we will use these equations.*

## OUT instruction – OUTput data from the accumulator
Binary form:  1110 pppp \
Operation:  PORT (p) ← A \
Example: OUT 1h

Transfers the numeric value stored in the Accumulator to Output Port. This instruction has no parameter.

The timing diagram for the OUT instruction is as follows:

![ Figure 7 ](/Pictures/Figure7.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 5 ](/Tables/Table5.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the OUT instruction is executed are:
-	EP = T1
-	LAR = T1
-	CP = T2
-	PM = T3
-	LI = T3
-	EA = OUT * T4
-	I/O = OUT * T4

*If we implement the Control Block using Combinational Logic we will use these equations.*

## The HLT instruction – HaLT computer
Binary form:  1111 ****\
Operation:  Halt computer \
Example: HLT

Stops further execution of computer instructions. It is an instruction with prefix 1111 and has no parameters. \

The timing diagram for the HLT instruction is as follows:

![ Figure 8 ](/Pictures/Figure8.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 6 ](/Tables/Table6.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the HLT instruction is executed are:
-	EP = T1
-	LAR = T1
-	CP = T2
-	PM = T3
-	LI = T3
-	HLT = HLT * T4 + HLT * T5 + HLT * T6

The control signal HLT is active starting from step T4 until the last step T6. In the original design of the SAP-1 computer, a timing diagram is not shown and this instruction is briefly described.
But from the circuit diagram you can see that gate C34 in the instruction decoder is active when we have the binary code 1111 corresponding to the HLT instruction. So, the control signal HLT is not influenced by the state of the T steps.
This fact can be presented graphically as in figure 8 where we see that for any step T4 – T6 the control signal is high.

*If we implement the Control Block using Combinational Logic we will use these equations.*
