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

1. Recursive function must have an exit condition.
2. An argument of recursive function must converge to the exit condition.

```c
#include <stdio.h>

void Recursive(int n) {
    if(n <= 0)    // exit condition
        return;
    printf("Recursive call %d\n", n);
    Recursive(n - 1);    // argument toward the exit condition
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

If parameter `n` is less than or equal to 0, function is terminated.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142752327-6d83552d-790b-49bc-afb3-c779de33c336.png"> 

If the exit condition of the recursive function is satisfied, the called last function start to return. The function exits after the function return a value.

<br/>

## Design method of recursive function
### why use a recursive function
1. Recursive functions **simplify** complex data structures or algorithms. 
2. Recursive **mathematical expression** can be translated directly into code.

---

### Factorial
Factorial can be expressed recursively because it has repeated calculations. 
First, it is important to define the recursive function well. All that remains is to implement it well.
The mathematical expression of factorial is as follows. 

$n! = n \times (n - 1) \times ...\times 2 \times 1$

---

Let's express the factorial recursively.

$n! = n \times (n - 1)!$

---

Let's design the mathematical expression to implement algorithm.

$$
f(x) = \begin{cases} 
    n \times f(n-1) & \text{(n >= 1)} \\ 
    1 & \text{(n = 0)} 
  \end{cases} 
$$

---

Let's express the factorial recursively.

```c
#include <stdio.h>

int Factorial(int n) {
    if (n == 0)    // exit condition
        return 1;
    else
        return n * Factorial(n - 1);    // argument toward the exit condition
}

int main(void) {
    printf("5! = %d\n", Factorial(5));
    return 0;
}
```

**Run Result**  
5! = 120  

<br/>

### Fibonacci Sequence
Fibonacci sequence is a representative sequence of recursive functions. 
0, 1, 1, 2, 3, 5, 8, 13, 21...

---

#### Definition
**Initial value**  
$F_1 = 0$&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$(n = 1)$
$F_2 = 1$&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$(n = 2)$

**Recurrence formula**  
$F_n = F_{n-1} + F_{n-2}$ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$(n > 2)$

---

Let's design the mathematical expression to implement algorithm.

$$
f(x) = \begin{cases}  
    0 & \text{(n = 1)} \\
    1 & \text{(n = 2)} \\
    f(n-1) + f(n - 2) & \text{(n > 2)} \\  
  \end{cases}  
$$

---

#### Algorithm Impelment
Let's express the factorial recursively.

```c
#include <stdio.h>

int Fibonacci(int n){
    if (n == 1)
        return 0;
    else if (n == 2)
        return 1;
    else
        return Fibonacci(n - 1) + Fibonacci(n - 2);
}

int main() {
    for(int i = 1; i <= 7; i++) // start from 1
        printf("%d ", Fibonacci(i));    // up to the 7th
    return 0;
}
```

**Run Result**  
0, 1, 1, 2, 3, 5, 8

<br/>

### The Tower of Hanoi
#### Understanding & Find Pattern
How to move a disc stacked on one bar to another disc as it is. It complies with the following rules:

1. Only one disc can be moved at a time.
2. A big disc can't be placed on top of a small disc.

Let's take a look the tower of Hanoi when there are 3 discs.

* A : Start point
* B : Middle point
* C : Target point

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142840847-011bb319-84e3-4cec-9a85-5c698443e21c.png">

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142840858-ce36be1b-dc26-42b9-a4b1-60922fdf37c9.png">

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142840868-411c5958-812c-4a7b-bb1f-c09d36516030.png">

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142840878-c6d6668a-3efe-4003-984e-a099791a044c.png">

---

This time, let's take a look the tower of Hanoi when there are 2 discs. Compare it to when there are 3 discs.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142839487-461b908f-7d47-45cb-8284-ca5782cc977c.png">

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142839497-113e332b-826e-4c26-857e-dc6eb8ff1d20.png">

* Transfer order and method
1. Move the small disc(no.1)from A to B. 
2. Move the big disc(no.2)from A to C. 
3. Move the small disc(no.1)from B to C. 

---

Let's look at the three discs again. First, no.3 disc should be moved to C. However no.1, no.2 must be moved first. 

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142845122-4cc3c0cb-2564-4943-8cf0-ec0d2833737b.png">

Then, how about thinking of the above discs as one disc except for the last one?

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142845149-8978c60f-d2d8-4c47-844a-ad6f747170e1.png">

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142846723-254680cf-2431-43e4-9b67-539f37a7f96e.png">

This is the same as when there are two discs.  
Let's **generalize** the pattern of movement order.

1. Move $n - 1$ small disc from A to B. 
2. Move $1$ big disc from A to C. 
3. Move $n - 1$ small disc from B to C. 

Then, we have to find **exit condition**. In the above case, step 3 is completed. <span style='background-color: #dcffe4'>So, if remain of discs to move is one, the escape condition is satisfied.</span> 

---

#### Algorithm Implement

**Recursive Pattern**  
Step 1. Move $n - 1$ small disc(last disc exception) from A to B.  
Step 2. Move $1$ big disc(last disc) from A to C.  
Step 3. Move $n - 1$ small disc(moved in 1 step) from B to C.  

**Exit Condition**  
 Remain of the discs to move is one, top disc of Hanoi's Tower(No.1 disc).

```c
#include <stdio.h>

// n : The number of disc to move
// disc movement : from >> via >> to
void HanoiMove(int n, char from, char via, char to) {
    if (n == 1) // exit condition
        printf("No.1 disc from % c move to % c\n", from, to);
    else {  // Movement
        HanoiMove(n - 1, from, to, via);    // step 1
        printf("No.%d disc from %c move to %c\n", n, from, to);    // step 2(last disc is No.n disc)
        HanoiMove(n - 1, via, from, to);     // step 3
    }
}

int main() {
    HanoiMove(3, 'A', 'B', 'C');
    return 0;
}
```

**Run Result**  
No.1 disc from A move to C  
No.2 disc from A move to B  
No.1 disc from C move to B  
No.3 disc from A move to C  
No.1 disc from B move to A  
No.2 disc from B move to C  
No.1 disc from A move to C  