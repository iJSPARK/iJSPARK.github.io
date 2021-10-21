---
title : "What's inside a computer?"
categories : 
    - Computer Architecture
tag :
    - [CPU, Memory, Motherboard, Input device, Output device, Interconnects, chipset]  # [C, python]
author_profile: false
sidebar:
    nav: "docs"
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

### Inside a computer

<img width="500" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137689774-4bdf1632-913e-4272-a846-c7e8ad052c39.PNG">

Seven main part of a computer: Processor, Memory, Motherboard, Input device, Output device, Interconnects, Chipset.

* **Power supply**  
	<img width="250" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137908862-b351cbc5-9ac0-408b-974c-3f795872ebcc.jpg">
	- AC(Outlet) -> DC(Electronic products)
	- Each component of the computer divides the voltage it needs and provides it evenly at the same time

* **Processor**  
	<img width="300" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137766451-95c4bb58-8e83-4c2c-aa00-702ddc432b83.jpg">
	- Also called CPU(Central Processing Unit)
	- Operate process(excuted program)

* **Memory**  
	- Caches 
	Temporary small memory to copy data ​​for quick access of in/outside CPU

	- Register
	Temporary small memory to copy data ​​for fast access then cache of inside CPU 

	- Main Memory
		- RAM(Random Acess Memory) : Temporary storage memory(Volatile Memory)
		<img width="300" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137910130-65fea509-e083-4982-931c-6a6e77dc032e.png">
			1. DRAM(Dynamic RAM) 
			Data is disappeared and chnaged 
			2. SRAM(Static RAM) 
			Data is changed with passing the time

		- ROM(Read Only Memory)  
		<img width="250" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137910366-b73829ea-72d8-4990-8d39-2d5f4c15fa0f.png">
		Non-Volatile Memory

	- Secondary Memory
		- HDD(Hard Disk Drive)
		Physical storage device(slow)
		- SSD(Solid State Disk)
		Electrically erased and reprogrammed(very fast)  
		<img width="500" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137911466-935659fc-2819-43cb-9cf7-c5327dc90076.PNG">

		- CD-ROM, DVD, FLASH

* **Motherboard**  
	<img width="500" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137911970-350f58db-bdc2-440c-b2a2-74543d84225e.png">
	- Also called mainboard
	-  It holds and allows communication between many of the crucial electronic components of a system, such as the CPU and memory, and provides connectors for other peripherals.
	-  the main printed circuit board (PCB) in general-purpose computers and other expandable systems.

* **Input device**
	- Mouse, Keyboard, Camera, Pen, Touch screen, Barcode Reader, Scanner

* **Ouput device**
	- Printer, Monitor, Speaker, Beam Projector

* **Interconnects**
	- buses : A communication system that transfers data and information between computer communication components and between computers.
	
* **Chipset(Data Flow Management System)**
	- Northbridge(Memory controller hub) : Manage communication between CPU, RAM, GQU, Southbridge, PCI express
	- Southbridge(I/O Controller hub) : Manage for input/output with the device  
	<img width="500" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137914656-e6f962f0-7f22-4e48-b5fc-0d6d8425443b.png">

<br/>

### 1. Processor

<img width="300" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137766451-95c4bb58-8e83-4c2c-aa00-702ddc432b83.jpg">

Intel core i7

* processor also called CPU(Central Processing Unit)  
* Fetches instructions from memory  
* Execute instructions  
* Transfer data from/to memory  

<br/>

<img width="300" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137728048-064378d7-8872-4b7e-ba86-5d0ce5082289.png">

CPU is made of semiconductors called transistors. Semiconductor consist of silicon. Transistor contains a pnp or npn semiconductor. A logic gate can be maded into a trangistor.

<br/>

<img width="700" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137701166-83affd46-46b1-4c0a-be43-1aed2708790c.jpeg">

Logic gates have NOT, AND, NAND, OR, NOR, XOR, XNOR.

<br/>

**Half Adder**

<img width="200" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137704602-379159c3-ae45-4575-b57c-937501a02713.png">

|A|B|Sum|Carry|
|-|-|:-:|:-:|
|0|0|0|0|
|0|1|1|0|
|1|0|1|0|
|1|1|0|1|

<br/>

Half Adder consist of XOR & AND gate.  
The sum operation can be performed using the XOR gate.  
The carry operation can be performed using the AND gate.  
Subtraction can also be done using the binary complement.  
Multiplication and division are also extensions of addition and subtraction.  
**The computer processes all operations using an adder.**

<br/>

**Full Adder**

<img width="250" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137708392-f6c84322-8532-4f56-8b41-b11e69aa2e50.png">

The actually used adder is a full adder. It is more complex than a half adder.

<br/>

<img width="700" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137701816-250a392b-90d1-4fd2-81be-f8e767742b9f.PNG">

All gates can be maded into a NAND gate. 
In modern times, it is used this way because design and manufacture is simplified.

<br/>

<img width="400" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137768774-afbeac03-e875-4946-a1bc-4bdad7db4997.PNG"> 

Intel core i7

**Main components of CPU**
* 6 Core 
* 12 Thread Processor : Hyper threading(executing 2 program per 1s) 
* Memory Controller(Memory controll logic)
* Cache : Temporary small memory to copy data ​​for quick access of in/outside CPU
* Register : Temporary small memory to copy data ​​for fast access then cache of inside CPU
* ALU(Arithmetic Logic Unit) : Calculate arithmetic(+/-...) & logic(AND,OR...)  
* Accumulator : Intermediate value, result value of the ALU operation are temporarily stored.
* Control unit : Decode instruction and sends instructions to the system to execute them(ALU, Memory, In/output devices)

### 2. Memory

* Staroge program and data
* Register, Caches, HDD, CD-ROM, DVD, ROM, FLASH
* RAM()
* SSD()
* 

<img width="700" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137870906-5b5921ff-3667-48ed-938f-ce41443c6cfa.png"> 

**CPU : Data request in quick order(Fetch) -> Decode -> Operation**


<br/>

### 3. Motherboard

<br/>

### 4. Input device

<br/>

### 5. Output device

<br/>

### 6. Interconnects

<br/>

### 7. chipset


```c
#include <stdio.h> 

/*--- recursive function ---*/
void recur(int n)
{
	if (n > 0) {
		recur(n - 1);
		printf("%d\n", n);
		recur(n - 2);
	}
}

int main(void)
{
	int x;
	printf("정수를 입력하세요 : ");
	scanf("%d", &x);

	recur(x);

	return 0;
}
```