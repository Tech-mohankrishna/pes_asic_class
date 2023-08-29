# 🟩 PES ASIC CLASS 🟩
# :book: About the repository

Welcome to my GitHub repository dedicated to VLSI Physical Design for ASICs using open-source tools! Here, we embark on a journey that starts with processor specifications and leverages the power of the RISC-V ISA. We'll build processors from scratch, taking them through the entire RTL to GDS process  that meets various Performance, Power, Area ( PPA ) and manufacturability requirements. The best part? We're doing it all with open-source tools, including the RISC-V toolchain and OpenLane anad many more .
<p align="center">
  <img src="https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/00ea3403-674e-4c70-a86e-a4d39aff4ff8" alt="Image" width="800">
</p>



# 🔖 Table of Contents
+ [Day 1:Introduction to RISC-V ISA and GNU Compiler Toolchain](#day-1-introduction-to-risc-v-isa-and-gnu-compiler-toolchain)
+ [Day 2:Introduction to ABI and Basic Verification Flow](#day-2-introduction-to-abi-and-basic-verification-flow)
+ [ Day-3:Introduction to Verilog RTL design and Synthesis](#day-3-Introduction-to-Verilog-RTL-design-and-Synthesis)

## Requirements:
+ **OS**: Ubuntu 20 +
+ **Memory**: 200 GB
+ **RAM**: 6 GB



Errors regarding tools installation are resolved in [Resolve Errors Guide](resolve_errors.md)


<details>
<summary> DAY 1: Introduction to RISC-V ISA and GNU Compiler Toolchain </summary>
<br>
	
# Day 1: Introduction to RISC-V ISA and GNU Compiler Toolchain

## Tools Installations
## Tools Used:
+ **RISC-V GNU Toolchain**: A comprehensive set of tools for compiling and building software to run on RISC-V processors.
+ **RISC-V ISA Simulator**: A RISC-V simulator used for functional verification and testing of RISC-V code without needing actual hardware.
+ **RISC-V Proxy Kernel**: The RISC-V Proxy Kernel, a lightweight execution environment for running user-level applications on RISC-V processors.

Build tool chain and pre-requisite  :  

```
sudo apt update
chmod +x install_tools.sh
./install_tools.sh
```


## Overview from Application to Hardware
<p align="center">
  <img src="https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/dd018703-3b2e-464d-8653-d7deb3c9dd6f" alt="Image" width="800">
</p>





- **Apps**: Application software, often referred to as "apps," performs specific tasks or functions for end-users.

- **System Software**: This category acts as an intermediary between hardware components and user-facing applications. It provides essential services, manages resources, and enables application execution.

- **Operating System**: The fundamental software managing hardware resources and offering services for users and applications. It controls memory, processes, files, and interfaces (e.g., Windows, macOS, Linux, Android).

- **Compiler**: Translates high-level programming code( C ,C++ , java etc... ) into assembly-level language.

- **Assembler**: Converts assembly language code into machine code ( 10101011100 ) for direct processor execution. 

- **RTL (Register Transfer Level)**: Represents digital circuit behavior using registers and data transfer operations.

- **Hardware**: Physical components of a computer system or electronic device enabling various tasks.






## Introduction to RISC-V :
### RISC-V Archiecture 

RISC-V is an **open-source Instruction Set Architecture (ISA)** that has gained significant attention and adoption in computer architecture and semiconductor design. RISC architectures simplify instruction sets by focusing on a smaller set of instructions, each executable in a single clock cycle, leading to faster instruction execution.

### RISC-V Instruction Types

- **R-Type**: Register-type instructions, involving operations between registers. Example: `add`, `and`, `or`.

- **I-Type**: Immediate-type instructions, using immediate values for operations. Example: `addi`, `ori`, `lw`.

- **S-Type**: Store-type instructions, storing data from a register to memory. Example: `sw`, `sb`.

- **B-Type**: Branch-type instructions, conditional branching based on comparisons. Example: `beq`, `bne`, `blt`.

- **U-Type**: Upper immediate-type instructions, used for large immediate values. Example: `lui`, `auipc`.

- **J-Type**: Jump-type instructions, unconditional jumps within the program. Example: `jal`, `jalr`.
  
 In addition to base instruction there are more instruction which help in improving exceution speed like Pseudo Instructions (`li` and `mv`) , Multiply Extension Instructions (`mul`, `mulh`, `mulhu`, and `mulhsu`) , Single and Double Precision Floating Point Extension and so on 

## Labwork for RISC-V software tool chain : 
The main objective of this lab is to compile simple C codes using `gcc compiler`  and run them on native hardware. Similarly, the goal is to compile the same code using `riscv64-unknown-elf-gcc`, execute it on a RISC-V core within a simulator, and understand the process involved. The ultimate goal is to ensure that any high-level program written can be successfully executed on our hardware platform.


A simple c code to find sum from 1 to N : 
```
#include <stdio.h>
int main() {
	int sum=0 , n=5;
	for (int i=0;i<=n;++i)
	{
		sum = sum+i;
	}
	printf("The sum of numbers from 1 to %d is %d\n",n,sum);
	return 0;
}
```
execution command : 
```
gcc sum_1_n.c -o sum_1_n.o
./sum_1_n.o
```
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/c86f5593-6111-4ffd-b601-ee1783cd7f10)




compile the same using riscv compiler and view the output


```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum_obj.o sum_1_n.c
spike pk sum_obj.o
```



Additional info :
- `-O1`: This flag sets the optimization level to low. It balances code size and execution speed while maintaining reasonable compilation times.

- `-mabi=lp64`: This flag defines the ABI (Application Binary Interface) with 64-bit pointers and long integers. It's a common choice for 64-bit RISC-V systems.

- `-march=rv64i`: This flag specifies the target architecture as the base integer-only RISC-V architecture for 64-bit systems. It focuses on the fundamental integer instructions.



TO see the RISC-V disassembled code : 
```
riscv64-unknown-elf-objdump -d sum_obj.o

```
To disassemble the object file and view its contents, use the following command:
```
riscv64-unknown-elf-objdump -d sum_obj.o | less 
```
To navigate through `less` use : 
+ Press /instance to search for a specific instance.
+ Press ENTER to begin the search.
+ To find the next occurrence, press n.
+ To search for the previous occurrence, press N.
+ To exit the less viewer, press esc, type :q, and then press ENTER.

-O1 optimised main 

here we see that we have 15 line of code in main

Now let us compile the code use `-Ofast` and see the line of exceution  
```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum_obj.o sum_1_n.c
```
-Ofast optimised main 


here we can see that the code is executed in only 12 lines , which is due to the optimisation we applied 



### Running the Assembly code on simulator in debug mode :
```
spike -d pk sum_obj.o
```
![1](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/03990e09-570c-4d77-92dd-7dcb70dfd3e0)


## Integer number representation :
### Unsigned Numbers
Unsigned numbers, also known as non-negative numbers, are numerical values that represent magnitudes without indicating direction or sign.
**Range :** [0, (2^n)-1 ]
### Signed Numbers
Signed numbers are numerical values that can represent both positive and negative magnitudes, along with zero.
**Range :** Positive : [0 , 2^(n-1)-1] Negative : [-1 to 2^(n-1)]

#### TO summarise : 
<p align="center">
  <img src="https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/ecea0456-d480-494f-912d-97f6708d39b5" alt="Image" width="800">
</p>

## LAB for signed and unsigned integer type 

let us run this C code to determine the range of integer type supported by RISC-V 
```
#include <stdio.h>
#include <math.h>

int main () {
	long long int max = (long long int) (pow ( 2, 63) -1);
	long long int min = (long long int) (pow ( 2, 63)* (-1));
	printf("highest number represented by long long int is %lld\n", max);
	printf("lowest number represented by long long int is %lld\n", min );
	return 0;
}
```

Output of code snippet : 

![8](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/602ee994-858a-428c-9768-0789f2bdb4a4)

we can play around with different values , datatype to find their respect max and min values 


</details>


<details>
<summary> Day 2 : Introduction to ABI and Basic Verification Flow </summary>
<br>
	


# Day 2 : Introduction to ABI and Basic Verification Flow
In Day 2 of your course, you will understanding the RISC-V instruction set architecture (ISA) by exploring the various fields of RISC-V instructions and their functions. This knowledge is crucial for gaining a comprehensive understanding of how RISC-V processors execute instructions and how programs are executed at the hardware level.

## Overview of few instructions :
### R-Type (Register-Type):
Operate on registers with fixed operand format.
Examples: ADD, SUB, AND, OR, XOR, SLL, SRL, SRA, SLT, SLTU

### I-Type (Immediate-Type):
Immediate operand and one register operand.
Examples: ADDI, SLTI, XORI, LB, LH, LW, JALR

### S-Type (Store-Type):
Store values from registers to memory.
Examples: SB, SH, SW

### B-Type (Branch-Type):
Conditional branching based on comparisons.
Examples: BEQ, BNE, BLT, BGE, BLTU, BGEU

### U-Type (Upper Immediate-Type):
Larger immediate field for encoding larger constants.
Examples: LUI, AUIPC

### J-Type (Jump-Type):
Unconditional jumps and function calls.
Example: JAL



## Example of RISC-V instruction : 
<p align="center">
  <img src="https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/f8c1fa62-8d2d-4bf4-897b-cca693879e83" alt="Image" width="800">
</p>

- **Opcode [7]:** Indicates the operation type (arithmetic, logic, memory access, control flow) for the instruction, guiding the CPU's execution.
- **rd (Destination Register) [5]:** Represents the destination register, where the operation result will be stored after execution.
- **rs1 (Source Register 1) [5]:** Represents the first source register, holding the value used in the operation (typically first operand).
- **rs2 (Source Register 2) [5]:** Represents the second source register, holding the value used in the operation (typically second operand).
- **func7 and func3 (Function Fields) [7] [3]:** Further specify opcode category and specific operation, enabling more instruction variations.
- **imm (Immediate Value):** Represents an embedded immediate constant within the instruction, used for offsets, constants, or data values.




## Application Binary Interface :

In the context of computer architecture and programming, **ABI** stands for **Application Binary Interface**. It's a set of conventions and rules that dictate how different parts of a software system interact with each other at the binary level. The ABI defines details such as:

+ **Calling Conventions:** Specifies how function calls handle parameters and pass data, including the order of arguments, used registers, and stack frame management.

+ **Register Usage:** Defines how registers are allocated for passing parameters, returning values, and other purposes.

+ **Data Alignment:** Establishes rules for aligning data structures in memory to enhance access efficiency.

+ **Stack Frame Layout:** Determines how the stack is structured during function calls, managing local variable storage.

+ **System Calls:** Describes how applications request services from the operating system through system calls.

+ **Exception Handling:** Outlines how the system manages exceptions like hardware interrupts or software errors.

<p align="center">
  <img src="https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/e156a95b-5fea-41b6-a00c-822c80e92f11" alt="Image" width="800">
</p>

### 32 - ABI registers in RISC-V and their usage:
<p align="center">
  <img src="https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/a7d48468-c612-488f-8ae8-00bfc65cfe65" alt="Image" width="400">
</p>



## Memory Allocations : 
Data can be stored in register by two methods :
+ Directly store in registers
+ Store into registers from memory
  
What sets RISC (Reduced Instruction Set Computer) architecture apart from CISC (Complex Instruction Set Computer) is its emphasis on simplicity and efficiency, particularly regarding memory operations.

In RISC, the load (L) and store (S) instructions play a fundamental role in memory access. They are used to efficiently transfer data between registers and memory. Additionally, arithmetic or logic operations often use register-to-register (reg-to-reg) instructions like ADD.


### CISC VS RISC : 
<p align="center">
  <img src="https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/96ec694b-698c-4841-8122-07fa477afcd6" alt="Image" width="400">
</p>

  
### RISC-V belongs to **litte endian** memory addressing system 

Consider adding two numbers from memory and storing the result back in memory:

```
LW  R1, 0(R2)      ; Load data from memory into register R1
LW  R3, 4(R2)      ; Load another data from memory into register R3
ADD R4, R1, R3     ; Add data in registers R1 and R3, store result in R4
SW  R4, 8(R2)      ; Store the result in R4 back into memory
 ``` 

### Little-Endian Representation:
In a little-endian system, the least significant byte (LSB) is stored at the lowest memory address, and the most significant byte (MSB) is stored at the highest memory address.

```
Memory Address:   0     1     2     3
Stored Value:    78    56    34    12
```

### Big-Endian Representation:

In a big-endian system, the most significant byte (MSB) is stored at the lowest memory address, and the least significant byte (LSB) is stored at the highest memory address.

```
Memory Address:   0     1     2     3
Stored Value:    12    34    56    78
```

## Lab for ABI function call
This is can interesting lab where we write a code along with assembly code . THe C code calls function to find sum written in the ASM .
we then display the results using c code again .

The algorithm will look like this :

<p align="center">
  <img src="https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/36d03a93-1b54-4120-9a26-3cfad88b71b5" alt="Image" width="600">
</p>

c code snipet : ``` custom_call.c```

```
#include <stdio.h>

extern int load(int x, int y); // Declare the external "load" function

int main() {
  int result = 0;              // Initialize the result variable
  int count = 9;               // Initialize the count variable
  result = load(0x0, count+1); // Call the "load" function with arguments
  printf("Sum of numbers from 1 to 9 is %d\n", result); // Print the result
  return 0;                    // Return 0 to indicate successful execution
}


```
ASM code snipet : ``` load.s```
```
.section .text        # Text section where the code resides
.global load          # Declare the function "load" as global
.type load, @function # Define the type of "load" as a function

load:                 # Start of the "load" function

# Initialize a4 with the value of a0 (copy value from a0 to a4)
add a4, a0, zero

# Copy the value of a1 to a2
add a2, a0, a1

# Initialize a3 with the value of a0 (copy value from a0 to a3)
add a3, a0, zero

loop:                 # Label for the loop

# Add the value in a3 to a4 (accumulate)
add a4, a3, a4

# Increment the value in a3 by 1
addi a3, a3, 1

# Compare a3 with a2 (comparison for loop termination)
blt a3, a2, loop       # Branch to "loop" if a3 < a2

# Copy the accumulated value in a4 to a0 (result)
add a0, a4, zero

ret                    # Return from the function


```


### Simulate C Program using Function Call :
+ **Compilation:** To compile C code and Asseembly file use the command
  ``` riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o custom_call.o custom_call.c load.s ```
  this would generate object file custom_call.o.

+ **Execution:** To execute the object file run the command
```spike pk custom_call.o```

Execution output :
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/a6ffd95d-8ddf-4829-989f-316bacae007c)


## Lab : Run C code on a RISC-V CPU
Let us run our simple C code in a RISC-V CPU - PICORV-32 wirtten in verilog .
Steps :
+ We convert our C program to a hex file and load in the memory of CPU
+ Make use of testbech to run the code
+ Display the results

  The picorv design and the shell scripts are already built in a github repo
  ```
  cd
  git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
  
  ```
  Once installed navigate through the ``` riscv_workshop_collaterals/labs```
  run the following command : 
  ```
  chmod 777 rv32im.sh
  ./rv32im.sh
  ```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/44e11507-3c66-4ad9-a70b-22cdb2d398e1)

to make the process easy we make use of shell script : ``` rv32im.sh```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/08c88682-2c1f-431c-8e96-c57613993fc1)

</details>



<details>
<summary>Day -3 : Introduction to Verilog RTL design and Synthesis</summary>


# Day-3-Introduction to Verilog RTL design and Synthesis


**RTL Design**: In simple terms RTL design or Register Transfer Level design is a method in which we can transfer data from one register to another. In RTL design we write code for Combinational and Sequential circuits in HDL(Hardware Description Language) like Verilog or VerilogHDL which can model logical and hardware operation. RTL design can be one code or set of verilog codes. **One key note is that we need to write RTL design with optimized and synthesizable (realizable as physical gates)**.

**Sample RTL design outline:**

	module module_name (port list);
		//declarations;
		//initializations;
		//continuos concurrent assigments;
		//procedural blocks;
	endmodule

**Test Bench**: Using Verilog we can write a test bench to apply stimulus to the RTL design and verify the results of the design by instantiating design with in test bench. Up-front verification becomes very important as design size increases in size and complexity while any project progresses. This ensures simulation results matches with post synthesis results. A test bench can have two parts, the one generates input signals for the model to be tested while the other part checks the output signals from the design under test. It can be represented as follows.
![Capture2](https://user-images.githubusercontent.com/104454253/166088950-634be5a4-7d5a-4b43-9990-711f8f660aaf.JPG)

**Simulation**: RTL design is checked for adherence to its design specification using simulation by giving sample inputs. This helps finding and fixing bugs in the RTL design in the early stages of design development. 

**Simulator**: Simulator is the tool used for this process. It looks for changes on input signals to evaluate outputs. No change in output if there is no change in input signals
Here is the flow of frondend design:
![Capture1](https://user-images.githubusercontent.com/104454253/166088866-80a4e792-7db7-4bf2-b3b5-b4b9b92452a8.JPG)

<details>
 <summary> Introduction to open source simulator iverilog and gtkwave </summary>
	
**iverilog**: iverilog stands for Icarus Verilog. Icarus Verilog is an implementation of the Verilog hardware description language. It supports the 1995, 2001 and 2005 versions of the standard, portions of SystemVerilog, and some extensions.

**Gtkwave**: GTKWave is a fully featured GTK+ based wave viewer for Unix, Win32, and Mac OSX which reads LXT, LXT2, VZT, FST, and GHW files as well as standard Verilog VCD/EVCD files and allows their viewing. 

</details>

### Lab examples using iverilog and gtkwave

Use the command  ```git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git```

This should create a folder ```sky130RTLDesignAndSynthesisWorkshop``` in your directory
    You could see two folders under ```sky130RTLDesignAndSynthesisWorkshop```
+ ***my_lib***: It contains all the standard cell libraries and verilog module
+ ***verilog_files***: It contains all the source code and testbench required for the lab



![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/cf7b32eb-68cc-4805-9877-38ac7105a0ba)


In this session, I've performed simulation of multiplexer. I've added both the RTL design code and test bench code in iverilog to generate vcd file which I used in gtkwave generator to get the output waveformes after simulation. The output was generated by taking the inputs from the testbench code. 

### good_mux.v
``` v
module good_mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```
### tb_good_mux.v
``` v
timescale 1ns / 1ps
module tb_good_mux;
	// Inputs
	reg i0,i1,sel;
	// Outputs
	wire y;

        // Instantiate the Unit Under Test (UUT)
	good_mux uut (
		.sel(sel),
		.i0(i0),
		.i1(i1),
		.y(y)
	);

	initial begin
	$dumpfile("tb_good_mux.vcd");
	$dumpvars(0,tb_good_mux);
	// Initialize Inputs
	sel = 0;
	i0 = 0;
	i1 = 0;
	#300 $finish;
	end

always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule
```


<details>
 <summary> Introduction to Yosys synthesizer </summary>

**Synthesis**: Synthesis transforms the simple RTL design into a gate-level netlist with all the constraints as specified by the designer. In simple language, Synthesis is a process that converts the abstract form of design to a properly implemented chip in terms of logic gates.

Synthesis takes place in multiple steps:
- Converting RTL into simple logic gates.
- Mapping those gates to actual technology-dependent logic gates available in the technology libraries.
- Optimizing the mapped netlist keeping the constraints set by the designer intact.

**Synthesizer**: It is a tool we use to convert out RTL design code to netlist. Yosys is the tool I've used in this workshop.
Here is the flow of above processess.

![rtl-netlist](https://user-images.githubusercontent.com/104454253/166097298-41d913ee-640d-4e1e-9e70-5bf427f35ef4.JPG)

**Yosys**:Yosys is a framework for RTL synthesis and more. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains. Yosys is the core component of most our implementation and verification flows.

I was given an overview of the operation of the tool and the files we'll need to provide the tool to give the required netlist. We give RTL design code, .lib file which has all the building blocks of the netlist. Using these two files, Yosys synthesizer generates a netlist file. .lib basically is a collection of logical modules like, And, Or, Not etc.... These are equivalent gate level representation of the RTL code. 

Below are the commands to perform above synthesis.

- RTL Design  - read_verilog
- .lib        - read_liberty
- netlist file- write_verilog

**Operational flow of Yosys Synthesizer**

![Synthesizer](https://user-images.githubusercontent.com/104454253/166094901-27c70c0d-8ef2-4a34-a4b2-7307af492698.JPG)

**Verification of Synthesized design**: In order to make sure that there are no errors in the netlist, we'll have to verify the synthesized circuit. The netlist verification flow can be seen in the below image:

![Synthesisgtkwave](https://user-images.githubusercontent.com/104454253/166095185-f82dbbe0-afb4-43ac-8ec6-6b75491d6b58.JPG)

The gtkwave output for the netlist should match the output waveform for the RTL design file. As netlist and design code have same set of inputs and outputs, we can use the same testbench and compare the waveforms.

**Introduction to loigc synthesis**: Below is the snippet RTL code and equivalent digital circuit:

![sample rtl](https://user-images.githubusercontent.com/104454253/166097112-0fb5685c-fe88-4ca0-8ecf-bc014de46088.JPG)

In the above image, mapping of code and digital circuit is done using Synthesis.

**.lib**: It is a collection of logical modules like, And, Or, Not etc...It has different flvors of same gate like 2 input AND gate, 3 input AND gate etc... with different performace speed.

**Need for different flavours of gate**: In order to make a faster circuit, the clock frequency should be high. For that the time period of the clock should be as low as possible. However, in a sequential circuit, clock period depends on three factors so that data is not lost or to be glitch free.

For the below circuit the three factors are
- Clock to Q of flipflop A
- Propagation delay of combinational circuit
- Setuptime of flipflop B
![Timedelay circuit](https://user-images.githubusercontent.com/104454253/166098730-33bf0734-abec-466f-abe2-a2ac6813b5e0.JPG)

The equation is as follows

![Time](https://user-images.githubusercontent.com/104454253/166097710-2c1099e3-6323-496c-8eb7-12ee04c12096.JPG)

As per the above equation, for a smaller propagation delay, we need faster cells.
But again, why do we have faster cells? This is to ensure that there are no HOLD time violations at B flipflop.
**This complete collection forms .lib**

**Faster Cells vs Slower Cells**: 
Load in digital circuit is of **Capacitence**. Faster the charging or dicharging of capacitance, lesser is the celll delay. However, for a quick charge/ discharge of capacitor, we need transistors capable of sourcing more current i.e, we need WIDE TRANSISTORS. 

Wider transistors have lesser delay but consume more area and power. Narrow transistors are other way around. Faster cells come with a cost of area and power.

**Selection of the Cells**: We'll need to guide the Synthesizer to choose the flavour of cells that is optimum for implementation of logic circuit. Keeping in view of previous observations of faster vs slower cells,to avoid hold time violations, larger circuits, sluggish circuits, we offer guidance to synthesizer in the form of **Constraints**.

Below is an illustration of Synthesis.

![Screenshot (44)](https://user-images.githubusercontent.com/104454253/166099264-e3842e91-1a27-44ae-830c-0757dc5b1a5e.png)

## Labs on Yosys introduction
Invoking Yosys:

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/27df6f23-b4f5-49fc-99f7-0efe543055ed)


Snippet below illustrates reading .lib, design and choosing the module to synthesize:

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/3f20467d-a4a6-40f2-89d3-811124926b2a)

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/e0cf7107-f767-4595-b56c-37c8798eae46)


**Generating Netlist**: The logic of good_mux will be realizable using gates in the sky130_fd_sc_hd__tt_025C_1v80.lib file

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/5b6da56b-45de-4c22-b9d6-7180424afe36)


Below is the snippet showing the synthesis results and synthesized circuit for multiplexer.


![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/38626be2-6a36-42ed-815c-ae2f2b4330c0)
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/3153d3dc-e532-4ab4-a860-2a10a2df1c8d)


**Netlist code**:

```v

/* Generated by Yosys 0.32+51 (git sha1 6405bbab1, gcc 11.4.0-1ubuntu1~22.04 -fPIC -Os) */

(* top =  1  *)
(* src = "good_mux.v:2.1-10.10" *)
module good_mux(i0, i1, sel, y);
  (* src = "good_mux.v:2.24-2.26" *)
  wire _0_;
  (* src = "good_mux.v:2.35-2.37" *)
  wire _1_;
  (* src = "good_mux.v:2.46-2.49" *)
  wire _2_;
  (* src = "good_mux.v:2.63-2.64" *)
  wire _3_;
  (* src = "good_mux.v:2.24-2.26" *)
  input i0;
  wire i0;
  (* src = "good_mux.v:2.35-2.37" *)
  input i1;
  wire i1;
  (* src = "good_mux.v:2.46-2.49" *)
  input sel;
  wire sel;
  (* src = "good_mux.v:2.63-2.64" *)
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule

```
command flow 
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog good_mux.v 
yosys> synth -top good_mux 
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

yosys> write_verilog good_mux_netlist.v 
yosys> !vim good_mux_netlist.v 

yosys> write_verilog -noattr good_mux_netlist.v
yosys> !vim good_mux_netlist.v 

```
</details>
</details>





