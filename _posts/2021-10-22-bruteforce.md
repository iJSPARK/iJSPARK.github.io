---
title : "Brute-Force"
categories : 
    - Algorithm
tag :
    - [Exhaustive, Bruteforce]  # [C, python]
author_profile: false
sidebar:
    nav: "docs"
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

### How to crack(search) a password by substituting all possible strings one by one
`i` is index of string to search and `^` is mismatch.

(1) mismatch
```c
i........
ABABCDEFG
ABC

.i.......
ABABCDEFG
ABC

..i......
ABABCDEFG
ABC

..^......
ABABCDEFG
ABC
```
  

(2) mismatch
```c
.i.......
ABABCDEFG
 ABC

.^.......
ABABCDEFG
 ABC
```
  
  
(3) match
```c
..i......
ABABCDEFG
  ABC
  
...i.....
ABABCDEFG
  ABC

....i....
ABABCDEFG
  ABC
```

#### Program that search for pattern in text using Brute-Force
`txt` of bf_match function is text and `pat` is called pattern.  
If search match, it returns the first matched index of `txt` and otherwise it returns -1.

```c
/*--- Program that search for a string using Brute-Force ---*/
#pragma warning (disable:4996)
#include <stdio.h>

/* Function that search for pattern in text using Brute-Force */
int bf_match(const char txt[], const char pat[])
{
	int pt = 0; // txt cursor 
	int pp = 0; // pat cursor 
	while (txt[pt] != '\0' && pat[pp] != '\0') {
		if (txt[pt] == pat[pp]) { 
			pt++;
			pp++;
		}
		else {
			pt = pt - pp + 1;   // txt cursor update 
			pp = 0; // pat cursor initialization : to the first index 
		}
	}
	if (pat[pp] == '\0')
		return pt - pp; // return the first matched index
	return -1;
}

int main(void)
{
	int idx;
	char s1[256];   // txt 
	char s2[256];   // pat 

	puts("Brute-Force");
	printf("Text : ");
	scanf("%s", s1);
	printf("Pattern : ");
	scanf("%s", s2);

	idx = bf_match(s1, s2);	
	if (idx == -1)
		puts("There is no patten in text");
	else
		printf("Match from the %dth character.\n", idx + 1);

	return 0;
}

```