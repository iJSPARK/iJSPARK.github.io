---
title : "Processor"
categories : 
    - Computer Architecture
tag :
    - [Processorm CPU]  # [C, python]
author_profile: false
sidebar:
    nav: "docs"
toc: true # 목차
toc_sticky: true
toc_label: false
---

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

* 6 Core 
* 12 Thread Processor : Hyper threading(executing 2 program per 1s) 
* Memory Controller(Memory controll logic)
* Cache : Temporary small memory to copy data ​​for quick access of in/outside CPU
* Register : Temporary small memory to copy data ​​for fast access then cache of inside CPU
* ALU(Arithmetic Logic Unit) : Calculate arithmetic(+/-...) & logic(AND,OR...)  
* Accumulator : Intermediate value, result value of the ALU operation are temporarily stored.
* Control unit : Decode instruction and sends instructions to the system to execute them(ALU, Memory, In/output devices)
