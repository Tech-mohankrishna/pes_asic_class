# ✅VLSI PHYSICAL DESIGN FOR ASIC ✅
# :book: ABOUT THE REPOSITORY

Welcome to my GitHub repository dedicated to VLSI Physical Design for ASICs using open-source tools! Here, we embark on a journey that starts with processor specifications and leverages the power of the RISC-V ISA. We'll build processors from scratch, taking them through the entire RTL to GDS process  that meets various Performance, Power, Area ( PPA ) and manufacturability requirements. The best part? We're doing it all with open-source tools, including the RISC-V toolchain and OpenLane anad many more .
<p align="center">
  <img src="https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/00ea3403-674e-4c70-a86e-a4d39aff4ff8" alt="Image" width="800">
</p>



# TABLE OF CONTENTS
+ [Day 1️⃣: Introduction to RISC-V ISA and GNU Compiler Toolchain](#introduction-to-risc-v-isa-and-gnu-compiler-toolchain)
+ [Day 2️⃣: Introduction to ABI and Basic Verification Flow](#introduction-to-abi-and-basic-verification-flow)
+ [Day 3️⃣: Introduction to Verilog RTL design and Synthesis](#introduction-to-verilog-rtl-design-and-synthesis)
+ [Day 4️⃣: Timing Libs, Heirarchical VS Flat Synthesis, & Effecient Flop Coding Styles](#timing-libs-heirarchical-and-flat-synthesis)
+ [Day 5️⃣: Combinational & Sequential Optimizations](#combinational-and-sequential-optimizations)
+ [Day 6️⃣: GLS, blocking vs non-blocking and Synthesis-Simulation mismatch](#gls-blocking-vs-non-blocking-and-synthesis-simulation-mismatch) <br> 
  	➡ GLS, Synthesis-Simulation mismatch and Blocking/Non-blocking statements<br>
  	➡ Labs on GLS and Synthesis-Simulation Mismatch<br>
  	➡ Labs on synth-sim mismatch for blocking statement<br> 


# 📋 REQUIREMENTS 
+ **OS**: Ubuntu 20 +
+ **Memory**: 200 GB
+ **RAM**: 6 GB



Errors regarding tools installation are resolved in [Resolve Errors Guide](resolve_errors.md)

# Day 1
# INTRODUCTION TO RISC-V ISA AND GNU COMPILER TOOLCHAIN 
[Back to main](#table-of-contents)
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

# DAY-2
# Introduction to ABI and Basic Verification Flow
[Back to main](#table-of-contents)
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



# Day-3
# INTRODUCTION TO VERILOG RTL DESIGN AND SYNTHESIS
[Back to main](#table-of-contents)

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

# Day-4
# timing libs heirarchical and flat synthesis
[Back to main](#table-of-contents)
+ ***Introduction to .lib***

![pvt vs  delay](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/4e36fd52-1bb1-47c9-8c2d-b63e2428ac1d)


+ **Heirarchical V/S Flat Synthesis**

 Hierarchical synthesis is an approach to manage and simplify the design process for such complex circuits by breaking down the design into smaller, manageable blocks or modules.

+ ***Abstraction Levels***: Hierarchical synthesis divides VLSI design into abstraction levels.
+ ***Progressive Refinement***: The design starts at a high level and gradually refines into lower levels.
+ ***Module Organization***: Design is divided into functional modules at each level.
+ ***Efficiency and Parallelism***: Different modules can be designed simultaneously, improving efficiency.
+ ***Modularity***: Modules can be reused and tested independently.
+ ***Complexity Management***: Helps manage the intricacy of modern VLSI designs.
+ ***Optimization***: Different hierarchy stages allow specific optimizations.

### Steps to Hierarchical Synthesis
- Go to verilog_files directory
- once you get to verilog_files directory, Invoke yosys by using the command `yosys`
- once yosys is invoked follow the above sequence of commands
  ``` sh
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
  read_verilog multiple_modules.v
  synth -top multiple_modules
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  show multiple_modules
  write_verilog -noattr multiple_modules_hier.v
  !vim multiple_modules_hier.v
  ```
  
**multiple_modules_hier.v**

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/197681ba-339d-4d93-b233-50f7c77e452b)


### Flat Synthesis
- In flat synthesis, the entire design is synthesized as a single, monolithic entity.
- All modules, submodules, and logic are flattened into a single level of hierarchy.
- This means that all components, regardless of their intended functionality, are combined into a single giant netlist
- Best suited for simple designs where complexity is low and maintainability isn't a significant concern.

### Steps to Flat Synthesis
- Go to verilog_files directory
- once you get to verilog_files directory, Invoke yosys by using the command `yosys`
- once yosys is invoked follow the above sequence of commands
  ``` sh
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
  read_verilog multiple_modules.v
  synth -top multiple_modules
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  flatten
  show
  write_verilog -noattr multiple_modules_flat.v
  !gvim multiple_modules_flat.v
  ```


***multiple_modules.flat.v***
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/d2c8e576-67b6-4815-a043-35602bab17c4)

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/4b5bd2f4-2a8e-477f-af2a-7b9fd50ea7a6)
  


### submodule level synthesis

Certainly, here's a more concise version:

1. **Modularity and Reusability:** Submodule synthesis enables the creation of reusable functional units, streamlining complex VLSI designs by breaking them into manageable components.

2. **Efficiency and Parallelism:** Designers can work simultaneously on different submodules, accelerating the design process and facilitating focused optimization and testing.

3. **Resource Optimization:** Submodule reuse optimizes resource utilization, while independent testing and iterative refinement enhance design quality and speed up development.


- Go to verilog_files directory
- once you get to verilog_files directory, Invoke yosys by using the command `yosys`
- once yosys is invoked follow the above sequence of commands
  ``` sh
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
  read_verilog multiple_modules.v
  synth -top sub_modules1
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  show
  ```
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/8d6d1b57-ba9e-47b2-a203-8fde5de609d9)


# Various Flop Coding Styles and optimization
## Flop coding styles

### **1. Asynchronous Reset D Flip-Flop**
- When an asynchronous reset input is activated (set to '1'), regardless of the clock signal, the stored value is forced to '0'.
- Otherwise, on the positive edge of the clock signal, the stored value is updated with the data input.
#### dff_asyncres.v
``` v
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```

#### simulation : 

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/6c11a10f-be1d-4d5f-b7ae-745685a1a169)

#### Syntehsis :

commands for synthesis :
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/19fca166-61e8-44b9-bbc9-4ae6cd0acd49)



### **2. Synchronous Reset D Flip-Flop**

- When a synchronous reset input is activated (set to '1') at the positive edge of the clock signal, the stored value is forced to '0'.
- Otherwise, on the positive edge of the clock signal, the stored value is updated with the data input.
#### dff_syncres.v
``` v
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
#### simulation :
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/00e20f73-0b52-4cc1-a893-910f780d8610)

#### Syntehsis :

commands for synthesis :
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/6dd7a274-9f3a-4e6b-81ae-b47e57d04cb1)



### **3. D Flip-Flop with Asynchronous Reset and Synchronous Reset**
- This flip-flop combines both asynchronous and synchronous reset features.
- When the asynchronous reset input is activated (set to '1'), the stored value is immediately forced to '0'.
- When the synchronous reset input is activated (set to '1') at the positive edge of the clock signal, the stored value is forced to '0'.
- Otherwise, on the positive edge of the clock signal, the stored value is updated with the data input.
#### dff_asyncres_syncres.v
``` v
module dff_asyncres_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

#### simulation result: 
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/d3346454-ab0d-4458-a792-0d2511a65030)


#### Synthesis :

commands for synthesis :
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres_syncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/88d3e7c5-47e8-467b-a9f0-98fdd1d58807)


### ***4. Asynchronous Set D Flip-Flop***
- When an asynchronous set input is activated (set to '1'), regardless of the clock signal, the stored value is forced to '1'.
- Otherwise, on the positive edge of the clock signal, the stored value is updated with the data input.
#### dff_async_set.v
``` v
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```
#### simulation :
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/16a42ad2-7cc6-44dc-9ba9-4c9ac8a130cd)


#### Syntehsis :

commands for synthesis :
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_set.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/be145c9f-af67-4db1-80f9-3f760039703a)


<details>
<summary> Interesting Optimizations </summary>
<br>

### Interesting Optimization

##### Code: 1
```
module mul2 (input [2:0] a, output [3:0] y);
	assign y = a * 2;
endmodule
```

command to  run : 
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog mul2_net.v
!vim mul2_net.v
```

#### Synthesis : 

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/0b500569-5a6d-4242-b1fd-55185e243a21)

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/9d687a90-c890-42ba-8774-bbfe7e6ba349)

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/831d6105-192b-402d-a9a1-2b1b535448c7)

##### Code:2
```
module mult8 (input [2:0] a , output [5:0] y);
        assign y = a * 9;
endmodule

```
<br>

command to  run : 
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog mul2_net.v
!vim mul2_net.v
```
#### Synthesis : 

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/58214dec-2bf9-4b88-8499-c831683f8bd3)
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/36327782-d902-44d1-bf0f-c179b22b504b)
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/c1e3d351-ee95-4997-97de-0d811053f920)

</details>
 

# Day 5
# Combinational and Sequential Optimizations
[Back to main](#table-of-contents)
<br>

+ ***Introduction to Optimizations***

Complete Theory on Optimization Techniques

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/3527eae1-1f2c-4522-99a8-7e3d8b2555e5)
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/1323e78e-cdaa-49f4-a6b1-6a6ec9020443)
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/c71b8d82-6fd7-4fa4-9269-b1ee97779931)

+ ***Combination Logic Optimization***


### Code: 1

```
module opt_check (input a , input b , output y);
        assign y = a?b:0;
endmodule

```

### synthesis:

commands:

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/a7a8b75b-7a54-49be-afe1-7fd3ef959356)



### Code: 2

```
module opt_check2 (input a , input b , output y);
        assign y = a?1:b;
endmodule

```

### synthesis:

commands:

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v
synth -top opt_check2
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/a7a8b75b-7a54-49be-afe1-7fd3ef959356)


### Code: 3

```
module opt_check3 (input a , input b, input c , output y);
        assign y = a?(c?b:0):0;
endmodule

```
### synthesis:

commands:

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check3.v
synth -top opt_check3
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/95355286-478e-425f-aa15-a9b0e0900ab1)



### Code: 4

```
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule


module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule


module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1);


endmodule


```
### synthesis:

commands:

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt.v
synth -top multiple_module_opt
flatten
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```




![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/c3fb7662-08c9-4085-a372-073a3410d13e)




### Code: 5

```
module sub_module(input a , input b , output y);
 assign y = a & b;
endmodule



module multiple_module_opt2(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
sub_module U2 (.a(b), .b(c) , .y(n2));
sub_module U3 (.a(n2), .b(d) , .y(n3));
sub_module U4 (.a(n3), .b(n1) , .y(y));


endmodule

```
### synthesis:

commands:

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt2.v
synth -top multiple_module_opt2
flatten
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/3bfed236-54b3-461b-a39b-0e2327b9ad7a)



+ ***Sequential Logic Optimization***


### **code - 1**

#### dff_const1.v
``` v
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
        if(reset)
                q <= 1'b0;
        else
                q <= 1'b1;
end

endmodule
~          
```

#### simulation : 

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/6b836fc6-c588-4869-997d-e8b566724e0a)


#### Syntehsis :

commands for synthesis :
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/331151f1-7b22-4cb0-b295-5adbe7e64841)








### **code - 2**

#### dff_const2.v
``` v
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
        if(reset)
                q <= 1'b1;
        else
                q <= 1'b1;
end

endmodule
               
```

#### simulation : 

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/b258b1a9-92dd-42b9-9e54-7c9592ad97fc)



#### Syntehsis :

commands for synthesis :
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const2.v
synth -top dff_const2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```


![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/33a9b0a0-f9d7-4802-ba53-b937e4cd2ea3)

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/3e439cf3-3d9c-4729-94a1-e41b93c22f58)

### **code - 3**

#### dff_const3.v
``` v
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
        if(reset)
        begin
                q <= 1'b1;
                q1 <= 1'b0;
        end
        else
        begin
                q1 <= 1'b1;
                q <= q1;
        end
end

endmodule

               
```

#### simulation : 

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/2afdcc12-770b-40b7-a0de-b5bc5ff1a1de)



#### Syntehsis :

commands for synthesis :
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/c4216171-9f06-48ae-b5e5-f26257062c78)




## Sequential Optimization for unsued outputs
### counter_opt.v
### code - 1

```v
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
        if(reset)
                count <= 3'b000;
        else
                count <= count + 1;
end

endmodule

```


### Synthesis : 
![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/af66acbb-2749-4ee9-ba42-4ef1c7a65121)


### counter_opt2.v
### code - 2

```v
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);

always @(posedge clk ,posedge reset)
begin
        if(reset)
                count <= 3'b000;
        else
                count <= count + 1;
end

endmodule


```


### Synthesis : 


![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/555d1e9f-5f48-4e3b-a74b-e9d5987db445)


# Day 6
# GLS, blocking vs non-blocking and Synthesis-Simulation mismatch
[Back to main](#table-of-contents)
## GLS Concepts And Flow Using Iverilog
### Gate level simulation
+ Gate-level synthesis produces an exact logic representation of the design using basic gates (AND, OR, NOT, etc.). This accuracy ensures that the design behaves as intended at the transistor level.
+ hile higher-level synthesis focuses on functionality, gate-level synthesis optimizes the design for factors like area, power consumption, and timing constraints. This ensures efficient use of resources.
+ It operates at a lower abstraction level than higher-level simulations and is essential for debugging and ensuring circuit correctness.
+ Gate-level synthesis considers gate delays and interconnect delays, allowing for more accurate timing analysis. It aids in achieving timing closure, ensuring signals propagate correctly within specified time limits.

### To perform gate-level simulation using Icarus Verilog (iverilog):

<p align="center">
  <img src="https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/5bbc2545-8e13-4bfb-a463-c88673482694" alt="Image" width="600">
</p>

1. Write RTL code.
2. Synthesize to generate gate-level netlist.
3. Create a testbench in Verilog.
4. Compile both netlist and testbench.
5. Run simulation with compiled files.Debug and iterate as needed.
6. Perform timing analysis if necessary.
7. Generate test vectors for manufacturing tests.

## Synthesis-Simulation mismatch
+ Synthesis-simulation mismatch is when there are differences between how a digital circuit behaves in simulation at the RTL level and how it behaves after gate-level synthesis.
+ This can occur due to optimization, clock domain issues, library differences, or other factors.
+ To address it, ensure consistent tool versions, check synthesis settings, debug with simulation tools, and follow best practices in RTL coding and design.
+ Resolving these mismatches is crucial for reliable hardware implementation.

## Blocking And Non-Blocking Statements
**Blocking Statements**
+ Blocking statements execute sequentially in the order they appear within a procedural block or always block.
+ When a blocking assignment or operation is encountered, the simulation halts and waits for it to complete before moving on to the next statement.
+ Blocking assignments are typically used to describe combinational logic, where the order of execution doesn't matter, and each assignment depends on the previous one.

**Example (Blocking Assignment):** `a = b + c; // b and c must be known before calculating 'a'`

**Non-Blocking Statements**
+ Non-blocking statements allow concurrent execution within a procedural block or always block, making them suitable for describing synchronous digital circuits.
+ When a non-blocking assignment or operation is encountered, the simulation does not wait for it to complete. Instead, it schedules the assignments to occur in parallel.
+ Non-blocking assignments are typically used to model sequential logic, like flip-flops and registers, where parallel execution is required.

**Example (Non-Blocking Assignment):** 
```
always @(posedge clk)
  begin
    b <= a; // Concurrently scheduled assignment
    c <= b; // Concurrently scheduled assignment
  end
```
## Caveats With Blocking Statements

**Caveats with blocking statements in hardware description languages like Verilog include:**
+ **Sequential Execution:** Blocking statements execute sequentially, which may not accurately represent concurrent hardware behavior in the design.

+ **Order Dependency:** The order of blocking statements can affect simulation results, leading to race conditions or unintended behavior.

+ **Combinational Logic Only:** Blocking statements are primarily used for modeling combinational logic, making them less suitable for sequential or synchronous logic.

+ **Limited for Testbenches:** In testbench code, excessive use of blocking statements can lead to simulation race conditions that don't reflect real-world hardware behavior.

+ **Initialization Issues:** In some cases, initializing variables with blocking assignments can lead to unexpected results due to order-dependent initialization.

To mitigate these issues, designers often use non-blocking statements for modeling sequential logic and adopt good coding practices to minimize order dependencies and improve code clarity.


## Labs on GLS and Synthesis-Simulation Mismatch

<details>
	
<summary>Lab: 1 </summary> 

### code: ternary_operator_mux.v
<br>

```
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
        assign y = sel?i1:i0;
        endmodule
```

### Simulation

```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/f35b22c2-8bcc-4c94-8510-bfd429f12bb2)

### Synthesis

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
show

```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/0fdca6cf-b80c-4e68-b6e6-bac1edbf6753)


### GLS section

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/41b9dcf7-4ab1-4960-abbc-5a015034f1c5)

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/5f49b9e1-89ad-4c2b-8b99-11b348beee26)

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/d00b419b-3be8-45bd-b047-131cd9bd31a1)




</details>


<details>
	
<summary>Lab: 2 </summary> 

### code: bad_mux.v
<br>

```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
        if(sel)
                y <= i1;
        else
                y <= i0;
end
endmodule

```

### Simulation

```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/a39ed418-9e26-494e-a639-c5d7fe97ec0a)


### Synthesis

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
show

```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/7dcb4f18-f801-4d58-a96e-3801f190e0c0)



### GLS section

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/51525ed8-b00a-4040-88b9-03b1334cfb8b)

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/631642fd-a911-4838-862b-f79cb887acf7)

</details>



## Labs on synth-sim mismatch for blocking statement 


<details>
	
<summary>Lab: 1 </summary> 

### code: blocking_caveat.v
<br>

```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
        if(sel)
                y <= i1;
        else
                y <= i0;
end
endmodule

```

### Simulation

```
iverilog blocking-caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/4a58ccf9-9a98-417c-90f9-d2bfdc788415)

### Synthesis

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
show

```

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/c3d8d016-b8dd-46c1-8231-dc95a8c2d057)


### GLS

![image](https://github.com/Tech-mohankrishna/pes_asic_class/assets/57735263/a7cbd67b-2740-4a02-9c0f-82122e506519)


</details>



