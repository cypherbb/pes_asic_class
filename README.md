# PES_ASIC_CLASS
**Course Name** : VLSI Physical Design for ASICs  
**Instructor** : Kunal Ghosh   
**About Course** : This course covers key aspects of RISC-V and chip design, spanning design cycles, RISC-V ISA, analog IPs, and mixed-signal flow. It delves into process design kits, libraries, RTL design synthesis, and gate-level simulation. The curriculum also includes RTL-to-GDS flow, encompassing SoC design implementation from floor planning to post-layout timing analysis.<br>

## Skill Outcomes
1. Architectural Design
2. RTL Design/Behavioral Modeling
3. Floorplanning
4. Placement
5. CTS
6. Routing 

## Installation
https://github.com/kunalg123/riscv_workshop_collaterals/blob/master/run.sh
1. Open Terminal<br>
2. Run the commands in run.sh file one after the another.

## Contents
### Day 1
* Introduction
* Program to compute the sum of natural numbers till n.
* Assembly code of sumNn.c
* Spike Simulations and Debug.
* 64 Bit Signed ans Unsigned Binary Integer Numbers.
* Program to find the maximum and minimum values of Signed and Unsigned numbers.
  
### Day 2
* Application Binary Interface(ABI)
* ABI function calls
* Running C program on RISC-V CPU

### Day 3
*

## Course
<details>
<summary> Day 1 </summary><br>

## Contents of Day 1
+ Introduction
+ Program to compute the Sum of Natural numbers till n.
+ Assembly code of sumNn.c
+ Spike Simulations and Debug.
+ 64 Bit Signed ans Unsigned Binary Integer Numbers.
+ Program to find the maximum and minimum values of Signed and Unsigned numbers.

## Introduction
Software --> **ISA** --> Hardware     
**ISA** is Instruction Set Architecture: It the language the processor understands.   

Application(C code) -> Compiler(ISA) -> Assembler(Machine code) -> Processor  

1. Application performing some operation is written in C language.
2. Compiler converts the C language into ISA.
3. Assembler Converts these instructions into machine code(binary).
4. Processor only undersands o's and 1's.  

## Program to compute the Sum of Natural numbers till n.
  #### Code -> sum1ton.c
  ```
  #include <stdio.h>

  int main()
  {
    int i, sum = 0, n = 5;
    for(i = 1; i <= n; ++i){
    sum =sum+i;
    }
    printf("Sum of numbers from 1 to %d = %d\n", n, sum);
    return 0;
  }
  ```
#### 1. Executing using GCC Complier<br>
   ```
   gcc sum1ton.c
  
   ```
  
   To display output
   ```
   ./a.out
   ```
   ![Screenshot from 2023-08-21 19-35-30](https://github.com/cypherbb/pes_asic_class/assets/107348780/b15fa1ca-c420-451f-88db-2c379fb19aa4)

   <br>

#### 2. Executing using RISC-V Complier<br>
   Before executing we must export path.<br>
   ```
   export PATH="/home/home/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH"   
   ```
   path of file == riscv_toolchain -> risc64-unknown-eld-gcc -> bin<br>
   and then add 'bin:$PATH' at the end.
   ```
   riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
   ```
   * **-O1**: This is a compiler optimization flag. The -O1 flag indicates level 1 optimization.
   * **-mabi=lp64**: This flag specifies the ABI (Application Binary Interface) used for the compiled code.(The lp64 ABI indicates that long and pointer types are 64 bits.)
   * **-march=rv64i**: This flag specifies the target RISC-V architecture and its instruction set.
   To display output
   ```
   spike pk sum1ton.o
   ```
 ![Screenshot from 2023-08-21 20-09-23](https://github.com/cypherbb/pes_asic_class/assets/107348780/44e729f9-744e-4cdc-8e54-56518624cc18)


   
## Assembly code of sum1ton.c

To view the assembly code of sum1ton.c 
```
riscv64-unknown-elf-objdump -d sum1ton.o | less
```
Since we have written our code in 'int main()'<br>
Search main by typing
```
/main
```
Press 'n' so that the address where the main file is present.
![Screenshot from 2023-08-21 20-20-13](https://github.com/cypherbb/pes_asic_class/assets/107348780/9308abe2-34db-4247-ba15-96b3648e7113)

To find the number of instructions in main file.
To find the number of instructions<br>
* **((Last address + 4) - initial address) / 4**
* Using **-O1** to compile.
  


  ![less_no_instru_dec](https://github.com/vamsi-2312/pes_asic_class/assets/142248038/ff0f901b-d268-48b1-9bab-ea84dd065a03)
  <br>
  Got **11** instructions.
* Usig **-Ofast** to compile.
  ![Screenshot from 2023-08-21 20-24-18](https://github.com/cypherbb/pes_asic_class/assets/107348780/d6254103-9bff-4812-ae91-a6b4e522b385)

  Got **11** instructions.

## Spike Simulations and Debug
Compile the code in riscv compiler.<br>
To debug 
```
spike -d pk sum1ton.o
```
Run the code till the address 100b0.
```
until pc 0 100b0
```
To view the content of register.
```
reg 0 a0
```
Press ENTER to run the next command.<br>
Then type 'reg 0 a0' see the content of a0, it will be updated.<br>
```
reg 0 sp
```
Next press ENTER to run the next command.<br>
```
reg 0 sp
```
'sp' register will be updated with addi command.<br>
The object dump file
![Screenshot from 2023-08-21 20-27-50](https://github.com/cypherbb/pes_asic_class/assets/107348780/0cb828b8-fdcc-4b9b-9500-32862a27a3ae)

<br>
**lui** - load upper immediate
**lui a0, %hi(.LC1)**<br>
  lui destn_reg, offset(immediate_value) 
<br>
**addi** - add immediate<br>
**addi destn_reg, src_reg_1, imm_value**
<br>
<br>
**Byte Addressing** - It is the method in which each address location in the memory is having unique address of 1 Byte or 8 bit.
In RISC-V architecture we use Byte addressing because it is much effecient use of memory.
<br>
And **each and every instruction** in RISC-v is of **32 Bit** in length.
<br>

## 64 Bit Signed ans Unsigned Binary Integer Numbers
- Humans understand **Decimal** numbers.
- Computers understand **Binary** numbers.
<br>
  MSB - Most Significant Bit<br>
  LSB - Least Significant Bit<br>
<br>
  * 1 Bit<br>
  * 1 Byte = 8 Bits<br>
  * 1 Halfword = 16 Bits<br>
  * 1 Word = 32 Bits = 4 Bytes<br>
  * 1 Doubleword = 64 Bits = 8 Bytes<br>

#### Range of Unsigned Integers<br>
For  n Bit --> 0 to (2^n - 1)<br>
For  2 Bit --> 0 to (2^2 - 1) = 0 to 3<br>
For  4 Bit --> 0 to (2^4 - 1) = 0 to 15<br>
For 64 Bit --> 0 to (2^64 - 1) = 0 to 18446744073709551999<br>

#### Range of Signed Integer<br>
For  n Bit --> (-2^(n-1))  to (2^(n-1) - 1)<br>
For  2 Bit --> (-2^1)  to (2^1 - 1)  = -2 to 1 <br>
For  4 Bit --> (-2^4)  to (2^4 - 1)  = -8 to 7 <br>
For 64 Bit --> (-2^64) to (2^64 - 1) = -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807<br>

#### 2's Compliment(Representation of Negative Numbers in Binary)
1. Write the magitude in binary format.
2. Invert the numbers(0 -> 1)(1 -> 0).
3. Add 1
4. We have got the 2'c compliment form of the negative number.
<br>

( 2)dec = (0010)bin<br>
(-2)dec = (1110)bin<br>
<br>

#### MSB for Signed Number
+ Postive Number has MSB as **0** for signed number.
+ Negative Number has MSB as **1** for Signed number.
<br>

## Program to find the maximum and minimum values of Signed and Unsigned numbers

<br>

#### 1. For Unsigned Numbers.<br>

#### Code
```
#include <stdio.h>
#include <math.h>

int main()
{
	unsigned long long int max = (unsigned long long int) (pow(2,64)-1);
	unsigned long long int bey_max = (unsigned long long int) (pow(2,99)-1);
	unsigned long long int min = (unsigned long long int) (pow(2,64)*-1);
	unsigned long long int mid = (unsigned long long int) (pow(2,10)-1);
	printf("The highest value of unsigned long long int = %llu\n",max);
	printf("The value of bey_max(if value more than max) = %llu\n",bey_max);
	printf("The lowest value of unsigned long long int = %llu\n",min);
	printf("The value of mid = %llu\n",mid);
	return 0;
}
```

![image](https://github.com/cypherbb/pes_asic_class/assets/107348780/65b37a2b-2a39-4b7e-bf13-5e7c1c68b75b)


<br>

#### 2. For Signed Numbers.<br>

```
#include <stdio.h>
#include <math.h>

int main()
{
	long long int max = (long long int) (pow(2,63)-1);
	long long int min = (long long int) (pow(2,63)*-1);
	long long int bey_max = (long long int) (pow(2,99)-1);
	long long int bey_min = (long long int) (pow(2,99)*-1);
	printf("The maximum value of signed long long int = %lld\n",max);
	printf("The minimum value of signed long long int = %lld\n",min);
	printf("The value if beyond maximum = %lld\n",bey_max);
	printf("The value if less then minimum = %lld\n",bey_min);
	return 0;
}
```

![Screenshot from 2023-08-21 19-58-15](https://github.com/cypherbb/pes_asic_class/assets/107348780/3b01ed5e-1d63-4cb5-ab20-e9c02fbbd018)



<br>
End of Day 1.
<br>
</details>

<details>
<summary> Day 2 </summary><br>

## Contents of Day 2
+ Application Binary Interface(ABI)
+ ABI function calls
+ Running C program on RISC-V CPU

## Application Binary Interface
An Application Binary Interface (ABI) defines how binary code interacts at a low level, specifying data structures, calling conventions, and system-level details to ensure compatibility between compiled software components on a given platform.
<br>

**Application software**  -(API)->  **Standard Libraries**  ->  **OS**  -(ISA)->  **Processor Architecture**  -(RTL)->  **Hardware**
<br>
API - Application Programming Interface<br>
ISA - Intruction Set Architecture<br>
RTL - Register transfer level<br>

Some part of the ISA availabe to OS and to the User
- User and System ISA
- User ISA
<br>

Application program can access some of the system or hardware directly by **System Calls**(like accessing register).<br>
We call this ABI aka Sytem call interface.<br>

ABI <-register-> registers

* RV32 has 32 32bit size registers.
* RV64 has 32 64bit size registers.
<br>

There are 2 different ways to load the data
1. Load and Store method.(RISC-V uses this method)
2. Direct memory accessing.
<br>

RISC-V uses **Little endian** Memory Addressing, meaning the LSB is loaded first then till MSB.
<br>

**Intructions size is always ***32 bit*** no matter is RV32 or RV64.**
<br>

#### Basic Integer Instruction
1. Load (I Type instruction)
2. Add (R Type instruction)
3. Store (S Type instruction)

##### Load Instruction
syntax - ld rd, imm(rs1);<br>
ld = load doubleword<br>
rd = destination register<br>
rs1 = source register 1<br>
imm = immediate value or offest<br>
[immediate value][rs1][func3][rsd][opcode]<br>

#### Add Instruction
syntax - add rd, rs1, rs2;
rd = destination register<br>
rs1 = source register 1<br>
rs2 = source register 2<br>
[func7][rs2][rs1][func3][rd][opcode]<br>

#### Store Instruction
syntax - sd rs2,imm(rs1);
sd = store doubleword<br>
rs1 = source register 1<br>
rs2 = source register 2<br>
imm = immediaate value or offset<br>
[imm(11:5)][rs2][rs1][func3][imm(4:0)][opcode]<br>

**Instruction Format**
+ R Type : All the operands are of register type.
+ I Type : One immediate value of 12 bit is use along with registers.
+ S Type : Stores value has immediate value and register.

#### Why are there only 32 registers in number?<br>
Because, all register has 5 bits of address, and **Total number of registers = 2^5 = 32 registers**.<br> 
These 32 registers are named as x0 till x31.<br>
and given some function.<br>
![1](https://github.com/cypherbb/pes_asic_class/assets/107348780/10a04405-3d17-4727-8a8a-f309886b31b9)

<br>

## ABI function calls
Basic idea how we are going to call the function and execute the code.<br>
![image](https://github.com/cypherbb/pes_asic_class/assets/107348780/a95e73d6-23d0-4b4e-ac0a-2096c5e4063d)

<br>

#### Code(Contain's the function call(load))
```
#include <stdio.h>

extern int load(int x, int y);

int main()
{
	int result = 0;
	int count = 9;
	result = load(0x0, count+1);
	printf("Sum of numbers from 1 to %d = %d\n", count, result);
	
}
```
#### Assembly code of the above flow of instructions
```
.section .text
.global load
.type load, @function

load:
	add	a4, a0, zero 
	add	a2, a0, a1   
	add	a3, a0, zero 
loop:
  add	a4, a3, a4  
	addi	a3, a3, 1    
	blt	a3, a2, loop 
	add	a0, a4, zero 
	ret 
```

#### Executing the code<br>
![Screenshot from 2023-08-21 20-00-33](https://github.com/cypherbb/pes_asic_class/assets/107348780/bbbfea85-31f9-46f4-959d-e1febb23b307)

obj dump file<br>
![Screenshot from 2023-08-21 20-02-42](https://github.com/cypherbb/pes_asic_class/assets/107348780/7e485f8c-cd21-4b8c-a2e7-958377fb38ce)

We can see some of the ABI are updated according to our load.S code.
<br>

## Running C program on RISC-V CPU
Before we can run the C program, we need to have the RISC-V CPU, testbench and other files.<br>
open terminal.<br>
```
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
```
```
cd riscv_workshop_collaterals/labs
```
We will abe able to see the files required installed.<br>
To run the code.<br>
```
chmod 777 rv32im.sh
```
```
./rv32im.sh
```

<br>

Flow of ecexuting the code on RISC-V CPU<br>
![image](https://github.com/cypherbb/pes_asic_class/assets/107348780/73c08bed-2297-43ad-bb5b-8cc684eadf50)



<br>
End of Day 2.
<br>
</details>

<details>
<summary> Day 3 </summary><br>

</details>
