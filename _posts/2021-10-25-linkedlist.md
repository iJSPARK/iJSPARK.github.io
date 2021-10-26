---
title : "Linked List"
categories : 
    - Data Structure
tag :
    - [List, Linear List, Linked List]  # [C, python]
author_profile: false
sidebar:
    nav: "docs"
use_math: true
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

#### Problem of linear list  

**Linear List**
```c
/* Member struct */
typedef struct { 
	int mem_no; // member number
	char* name; // name
	char* phone; // phone number
} Person;

Person data[] = {
	{11, "Steve jobs", "111-1111-1111"},
	{22, "Bill gates", "222-2222-2222"},
	{66, "Da vinci", "666-6666-6666"},
	{77, "Elon musk", "777-7777-7777"},
	{0, "", ""},
	{0, "", ""},
};
```

* Before data inserting

|0|1|2|3|4|5|
|:-:|:-:|:-:|:-:|:-:|:-:|
|11|22|66|77|0|0|

* after data inserting

A person with a mem_no 44 was inserted between men_no 22 and 66.

|0|1|2|3|4|5|
|:-:|:-:|:-:|:-:|:-:|:-:|
|11|22|44|66|77|0|

**Problem**

1. The efficiency is not good because all data must move according to data insertion and deletion.
2. Need to know the size of the accumulated data in advance.

---

#### Pointer Linked list

Create node when insert to data for linked list
* Linked list

* Brute-Force

```c
/* (1) Search start */
>......... 
ZABCABCABD
ABCABD

x.........  // mismatch
ZABCABCABD
ABCABD

/* (2) Search start */
.>........
ZABCABCABD
 ABCABD

.ooooox...  // mismatch
ZABCABCABD
 ABCABD

/* (3) Search start */
..>.......
ZABCABCABD
  ABCABD
```

<br/>

* KMP

```c
/* (1) Search start */
>......... 
ZABCABCABD
ABCABD

x.........  // mismatch
ZABCABCABD
ABCABD

/* (2) Search start */
.>........
ZABCABCABD
 ABCABD

.ooooox...  // mismatch
ZABCABCABD
 ABCABD

/* (3) Search start */
..skip>...  // skip 'AB' and start a search from 'C'
ZABCABCABD
    ABCABD
```

Brute-Force compare in order one by one. On the other hand, kmp is skiped `AB` of `ABCABD` and start a search from `C` in (2) of code above after mismatched in (2). 

**How can skip in KMP algorithm?**  

Look at step (2) in the code above.
Characters are matched up to `ABCAB` of `ABCABD`, `AB` is searched repeatly in `ABCAB`.

Look at step (3) in the code above.
Skip characters `AB` is reapted and start search.

---

#### KMP implementation

Parttern is string to search in text

1. In order to implement KMP, if there is mismatched in the process of searching for pattern in the string text, it need to know the location of the pattern to be searched again. However it is inefficient to get a location for everytime, so need to create a table in which the location is stored in advance. Therefore create `skip table`.

2. Need to get repeatly character. To do this, need to use the concept of prefix and suffix.

|0|1|2|3|4|5|
|:-:|:-:|:-:|:-:|:-:|:-:|
|<span style="color:red">A</span>|<span style="color:red">B</span>|C|<span style="color:blue">A</span>|<span style="color:blue">B</span>|D|

Pattern : <span style="color:red">AB</span>C<span style="color:blue">AB</span>D  
Prefix : <span style="color:red">AB</span>  
Suffix : <span style="color:blue">AB</span>  
Text : ZABCABCABD

(1) Characters are matched up to 'ABCAB' of 'ABCABD' and mismatched at 7 index  

|0|1|2|3|4|5|6|7|8|9|  
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|  
|Z|<span style="color:green">A</span>|<span style="color:green">B</span>|<span style="color:green">C</span>|<span style="color:green">A</span>|<span style="color:green">B</span>|<span style="color:red">C</span>|A|B|D|  
||<span style="color:green">A</span>|<span style="color:green">B</span>|<span style="color:green">C</span>|<span style="color:green">A</span>|<span style="color:green">B</span>|<span style="color:red">C</span>||||

(2) Skip 'AB' and start a search from 'C'  

|0|1|2|3|4|5|6|7|8|9|  
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|Z|A|B|C|<span style="color:green">A</span>|<span style="color:green">B</span>|C|A|B|D|
|||||<span style="color:green">A</span>|<span style="color:green">B</span>|C|A|B|D|

Look at step (1) in the code above. <span style="color:blue">AB</span> of text(ZABC<span style="color:blue">AB</span>CABD) and suffix(<span style="color:blue">AB</span>) of pattern(ABC<span style="color:blue">AB</span>D) overlap. Then <span style="color:red">AB</span> of pattern(<span style="color:red">AB</span>CABD) move to <span style="color:blue">AB</span> of text(ZABC<span style="color:blue">AB</span>CABD) in (2).

|0|1|2|3|4|5|6|7|8|9|  
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|Z|A|B|C|<span style="color:blue">A</span>|<span style="color:blue">B</span>|C|A|B|D|
|||||<span style="color:red">A</span>|<span style="color:red">B</span>|C|A|B|D| 

<span style="color:blue">AB</span> of text(ZABC<span style="color:blue">AB</span>CABD) is like suffix(<span style="color:blue">AB</span>) of pattern(ABC<span style="color:blue">AB</span>D). In conclusion, prefix(<span style="color:red">AB</span>) of pattern(<span style="color:red">AB</span>CABD) and suffix(<span style="color:blue">AB</span>) of pattern('ABC<span style="color:blue">AB</span>D') overlap. Therefore, the KMP algorithm should be implemented using the prefix and suffix of the string pattern to be searched.

---

#### Skip table creation

In order to create a skip table, using the prefix and surfix.
After overlapping the two strings pat one index back, the search is started. Then the number of matching characters is stored in the skip table.

(1)
```c
-x....
ABCABD
 ABCABD
```
**skip table**  

|A|B|C|A|B|D|
|:-:|:-:|:-:|:-:|:-:|:-:|
|-|0|||||

<br/>

(2)
```c
-.x...
ABCABD
  ABCABD
```
**skip table**  

|A|B|C|A|B|D|
|:-:|:-:|:-:|:-:|:-:|:-:|
|-|0|0||||

<br/>

(3)
```c
-..o..
ABCABD
   ABCABD

-..oo.
ABCABD
   ABCABD

-..oox
ABCABD
   ABCABD
```
**skip table**  

|A|B|C|A|B|D|
|:-:|:-:|:-:|:-:|:-:|:-:|
|-|0|0|1|2|0|

In (1) and (2), there is no overlapping character, so 0 is stored.
In (3), `A` matches and stores 1, and `B` matches and stores 2 continuously. And `C` is mismatched, so 0 is stored. 

In this way, when searching for a pattern in the text, if the characters do not match, get the location (index) to search again by accessing the skip table.

----

#### KMP algorithm code

```c
#pragma warning (disable:4996)
#include <stdio.h>

/*--- Search pat in txt ---*/
int kmp_match(const char txt[], const char pat[]) 
{
	int pt = 1; // txt cursor
	int pp = 0; // pat cursor
	int skip[1024]; // Store where to start scanning again as many as the prefix and suffix of pat match.

	/* Scanning character be reapted of pat(prefix, suffix), set skip table */
	while (pat[pt] != '\0') {
		if (pat[pt] == pat[pp]) // character match
			skip[pt++] = ++pp; // save the location that searh to start
		else if (pp == 0) // to scan first character
			skip[pt++] = pp; // save 0 in skip and move to next character
		else // character mismatch
			pp = skip[pp - 1]; // Where to start the search
	}


	/* search pat in txt */
	pt = pp = 0; // initialization txt cursor, pat cursor
	while (txt[pt] != '\0' && pat[pp] != '\0') {
		if (txt[pt] == pat[pp]) {  // character match
			pt++;  pp++; // move next character(two cursor each 1 index)
		}
		else if (pp == 0) // search of pat from first character
			pt++; // next pt cursor 
		else
			pp = skip[pp - 1]; // Where to start the search
	}

	if (pat[pp] == '\0') // pat matching complete
		return pt - pp; // Returns the index at which the match starts 

	return -1; // mismatch
}

int main(void)
{
	int idx;
	char str1[256];		// txt 
	char str2[256];		// pat 

	puts("KMP");
	printf("Text : ");
	scanf("%s", str1);
	printf("pattern : ");
	scanf("%s", str2);

	idx = kmp_match(str1, str2);	
	
	if (idx == -1)
		puts("There is no patten in text");
	else
		printf("Match from the %dth character.\n", idx + 1);

	return 0;
}

```

---

#### Big-O

* Time complexity : O(n)
* Space complexity : O(n)

If string length of text is n and length of pattern is m, In Brute-Force, the worst case time complexity is O(mn). KMP to reduce the number of comparisons significantly. Even in the worst case, KMP compares as much as text length, and the time complexity is O(m + n), which is usually O(n), considering that m ≥ n. In terms of memory, skip table requires m more memory. 