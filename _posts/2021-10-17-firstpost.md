---
title : "Post Test"
categories : 
    - Data Structure
tag :
    - C # [C, python]
author_profile: false
sidebar:
    nav: "docs"
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

### 1. 접미사 접두사 이용
### 2. kmp 알고리즘
### 3. boyer-moore 알고리즘

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