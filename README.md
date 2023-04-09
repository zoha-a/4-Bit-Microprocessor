# 4-Bit-Microprocessor
A 4-Bit Microprocessor with two general purpose registers and nine instructions constructed on simulator.io
https://simulator.io/board/ftYGkLoovG

Assignment 01

ROM:
Rom uses toggle diode to point to the address of the instruction. Adding a toggle diode represents a 1 and no toggle diode represents a 0 which is used to represent binary values to represent instructions.

Instruction Set Architecture (ISA)
1)	0x00	OUT (from Register A)
2)	0x01	LDI k (to Register A)
3)	0x02	SWAP (Register A and B)
4)	0x03	ADD (Register A and B and save into A)
5)	0X04	SUB (Register A and B and save into A)
6)	0X05	AND (Register A and B and save into A)
7)	0X06	OR (Register A and B and save into A)
8)	0X07	XOR (Register A and B and save into A)
9)	0X08	NOT (Register A and B and save into A)

Register A and B:
Register A and B store values of A and B respectively, 4 D flip flips are used for each, each flip flop stores a binary value (byte).

Output (LED):
When value 0000 comes from the instruction register, the value stored in Register A is sent to the output display.

I/O Controller:
Controls whether value of A should be sent to the output display or not depending on whether the output instruction has been executed.

Control Unit:
There is an instruction pointer that needs to point to the next instruction. Therefore, it takes the current instruction from the Program ROM it to the Half Adders. Here, we add one to the instruction to increment the instruction. The sum of each half adder is sent to a series of D flip flops, while the Carry is sent to the next half adder with the previous instruction. After the entire addition is complete, the instruction is successfully incremented and therefore, is fed back to the flip flops.

![image](https://user-images.githubusercontent.com/76551920/230781898-b3837597-8d8c-4bd2-b43d-29d8872794e0.png)

The clock to these flip flops is connected to the control unit cycle. To understand the control unit, we start off with the JK flip flops that we have. Since these are set to toggle, and the output of the first is sent into the second Flip Flop, we ensure that whenever these two outputs sync up and are low, a clock pulse is sent to the instruction register, meaning that the instruction is fetched currently. We can think of this like a binary table, represented in the following manner:

Instruction is fetched:	Both the inputs are low - 00
Parameter is fetched:	One input is low, other is high – 01
Instruction is executed:	The other input is low, first is high - 10
Write back:	Both inputs are high - 11

The output of this fetch AND gate is to be remembered as it’ll be used later. The next portion is how we’ll fetch the parameters. We have three requirements to activate the clock on the parameter register. The first being that the instruction should be the LDI instruction, because that is the only instruction in our instruction set that utilizes the parameter register. Therefore, we can see this in the following AND gate’s setup:

![image](https://user-images.githubusercontent.com/76551920/230782099-64e0422f-fcaa-496b-b826-f18b5b11efa5.png)

Next, we need to check if the two JK flip flops give us the outputs of 01 since this is the case when the parameter will be fetched. The final condition is going to be if the clock pulse is currently being given. If all these conditions are fulfilled, then the clock is given to the Parameter Register. The parameter register holds the values of any parameters given to it, however in our microprocessor, we’ll only be utilizing it for when we need to move a value using the LDI instruction. The parameter register will hold this value that we need to load. Therefore, it is only connected to the LDI instruction.

Now this AND is also significant, as either when this is high or when the previously mentioned AND is high, the clock to the instruction pointer is sent. We need to remember that this happens because the instruction pointer needs to point to the next instruction after either the current instruction is fetched, or the parameter is fetched.

We next move onto the execution of the instruction, being done when the output is of 10. Whenever this output is synced up with the actual clock, a pulse is sent that we’ll be calling the Execution Clock. This will make it so that the instruction is executed whenever it is fed into the instruction register. We’ll see more about this when we talk about the execution of instructions.

This entire system is looped back as the clock of the original JK flip flop is connected to an OR gate. This OR gate ensures that whenever nothing is being executed or if a pulse indicating that the execution is done is received, a clock is sent to the flip flops of the instruction pointer and we move onto the next instruction.

Instruction Set Architecture Briefing
1. OUT
![image](https://user-images.githubusercontent.com/76551920/230782196-7689df77-3ee3-4475-96dd-a19ea0752c10.png)
 
OUT allows us to display whatever is currently loaded in register A. The bubbles correspond to the op-code that we’ve set for the OUT instruction.  When that is received, the second AND gate waits for the execution clock, sending a signal to the I/O controller’s clock and displaying the data. The data is sent into the I/Os using the Data Output of register A. The I/O controllers are 4 D flip flops where we’ll be storing data before we send it forward for display.

2. LDI:
We can load any value we want into Register A. The bubbles correspond to the op-code that we’ve set for the LDI instruction. We then feed the output of the first AND gate into the lower four ones, which will only be high when the LDI instruction is called. The second input comes from the parameter register, where the value that we want to load is presented. We then AND these two values, and the value of the parameter register is loaded into register A. The output of the first AND gate also serves as a clock to register A. We once again, send a signal back to the Control Unit as an indication that execution is done. This will be done in all operations to send the clock to the JK flip flop that cycles through.

3. SWAP:
The SWP function is a simple function that swaps the contents of register A and B. The AND gates in there basically take one input from the first AND gates’ output which is activated whenever the SWP function is called. The second input for the upper 4 gates are the contents of register B, which are fed into the register A. The lower 4 do the same things, with the roles of A and B reversed. We then take the first gate’s output and feed it into the clock of the register A and B and by doing this, the clock is fed every time the SWP function is called. 

Next we have the ALU instructions including the ADD, SUB, AND, OR, XOR and NOT. These will be covered in the ALU part.

ALU
An Arithmetic Logic Unit (ALU) is a digital circuit that performs arithmetic and logic operations in a  microprocessor. It consists of a data path that performs arithmetic operations such as addition, subtraction.

4. ADDER and 5.SUBTRACTOR:
Full adders are implemented using a combination of logic gates such as XOR, AND, and OR gates. The XOR gate produces the sum bit S, while the AND gate produces the carry-out bit Cout. The OR gate combines the carry-out bit from the AND gate and the carry-in bit to generate the final carry-out bit. The carry-in bit is added to the input bits A and B using another XOR gate.
For adder/subtractor we implemented the same concept of 1 binary adder/subtractor that we studied in DLD but here we used 4 full adders to add or subtract 4 bit binary number .When the input signals to an XOR gate are the same, the output is a logical zero, and when the inputs are different, the output is a logical one. In an adder/subtractor circuit, an XOR gate is used to add the two binary digits A and B. If the carry-in  signal is also added to the inputs, the XOR gate can perform the addition operation of the three input signals (A, B, and C).In the subtraction operation, the two binary digits A and B are subtracted using an XOR gate to produce a difference (D) output, while the borrow-out  signal is generated by an AND gate. If the borrow-in  signal is also used in the subtraction operation, it can be combined with the borrow-out signal to generate the final borrow-out signal.

![image](https://user-images.githubusercontent.com/76551920/230782413-acf00afa-bde6-434f-a068-e829b75b2401.png)

5. AND:
We have implemented AND instruction which is executed by first AND the first bit of register A with first bit of register B and so on, then AND its output with the output of AND instruction which is then sent to ‘write register A’  which is then stored in register A.

![image](https://user-images.githubusercontent.com/76551920/230782431-82a425a6-0f40-416c-b1a8-f6f83494a44b.png)

6. OR:
We applied the same logic as AND. The only difference is that we first OR the first bit of register A with first bit of register B and so on, then AND its output with the output of OR  instruction which is then sent to ‘write register A’  which is then stored in register A.

![image](https://user-images.githubusercontent.com/76551920/230782444-0a2de8f7-5075-44f1-b45b-0eaa1e844f89.png)

7. XOR:
Same logic has been applied as AND and OR . The only difference is that we first XOR the first bit of register A with first bit of register B and so on, then AND its output with the output of XOR instruction which is then sent to ‘write register A’  which is then stored in register A.

![image](https://user-images.githubusercontent.com/76551920/230782467-57e14c18-b344-47a3-9924-fd60438ca3c5.png)

8. NOT:
The NOT instruction has been implemented by first NOT the bits of register A and then AND its output with the output of NOT instruction which is then sent to ‘write register A’  which is then stored in register A.

![image](https://user-images.githubusercontent.com/76551920/230782489-79e4c235-8ce2-4561-9d2a-25f0b692d587.png)

Working of Subcycle Using Sample Code:
We have written a sample code as attached. The values are: 1, 1, 2, 1, 3, 3. This translates to
LDI 1
SWAP
LDI 3
ADD
OUT

![image](https://user-images.githubusercontent.com/76551920/230782836-7014cda5-1084-44f0-8bfa-9d3ac48c1cd8.png)

Design Choices:
We decided to save space on ALU registers as the task of writing minimum five instructions were basic and did not require repeated arithmetic. Hence, we efficiently integrated the ISA and ALU.

Workload
1. Zoha Ahmed: Creation of SWAP, ADD, SUB, helped with Control Unit
2. Ayesha Azhar: Creation of AND, OR, XOR, NOT instructions
3. Ahmad Faisal: Creation of Control Unit and LDI
4. Bilal Haseeb Hashmi: Creation of ROM, Registers and OUT
