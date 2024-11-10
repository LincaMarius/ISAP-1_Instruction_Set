# ISAP-1 Computer Instruction Set

This is the process of optimizing the functionality of the SAP-1 computer at the Instruction Set level, which I went through, presented step by step.

By: Lincă Marius Gheorghe.

Pitești, Argeș, România, Europe.

https://github.com/LincaMarius

## About the project, brief description

The goal of this project is to create a more efficient version of the SAP (Simple As Possible) computer, but with minimal changes so that the instruction format remains the same.

This project is a continuation of another project made by me:\
https://github.com/LincaMarius/ISAP-Computer

where I optimized the SAP-1 computer at the block diagram level.

The final structure of the ISAP-1 computer is:

![ Figure 1 ](/Pictures/Figure1.png)

The block diagram of the Central Processing Unit of the ISAP-1 computer is:

![ Figure 2 ](/Pictures/Figure2.png)


## The original format of the SAP-1 computer instructions is:

| 4 bits instruction code   | 4 bits operand (memory address)          |
|---------------------------|------------------------------------------|

We notice that the upper nibble is used to encode an instruction.\ 
So, any instruction is encoded on 4 bits.\ 
Thus, we can have a maximum of 2 ^ 4 = 16 instructions.

How can we increase this number?

We will reserve an instruction code to indicate a special instruction, a prefix that tells the instruction decoder that we have a special instruction. I named this as a prefix for extended instructions.

I chose the prefix as 1111 in binary or 0xF in hexadecimal.

This instruction takes the extended instruction type as a parameter. \
So, we will have 2 ^ 4 = 16 extended instructions. \
These statements have no parameters.

The instruction format for the ISAP-1 computer is the same. But I'm going to add the extended instructions, which have the following form:

| extended instruction prefix 4 bits (0xF) | extended instruction code 4 bits |
|------------------------------------------|----------------------------------|

The instruction set of the ISAP-1 computer has 31 instructions:
- 15 instructions with parameter
- 16 instructions without parameter

If we drop one more instruction with a parameter, we can add another 16 instructions without a parameter, and we will have a total of 46 instructions:
- 14 instructions with parameter
- 32 instructions without parameter

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

## NOP instruction – No OPeration
Binary form:  **** **** \
Operation:  no operation \
Example: NOP

The NOP instruction has only the Fetch portion present in all instructions, but has nothing in the execution portion of the instruction. \
No action is performed. This instruction can be used in programs to delay the execution of an action while waiting for a response from slow peripherals.

This instruction has no parameter so it will be implemented as an extended instruction. \
The binary form for the extended NOP instruction will be: 1111 0000

The timing diagram for the NOP instruction is:

![ Figure 3 ](/Pictures/Figure3.png)

ALL UNIMPLEMENTED INSTRUCTIONS WILL BE TREATED BY THE ISP-1 CPU AS A NOP INSTRUCTION

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 1 ](/Tables/Table1.png)

Signals represented in Red: are active when data is written to the Data BUS \
Signals represented in Green: are active when reading data from the Data BUS \
Signals shown in Black: their activation has no influence on the Data BUS

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the NOP instruction is executed are:
-	EP = NOP * T1
-	LAR = NOP * T1
-	PM = NOP * T2
-	LI = NOP * T2
-	CP = NOP * T2
-	NEXT = NOP * T3 + NOP * T4 + NOP * T5 + NOP * T6 + NOP * T7 + NOP * T8

Since steps T1 and T2 are present and identical in any instruction we can say that they are independent of the executed instruction so we can rewrite the instructions as follows:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	NEXT = NOP * T3 + NOP * T4 + NOP * T5 + NOP * T6 + NOP * T7 + NOP * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the NOP instruction will use 2/3=0.67 which is 67% of the time compared to 2/8=0.25 and 25% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = NOP * T3

In this variant, if the microprogram reaches one of the steps T4 - T8, it will not automatically jump to the next instruction and the time related to steps T4 - T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## LDA instruction – LoaD the Accumulator
Binary form:    0000 nnnn \
Operation:      A ← [n] \
Example: LDA 9h 

Loads the numeric value from Address n into the Accumulator.

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
-	LAR = T1 + LDA * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3
-	DM = LDA * T4
-	LAH = LDA * T4
-	LAL = LDA * T4
-	NEXT = LDA * T5 + LDA * T6 + LDA * T7 + LDA * T8

The last two equations are equivalent to: LA = LDA * T4

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the NOP instruction will use 4/5=0.8 which is 80% of the time compared to 4/8=0.5 and 50% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = LDA * T5

In this variant, if the microprogram reaches one of the steps T6 - T8, it will not automatically jump to the next instruction and the time related to steps T6 - T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## ADD instruction – ADD to accumulator
Binary form:  0001 nnnn \
Operation:    A ← A + [n] \
Example: ADD 8h 

Adds the numeric value at address n with the numeric value stored in the Accumulator and stores the result in the Accumulator.

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
-	LAR = T1 + ADD * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = ADD * T3
-	DM = ADD * T4
-	LB = ADD * T4
-	EU = ADD * T5
-	LAH = ADD * T5
-	LAL = ADD * T5
-	NEXT = ADD * T6 + ADD * T7 + ADD * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the ADD instruction will use 5/6=0.84 which is 84% of the time compared to 5/8=0.625 and 62.5% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = ADD * T6 

In this variant, if the microprogram reaches one of steps T7 or T8, it will not automatically jump to the next instruction and the time related to steps T7 and T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## SUB Instruction – Subtract from accumulator
Binary form:  0010 nnnn \
Operation:  A ← A – [n] \
Example: SUB 5h

Subtracts the numeric value at address n from the numeric value stored in the Acumulator and stores the result in the Accumulator.

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
-	LAR = T1 + SUB * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = SUB * T3
-	DM = SUB * T4
-	LB = SUB * T4
-	EU = SUB * T5
-	LAH = SUB * T5
-	LAL = SUB * T5
-	SU = SUB * T5
-	NEXT = SUB * T6 + SUB * T7 + SUB * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the SUB instruction will use 5/6=0.84 which is 84% of the time compared to 5/8=0.625 and 62.5% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = SUB * T6

In this variant, if the microprogram reaches one of steps T7 or T8, it will not automatically jump to the next instruction and the time related to steps T7 and T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## OUT instruction – Output data from the accumulator
Binary form:  1110 pppp \
Operation:  PORT (p) ← A \
Example: OUT 1h

Transfers the numeric value stored in the Accumulator to Output Port p.

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
-	LAR = T1 + OUT * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = OUT * T3
-	EA = OUT * T4
-	I/O = OUT * T4
-	R/W = OUT * T4
-	NEXT = OUT * T5 + OUT * T6 + OUT * T7 + OUT * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the OUT instruction will use 4/5=0.8 which is 80% of the time compared to 4/8=0.5 and 50% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = OUT * T5

In this variant, if the microprogram reaches one of steps T6 - T8, it will not automatically jump to the next instruction and the time related to steps T6 - T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## The HLT instruction – Halt computer
Binary form:  1111 ****\
Operation:  Halt computer \
Example: HLT

Stops further execution of computer instructions. It is an instruction with prefix 1111 and has no parameters. \

This instruction has no parameter, so it will be implemented as an extended instruction. \
The binary form for the extended HLT instruction will be: 1111 1111

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
-	PM = T2
-	LI = T2
-	CP = T2
-	HLT = HLT * T3 + HLT * T4 + HLT * T5 + HLT * T6 + HLT * T7 + HLT * T8

The control signal HLT is active starting from step T3 until the last step T8. In the original design of the SAP-1 computer, a timing diagram is not shown and this instruction is briefly described.
But from the circuit diagram you can see that gate C34 in the instruction decoder is active when we have the binary code 1111 corresponding to the HLT instruction. So the control signal HLT is not influenced by the state of the T steps.
This fact can be presented graphically as in figure 8 where we see that for any step T3 – T8 the control signal is high.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## LIL instruction – Load Immediate value into Lower nibble of accumulator
Binary form: 0011 nnnn \
Operation: A[3-0] ← Imm \
Example: LIL 2h

This instruction is added by me and is useful for loading an immediate numeric value into the Accumulator in its lower half, leaving the upper half unchanged.

The timing diagram for the LIL instruction is as follows:

![ Figure 9 ](/Pictures/Figure9.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 7 ](/Tables/Table7.png)

Signals represented in Red: are active when data is written to the Data BUS \
Signals represented in Green: are active when reading data from the Data BUS \
Signals shown in Black: their activation has no influence on the Data BUS

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the LIL instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LIL * T3
-	LAL = LIL * T3
-	NEXT = LIL * T4 + LIL * T5 + LIL * T6 + LIL * T7 + LIL * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the LIL instruction will use 3/4=0.75 which is 75% of the time compared to 3/8=0.375 and 37.5% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = LIL * T4

In this variant, if the microprogram reaches one of steps T5 - T8, it will not automatically jump to the next instruction and the time related to steps T5 - T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## LIH instruction – Load Immediate value into Higher nibble of accumulator
Binary form: 0100 nnnn \
Operation: A[7-4] ← Imm \
Example: LIH 5h

This instruction is added by me and is useful for loading an immediate numeric value into the Accumulator in its upper half, leaving the lower half unchanged.

The timing diagram for the LIH instruction is as follows:

![ Figure 10 ](/Pictures/Figure10.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 8 ](/Tables/Table8.png)

Signals represented in Red: are active when data is written to the Data BUS \
Signals represented in Green: are active when reading data from the Data BUS \
Signals shown in Black: their activation has no influence on the Data BUS

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the LIH instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LIH * T3
-	LAH = LIH * T3
-	NEXT = LIH * T4 + LIH * T5 + LIH * T6 + LIH * T7 + LIH * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the LIH instruction will use 3/4=0.75 which is 75% of the time compared to 3/8=0.375 and 37.5% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = LIH * T4

In this variant, if the microprogram reaches one of steps T5 - T8, it will not automatically jump to the next instruction and the time related to steps T5 - T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## IN Instruction – INput data from an input port into the accumulator
Binary form:  1101 pppp \
Operation:  A ← PORT (p) \
Example: IN 1h

This instruction is added by me and transfers the numeric value from Input Port p to Accumulator.

The timing diagram for the IN instruction is as follows:

![ Figure 11 ](/Pictures/Figure11.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 9 ](/Tables/Table9.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the IN instruction is executed are:
-	EP = T1
-	LAR = T1 + IN * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = IN * T3
-	I/O = IN * T4
-	LAH = IN * T4
-	LAL = IN * T4
-	NEXT = IN * T5 + IN * T6 + IN * T7 + IN * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the IN instruction will use 4/5=0.8 which is 80% of the time compared to 4/8=0.5 and 50% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = IN * T5

In this variant, if the microprogram reaches one of steps T6 - T8, it will not automatically jump to the next instruction and the time related to steps T6 - T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## JMP instruction – unconditional JuMP to address n
Binary form:  1100 nnnn \
Operation:  PC ← Imm \
Example: JMP 7h

This instruction is added by me and performs an unconditional jump to Address n.

The timing diagram for the JMP instruction is as follows:

![ Figure 12 ](/Pictures/Figure12.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 10 ](/Tables/Table10.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the JMP instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = JMP * T3
-	LP = JMP * T3
-	NEXT = JMP * T4 + JMP * T5 + JMP * T6 + JMP * T7 + JMP * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the JMP instruction will use 3/4=0.75 which is 75% of the time compared to 3/8=0.375 and 37.5% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = JMP * T4

In this variant, if the microprogram reaches one of steps T5 - T8, it will not automatically jump to the next instruction and the time related to steps T5 - T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## STA Instruction – STore Accumulator
Binary form:  0101 nnnn \
Operation:  [n] ← A \
Example: STA 9h

This instruction is added by me and has the effect of storing the numeric value present in the Accumulator at address n in Data Memory.

The timing diagram for the STA instruction is as follows:

![ Figure 13 ](/Pictures/Figure13.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 11 ](/Tables/Table11.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the STA instruction is executed are:
-	EP = T1
-	LAR = T1 + STA * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = STA * T3
-	EA = STA * T4
-	DM = STA * T4
-	R/W = STA * T4
-	NEXT = STA * T5 + STA * T6 + STA * T7 + STA * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the STA instruction will use 4/5=0.8 which is 80% of the time compared to 4/8=0.5 and 50% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = JMP * T5

In this variant, if the microprogram reaches one of steps T6 - T8, it will not automatically jump to the next instruction and the time related to steps T6 - T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## CMP Instruction – CoMPare with accumulator
Binary form:  0110 nnnn \
Operation:  A ? [n] \
Example: CMP 2h

Subtracts the numeric value at address n from the numeric value stored in the Accumulator and does NOT store the result in the Accumulator.

The timing diagram for the CMP instruction is as follows:

![ Figure 14 ](/Pictures/Figure14.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 12 ](/Tables/Table12.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the CMP instruction is executed are:
-	EP = T1
-	LAR = T1 + CMP * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = CMP * T3
-	DM = CMP * T4
-	LB = CMP * T4
-	SU = CMP * T5
-	NEXT = CMP * T6 + CMP * T7 + CMP * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the CMP instruction will use 5/6=0.84 which is 84% of the time compared to 5/8=0.625 and 62.5% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = JMP * T6

In this variant, if the microprogram reaches one of steps T7 and T8, it will not automatically jump to the next instruction and the time related to steps T6 and T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## CPY instruction – CoPY contents of accumulator register to B register
Binary form:  1111 1110 \
Operation:  B <- A \
Example: CPY

Copies the contents of the Accumulator register to the B register.

The timing diagram for the CPY instruction is as follows:

![ Figure 15 ](/Pictures/Figure15.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 13 ](/Tables/Table13.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the CPY instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EA = CPY * T3
-	LB = CPY * T3
-	NEXT = CPY * T4 + CPY * T5 + CPY * T6 + CPY * T7 + CPY * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the CPY instruction will use 3/4=0.75 which is 75% of the time compared to 3/8=0.375 and 37.5% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = CPY * T4

In this variant, if the microprogram reaches one of steps T5 - T8, it will not automatically jump to the next instruction and the time related to steps T5 - T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## JZ instruction – Jump if Zero flag is set
Binary form:  1001 nnnn \
Operation:  Z = 1 ? PC <- nnnn : PC <- PC+1 \
Example: JZ 7h

The program jumps to Address n if the Zero flag is set, otherwise it continues with the next instruction

The timing diagram for the JZ instruction is as follows:

![ Figure 16 ](/Pictures/Figure16.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 14 ](/Tables/Table14.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the JZ instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = JZ * T3
-	LP = (JZ * Z) * T3
-	NEXT = JZ * T4 + JZ * T5 + JZ * T6 + JZ * T7 + JZ * T8

Using the NEXT signal moves to the next instruction without losing micro-steps. This variable microcode length system for the JZ instruction will use 3/4=0.75 which is 75% of the time compared to 3/8=0.375 and 37.5% if we do not use this option.

Use of the NEXT signal is optional. The simplified version of this instruction can also be used:
- NEXT = JZ * T4

In this variant, if the microprogram reaches one of steps T5 - T8, it will not automatically jump to the next instruction and the time related to steps T5 - T8 is lost.

*If we implement the Control Block using Combinational Logic we will use these equations.*

## JC instruction – Jump if Carry
Binary form:  1010 nnnn \
Operation:  C = 1 ? PC <- nnnn : PC <- PC+1 \
Example: JC 8h

The program jumps to Address n if the Carry flag is set, otherwise it continues with the next instruction

The timing diagram for the JC instruction is as follows:

![ Figure 17 ](/Pictures/Figure17.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 15 ](/Pictures/Table15.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the JC instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = JC * T3
-	LP = JC * C * T3
-	NEXT = JC * T4

*If we implement the Control Block using Combinational Logic we will use these equations.*

## JS instruction – Jump if Sign
Binary form:  1011 nnnn \
Operation:  S = 1 ? PC <- nnnn : PC <- PC+1 \
Example: JS 9h

The program jumps to Address n if the Sign flag is set, otherwise it continues with the next instruction

The timing diagram for the JC instruction is as follows:

![ Figure 18 ](/Pictures/Figure18.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 16 ](/Pictures/Table16.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the JS instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = JS * T3
-	LP = JS * S * T3
-	NEXT = JS * T4

*If we implement the Control Block using Combinational Logic we will use these equations.*

## The INC instruction – increment the contents of the Accumulator register
Binary form:  1111 0001 \
Operation:  A <- A + 1 \
Example: INC

It's an instruction added by me that does Increment the contents of the Accumulator Register by one. It is an instruction that has no parameters, so it is an extended instruction.

The timing diagram for the INC instruction is as follows:

![ Figure 19 ](/Pictures/Figure19.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 17 ](/Pictures/Table17.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the INC instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EC1 = INC * T3
-	LB = INC * T3
-	EU = INC * T4
-	LAH = INC * T4
-	LAL = INC * T4
-	NEXT = INC * T5 + INC * T6 + INC * T7 + INC * T8

*If we implement the Control Block using Combinational Logic we will use these equations.*

## DEC instruction – Decrement the contents of the Accumulator register
Binary form:  1111 0010 \
Operation:  A <- A - 1 \
Example: DEC

It is an instruction added by me that decrements the contents of the Accumulator Register by one. It is an instruction that has no parameters, so it is an extended instruction.

The timing diagram for the DEC instruction is as follows:

![ Figure 20 ](/Pictures/Figure20.png)
