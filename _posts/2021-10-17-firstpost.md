---
title : "Post Test"
categories : 
    - Data Structure
tag :
    - C # [C, python]
author_profile: true

toc : true # 목차
---

- 이미지 목차
        - 이미지 세부 목차1
        - 이미지 세부 목차2

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