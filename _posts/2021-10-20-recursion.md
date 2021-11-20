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

When the recursive function is called, it makes a copy and run the copy. However, if there is no return value, the call is repeated indefinitely. The function should be terminated, but the recursive function called in the middle did not terminate. So, the recusive function must have an exit condition.

```c
#include <stdio.h>

void Recursive(int num) {
    if(num <= 0)    // exit condition
        return;
    printf("Recursive call %d\n", num);
    Recursive(num - 1);    // argument decrease
}

int main(void) {
    Recursive(3);
    return 0;
}
```

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/142724772-40742944-b53a-4e30-8973-d981707e1f56.png"> 




$$
  f(x) = \begin{cases}  
    n f(n-1) & \text{$n >= 1$} \\  
    1 & \text{$n = 0$}  
  \end{cases}
$$