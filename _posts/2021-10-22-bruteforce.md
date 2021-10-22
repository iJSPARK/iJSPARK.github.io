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
---

### How to crack(search) a password by substituting all possible strings one by one

#### Program that search for string2 in string1 using Brute-Force
text(`txt`) of bf_match function is string1 and string2 is called pattern(`pat`).
If search match, it returns first index + 1 of string1 and otherwise it returns -1.

```c
/*--- Program that search for a string using Brute-Force ---*/
#pragma warning (disable:4996)
#include <stdio.h>

/* Function that search for a string using Brute-Force */
int bf_match(const char txt[], const char pat[])
{
	int pt = 0;		/* txt cursor */
	int pp = 0;		/* pat cursor */
	while (txt[pt] != '\0' && pat[pp] != '\0') {
		if (txt[pt] == pat[pp]) {
			pt++;
			pp++;
		}
		else {
			pt = pt - pp + 1;
			pp = 0;
		}
	}
	if (pat[pp] == '\0')
		return pt - pp;
	return -1;
}

int main(void)
{
	int idx;
	char s1[256];		/* txt */
	char s2[256];		/* pat */

	puts("Brute-Force");
	printf("Text : ");
	scanf("%s", s1);
	printf("Pattern : ");
	scanf("%s", s2);

	idx = bf_match(s1, s2);	// search for pattern(s2) in text(s1) using Brute-Force
	if (idx == -1)
		puts("There is no patten in text");
	else
		printf("Match from the %dth character.\n", idx + 1);

	return 0;
```