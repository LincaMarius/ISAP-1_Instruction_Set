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

## ISAP-1 Computer Instruction Set revision 1

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

## FETCH an instruction
The SAP-1 computer executes an instruction in multiple steps called “Microsteps”.

Their number is chosen to allow the complete execution of the most complex instruction.

The SAP-1 computer needs 6 “Steps” to execute the ADD and SUB instructions.

Any Instruction has a FETCH portion, during which it is loaded from RAM into the Instruction Register, followed by the actual execution portion of the instruction.

The NOP instruction is not included in the SAP-1 computer's Instruction Set but must be studied because, as we have shown previously, the codes 03h to 0Dh are equivalent to the NOP instruction for the SAP-1 computer.

## NOP instruction – No OPeration
Binary form:  **** **** \
Operation:  no operation \
Example: NOP

The NOP instruction has only the Fetch portion (present in all instructions), but has nothing in the execution portion of the instruction.

We do not have the NOP instruction on the SAP-1 computer, but we need to study it because all 11 unimplemented instruction codes will be treated by the control unit and implicitly by the SAP-1 computer as a NOP instruction.

The original timing diagram for the NOP instruction of the SAP-1 computer is:

![ Figure 1 ](/Pictures/Figure1.png)

ALL UNIMPLEMENTED INSTRUCTIONS WILL BE TREATED BY THE ISP-1 CPU AS A NOP INSTRUCTION!

All instructions of the SAP-1 computer are executed in 6 steps noted in the diagram and wiring diagram T1 - T6. The first 3 steps are the Fetch portion and the last 3 are the Execution portion of the instruction. The Fetch part of the statement is identical for all statements. The Execution part is specific to each individual instruction.

We can summarize the value of the control signals over time presented in these diagrams in the following table:

![ Table 1 ](/Tables/Table1.png)

Signals represented in Red: *are active when data is written to the Data BUS* \
Signals represented in Green: *are active when reading data from the Data BUS* \
Signals shown in Black: *their activation has no influence on the Data BUS*

If we put all the output signals on columns and highlight the control signals used by the NOP instruction we obtain the Truth Table for the NOP instruction for the SAP-1 computer.

![ Table 2 ](/Tables/Table2.png)

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

### LDA instruction – LoaD the Accumulator
Binary form:    0000 nnnn \
Operation:      A ← [n] \
Example: LDA 9h 

Loads the numeric value from Address [n] into the Accumulator.

The timing diagram for the LDA instruction implemented on SAP-1 Computer is as follows:

![ Figure 2 ](/Pictures/Figure2.png)

We can summarize the value of the control signals over time presented in these diagrams in the following table:

![ Table 3 ](/Tables/Table3.png)

Signals represented in Red: *are active when data is written to the Data BUS* \
Signals represented in Green: *are active when reading data from the Data BUS* \
Signals shown in Black: *their activation has no influence on the Data BUS*

If we put all the output signals on columns and highlight the control signals used by the LDA instruction we obtain the Truth Table for the LDA instruction for the SAP-1 computer.

![ Table 4 ](/Tables/Table4.png)

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the LDA instruction is executed for computer SAP-1 are:
-	EP = T1
-	LAR = T1 + LDA * T4
-	CP = T2
-	PM = T3 + LDA * T5
-	LI = T3
-	EI = LDA * T4
-	LA = LDA * T5

*If we implement the Control Block using Combinational Logic we will use these equations.*

### ADD instruction – ADD to accumulator
Binary form:  0001 nnnn \
Operation:    A ← A + [n] \
Example: ADD 8h 

Adds the numeric value at Address [n] with the numeric value stored in the Accumulator and stores the result in the Accumulator.

The timing diagram for the ADD instruction implemented on SAP-1 Computer is as follows:

![ Figure 3 ](/Pictures/Figure3.png)

We can summarize the value of the control signals over time shown in these diagrams in the following table:

![ Table 3 ](/Tables/Table3.png)

Signals represented in Red: *are active when data is written to the Data BUS* \
Signals represented in Green: *are active when reading data from the Data BUS* \
Signals shown in Black: *their activation has no influence on the Data BUS*

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the ADD instruction is executed for computer SAP-1 are:
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

Subtracts the numeric value at Address [n] from the numeric value stored in the Acumulator and stores the result in the Accumulator.

The timing diagram for the SUB instruction implemented on SAP-1 Computer is as follows:

![ Figure 4 ](/Pictures/Figure4.png)

We can summarize the value of the control signals over time presented in this diagram in the following table:

![ Table 4 ](/Tables/Table4.png)

Signals represented in Red: *are active when data is written to the Data BUS* \
Signals represented in Green: *are active when reading data from the Data BUS* \
Signals shown in Black: *their activation has no influence on the Data BUS*

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the SUB instruction is executed for computer SAP-1 are:
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
Binary form:  1110 **** \
Operation:  PORT (*) ← A \
Example: OUT *

Transfers the numeric value stored in the Accumulator to Output Port. This instruction has no parameter.

Since in the case of the SAP-1 computer we only have a single output port, it is activated using any value for the instruction parameter.

The timing diagram for the OUT instruction implemented on SAP-1 Computer is as follows:

![ Figure 5 ](/Pictures/Figure5.png)

We can summarize the value of the control signals over time presented in this diagram in the following table:

![ Table 5 ](/Tables/Table5.png)

Signals represented in Red: *are active when data is written to the Data BUS* \
Signals represented in Green: *are active when reading data from the Data BUS* \
Signals shown in Black: *their activation has no influence on the Data BUS*

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the OUT instruction is executed for computer SAP-1 are:
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

Stops further execution of computer instructions.

The timing diagram for the HLT instruction implemented on SAP-1 Computer is as follows:

![ Figure 6 ](/Pictures/Figure6.png)

We can summarize the value of the control signals over time presented in this diagram in the following table:

![ Table 6 ](/Tables/Table6.png)

Signals represented in Red: *are active when data is written to the Data BUS* \
Signals represented in Green: *are active when reading data from the Data BUS* \
Signals shown in Black: *their activation has no influence on the Data BUS*

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the HLT instruction is executed for computer SAP-1 are:
-	EP = T1
-	LAR = T1
-	CP = T2
-	PM = T3
-	LI = T3
-	HLT = HLT * T4 + HLT * T5 + HLT * T6

The control signal HLT is active starting from step T4 until the last step T6. In the original design of the SAP-1 computer, a timing diagram is not shown and this instruction is briefly described.

But from the schematic you can see that gate C34 in the instruction decoder is active when we have the binary code 1111 corresponding to the HLT instruction. So, the control signal HLT is not influenced by the state of the T steps.

This fact can be presented graphically as in [Figure 6](/Pictures/Figure6.png) where we see that for any step T4 – T6 the control signal is high.

Because the HALT instruction blocks the clock signal and the computer can no longer function, exiting this state can only be done by resetting the computer.

*If we implement the Control Block using Combinational Logic we will use these equations.*
