# 4-Bit-Microprocessor
A 4-Bit Microprocessor with two general purpose registers and nine instructions constructed on simulator.io
https://simulator.io/board/ftYGkLoovG

Assignment 01

ROM
Rom uses toggle diode to point to the address of the instruction. Adding a toggle diode represents a 1 and no toggle diode represents a 0 which is used to represent binary values to represent instructions. The instructions are as follows:
1)	0x00	IN
2)	0x01	OUT
3)	0x03	SWP
4)	0x04	ADD
5)	0X05	SUB
6)	0X06	AND
7)	0X07	OR
8)	0X08	XOR
9)	0X09	NOT

Output
		When value 0001 (location of the output instruction in RAM) comes from the instruction register, the value of A is sent to the output display.

I/O Controller
	Controls whether value of A should be sent to the output display or not depending on whether the output instruction has been executed

Register A and B
	Register A and B store values of A and B respectively, 4 D flip flips are used for each, each flip flop stores a binary value (byte) 

Program Counter
	Program counter serially fetches instructions from the RAM by keeping track of which address to take instruction from. It stores the value of the address of the next instruction to be executed by the RAM.
	The instruction pointer made up to flip flops stores the value of the address of the next instruction in binary form. The increment instruction pointer increments to the binary value of the current address stored to move on to the next address of the next instruction.
