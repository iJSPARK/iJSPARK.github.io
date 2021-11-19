---
title : "Overview of Data Structure & Algorithm"
categories : 
    - Data Structure
    - Algorithm
author_profile: false
sidebar:
    nav: "docs"
use_math: true
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

## Overview of program
### What is program?
**Program is expression of data and process the expressed data.**

<br/>

## Overview of Data Structure
### What is data structure?
**Expression and store of data.**

There are kind of data structure.

* Simple Structure
    1. Integer
    2. floating point
    3. Character
    4. String

---

* Linear
    1. List
    2. Stack
    3. Queue

---

* Non-Linear
    1. Tree
    2. Graph

---

* File Structure
    1. Sequential File
    2. Indexed Sequential File
    3. Direct File

---

### Study direction for data structure.
1. <span style='background-color: #dcffe4'>Understanding for model of data structure.</span>  
2. <span style='background-color: #dcffe4'>Implement of data structure in code level.</span> 

<br/>

## Overview of Algorithm
### What is algorithm?
**Problem solving method for stored and expressed data.**

When the data structure is determined, an efficient algorithm can be determined accordingly. The algorithm and depends on the data structure.

---

### Performance analysis method of algorithm
There are two measure here. 

* Time Complexity
* Space Complexity

<span style='background-color: #dcffe4'>In general, we focus on performance speed when algorithm measurement rathar than memory usage.</span>  

Algorithm measurement is necessary to know the increase and decrease of the performance time according to the change of the amount of data to be processed. There are two measure blow. 

1. Count number of operation.
2. Constitute operation count function $T(n)$ for the number of data $n$ to be processed.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142577982-a6fc5002-4ba3-4a2d-bad7-c196e72c44a6.png"> 

1. Number of data ($0 <= n <= a$)
$T(n_1)$ algorithm more fast than $T(n_2)$ algorithm
2. Number of data ($n > a$)
$T(n_2)$ algorithm more fast than $T(n_1)$ algorithm
    
Let's look at the increase in the number of operations according to the number of data increases.
$T(n_2)$ slope constant when the number of data increase but $T(n_1)$ operation count increase rapidly. So $T(n_2)$ algorithm is better than $T(n_1)$. Be that as $T(n_1)$ may be not to say it’s a bad algorithm. $T(n_2)$ is difficult to implement than $T(n_1)$. Therefore if the number of data is small, select $T(n_1)$ algorithm.

---

### Study direction for algorithm.
1. <span style='background-color: #dcffe4'>Find core operation in algorithm.</span>  
2. <span style='background-color: #dcffe4'>Calculate time/space complexity for worst/average case</span> 

---

### Big-O 
Big-O expresses the upper limit of the number of operations according to data increase. Also, it is time/space complexity for worst case.

$O(n) = 2n + 1$  
$O(n^2) = 3n^2$

---








