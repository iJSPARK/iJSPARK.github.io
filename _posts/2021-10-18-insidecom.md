---
title : "What's inside a computer?"
categories : 
    - Computer Architecture
tag :
    - [Processor, CPU, Memory, Motherboard, Input device, Output device, Interconnects, chipset]  # [C, python]
author_profile: false
sidebar:
    nav: "docs"
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

## Inside a computer
<img width="500" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137689774-4bdf1632-913e-4272-a846-c7e8ad052c39.PNG">

Seven main part of a computer: Processor, Memory, Motherboard, Input device, Output device, Interconnects, Chipset.

### Power supply
<img width="250" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137908862-b351cbc5-9ac0-408b-974c-3f795872ebcc.jpg">  

- AC(Outlet) -> DC(Electronic products)
- Each component of the computer divides the voltage it needs and provides it evenly at the same time  

<br/>

### Processor 
<img width="300" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137766451-95c4bb58-8e83-4c2c-aa00-702ddc432b83.jpg">

- Also called CPU(Central Processing Unit)
- Operate process(excuted program)  

<br/>

### Memory
- #### Caches
	Temporary small memory to copy data ​​for quick access of in/outside CPU

- #### Register
	Temporary small memory to copy data ​​for fast access then cache of inside CPU  

- #### Main Memory
	- **RAM(Random Acess Memory)** 
	Temporary storage memory(Volatile Memory)
	<img width="300" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137910130-65fea509-e083-4982-931c-6a6e77dc032e.png">
	1. **DRAM(Dynamic RAM)**  
	Data is disappeared and chnaged
	2. **SRAM(Static RAM)**  
	Data is changed with passing the time  

	<br/>

	- **ROM(Read Only Memory)**

		<img width="250" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137910366-b73829ea-72d8-4990-8d39-2d5f4c15fa0f.png"> 
		
		Non-Volatile Memory 

<br/>

- #### Secondary Memory
	<img width="500" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137911466-935659fc-2819-43cb-9cf7-c5327dc90076.PNG">

	- **HDD(Hard Disk Drive)**  
	Physical storage device(slow)

	- **SSD(Solid State Disk)**  
	Electrically erased and reprogrammed(very fast)


	- CD-ROM, DVD, FLASH

<br/>

### Motherboard
<img width="500" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137911970-350f58db-bdc2-440c-b2a2-74543d84225e.png">

- Also called mainboard
-  It holds and allows communication between many of the crucial electronic components of a system, such as the CPU and memory, and provides connectors for other peripherals.
-  the main printed circuit board (PCB) in general-purpose computers and other expandable systems.

<br/>

### Input device
- Mouse, Keyboard, Camera, Pen, Touch screen, Barcode Reader, Scanner

### Ouput device
- Printer, Monitor, Speaker, Beam Projector

### Interconnects
- **buses** : A communication system that transfers data and information between computer communication components and between computers.
	
### Chipset
**Chipset is data flow management system**

<img width="500" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137914656-e6f962f0-7f22-4e48-b5fc-0d6d8425443b.png">

- **Northbridge(Memory controller hub)** : Manage communication between CPU, RAM, GQU, Southbridge, PCI express
- **Southbridge(I/O Controller hub)** : Manage for input/output with the device 

<br/>

#### Memory
* Staroge program and data
* Register, Caches, HDD, CD-ROM, DVD, ROM, FLASH
* RAM()
* SSD()
* 

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/137870906-5b5921ff-3667-48ed-938f-ce41443c6cfa.png"> 

**CPU : Data request in quick order(Fetch) -> Decode -> Operation**

