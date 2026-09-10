# 16bit-CPU-Architecture
custom von neumann CPU built from scratch out of logic gates in Logisim evolution.

-note only the .circ file for the project is provided and Logisim evolution is still needed to run it, Logisim evolution can be downloaded here https://github.com/logisim-evolution/logisim-evolution/releases

below is an overview of the CPU ISA and other details regarding operation:

6bit operation code config:
00-0000
first 4 bits represent functions (within a unit)
last 2 bits represent unit (each unit has functions within it)

units:
00-control (e.g. jump, halt)
01-data transfer (e.g. load, save)
10-arithmatic and logical
11-null

-19 instructions total
instructions:
00-0000 = nop (no operation)
00-0001 = jmp (jump to a specified instructions location)
00-0010 = jz  (jump to a specified instructions location, if flag register outputs zero flag)
00-0011 = jnz (jump to a specified instructions location, if flag register does not output zero flag)
00-0100 = hlt (halt)
instructions from 00-0101 to 00-1111 are null
01-0000 = lod (load, move data from memory into general purpose registers)
01-0001 = sav (save, move data from general purpose registers into memory)
instructions from 01-0010 to 01-1111 are null
10-0000 = add (addition)
10-0001 = sub (subtraction)
10-0010 = inc (increment, increase a number by 1)
10-0011 = dec (decrement, decrease a number by 1)
10-0100 = and (logical bitwise AND operation between two numbers)
10-0101 = or  (logical bitwise OR operation between two numbers)
10-0110 = xor (logical bitwise XOR operation between two numbers)
10-0111 = not (logical bitwise NOT operation on a number, invert each bit)
10-1000 = lsl (logical shift left, move all bits to the left by 1 (double the number))
10-1001 = lsr (logical shift right, move all bits to the right by 1 (half the number))
instructions from 10-1010 to 10-1011 are null
10-1100 = psa (passthrough number/ALU input A)
10-1101 = psb (passthrough number/ALU input B)
instructions from 10-1110 to 11-1111 are null

8bit memory code config:
00000-00000
first 5 bits are for addressing where to send data (TO)
last 5 bits are for addressing where to retrieve data (FROM)

-when using load the FROM bits are used to address memory location and TO bits are used to address gpr location
-when using save the FROM bits are used to address gpr location and TO bits are used to address memory location
-when using any arithmetic operation FROM bits are used to address the location of first value and TO bits are used to address location of second value aswell as final data location, e.g. take value from register 4 and add it to value from register 2 then store resultant in register 2 (if using operation with only 1 operand e.g. NOT then TO bits only dictate final location and FROM bits are responsible for supplying the value)
-when using control operations FROM bits are used to address the gpr location of the value we want to use (e.g. jump to)
the TO bits are not used (and if using halt or nop then no memory code bits are needed)

-note for using the memory codes for addressing gpr only the first 3 bits need to be used to address all 8 registers
