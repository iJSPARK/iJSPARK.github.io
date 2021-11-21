---
title : "Recursion"
categories : 
    - Data Structure
tag :
    - [Recursive function, Recursion, Fibonacci]  # [C, python]
author_profile: false
sidebar:
    nav: "docs"
use_math: true
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

## Basic of recursive function

```c
void Recursive(void) {
    printf("Recursive call\n");
    Recursive();    // itself call 
}
```

Recursive function call is easy to understand as a structure that make a copy of the recursive function and run it.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142723977-4ec55614-a1c0-491f-8e80-66bc76adb9d8.png"> 

When the recursive function is called, it makes a copy and run the copy. However, if there is no return value, the call is repeated indefinitely. The function should be terminated, but the recursive function called in the middle did not terminate. <span style='background-color: #dcffe4'> So, the recusive function must have an exit condition. Also, An argument of recursive function must converge to the exit condition.</span>


### Condition of recursive function

1. Recursive funciton must have an exit condition.
2. An argument of recursive function must converge to the exit condition.

```c
#include <stdio.h>

void Recursive(int num) {
    if(num <= 0)    // exit condition
        return;
    printf("Recursive call %d\n", num);
    Recursive(num - 1);    // argument toward the exit condition
}

int main(void) {
    Recursive(3);
    return 0;
}
```

* Run result
Recursive call 3
Recursive call 2
Recursive call 1

If parameter `num` is less than or equal to 0, function is terminated.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142749561-d03c29f7-cac9-47df-9021-258753a8e2f1.png"> 

If the exit condition of the recursive function is satisfied, the called last function start to return. The function exits after the function return a value.

<br/>

## Design of recursive function
### why use a recursive function
1. Recursive functions simplify complex data structures or algorithms. 
2. Recursive mathematical equation can be translated directly into code.

---

#### Factorial
Factorial can be expressed recursively because it has repeated calculations.
The mathematical equation of factorial is as follows. 

$n! = n \times (n - 1) \times ...\times 2 \times 1$

---

Let's express the factorial recursively.

$n! = n \times (n - 1)!$

---

Let's design the mathematical equation to implement algorithm.

$
f(x) = \begin{cases}  
    n \times f(n-1) & \text{(n >= 1)} \\ 
    1 & \text{(n = 0)} 
  \end{cases}  
$

$$
f(x) = \begin{cases}  
    n \times f(n-1) & \text{$n >= 1$} \\  
    1 & \text{$n = 0$}  
  \end{cases}
$$

---

Let's express the factorial recursively.

```c
#include <stdio.h>

int Factorial(int num) {
    if (num == 0)    // exit condition
        return 1;
    else
        return num * Factorial(num - 1);    // argument toward the exit condition
}

int main(void) {
    printf("5! = %d\n", Factorial(5));
    return 0;
}
```

* Run result
5! = 120