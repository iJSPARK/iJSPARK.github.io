---
title : "Linked List"
categories : 
    - Data Structure
tag :
    - [Linke List, Circular Linked List, Doubly Linked List, Circular Doubly Linked List]  # [C, python]
author_profile: false
sidebar:
    nav: "docs"
use_math: true
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

## Circular Linked List
In a linked list, if tail node point to the head node, it becomes a cicular linke list. [Liked List](https://ijspark.github.io/data%20structure/linkedlist/)
Circular linked list is a suitable data structure for storing data arranged in a ring.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/140265028-1fe4c6a9-d6d9-4a85-84e3-26b92d0cbb7e.png"> 

Cicular linked list different to linked list, pointer is not NULL vaule but address of head node. `p` is node type pointer variable

* Check empty circular linked list
    `list->head == NULL`
---
* Check a circular list with one node
    `list->head->next == list->head`
---
* Check if it is head node
    `p == list->head`
---
* Check if it is tail node
    `p->next == list->head` 

<br/>

## Doubly Linked List

Linked list easy to find next node but can't find front node. That's why the doubly linked list was devised.
Each node has two pointers which point to next node and front node.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/140598091-4159be2f-c6c3-4e79-9339-aa7861bba4f1.png"> 

```c
typedef struct __node {
    Member data;    /* data */
    struct __node *prev;    /* pointer for front node */ 
    struct __node *next;    /* pointer for next node */
} Dnode;
```

* Check if it is head node
    ```c
    p == list->head /* p is node type pointer variable */
    p->prev == NULL 
    ```

* Check if it is tail node
    ```c
    p->next == NULL
    ```

## Circular Doubly Linked List
Circular Doubly Linked List is a combination of the two concepts above.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/140598093-084b2d8d-cd6a-457b-9fbf-bf9d0f862c0c.png"> 

* Dnode is structure node
    1. `data` : member that stores data
    2. `next` : pointer to next node
    3. `prev` : pointer to front node

```c
/*--- Node ---*/
typedef struct __node {
	Member data;            /* data */
	struct __node *prev;    /* pointer to front node */
	struct __node *next;    /* pointer to next node */
} Dnode;
```

---

* Dlist is structure that manage circular doubly linked list
```c
/*--- Dlist manage circular doubly linked list ---*/
typedef struct { 
	Dnode *head;    /* pointer for head dummy node */
	Dnode *crnt;    /* pointer for selected node */
} Dlist;
```

<br/>

### Main function
#### Initialize circular doubly linked list
Function that create circular doubly linked list, list initialize by create empty node. The node on head of list in order to perform insertion and deletion of node. It is called 'dummy node'. Using the dummy node provide consistency in code when inserting, searching, deleting node. The dummy node has front pointer and next pointer point to itself.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/140598096-40e3817f-fcba-42e5-8542-f5c2858a460a.png"> 

```c
/*--- list initialize ---*/
void Initialize(Dlist* list)
{
	Dnode* dummyNode = AllocDNode();    /* create dummy node */
	list->head = list->crnt = dummyNode;    /* on the head */
	dummyNode->prev = dummyNode->next = dummyNode;  /* point to itself */
}
```

---

#### Check if list is empty
 The dummy node has front pointer and next pointer point to itself.

Empty case
1. `list->head->next == list->head`
2. `list->head->prev == list->head`

```c
/*--- is empty? ---*/
static int IsEmpty(const Dlist* list)
{
	return list->head->next == list->head;
}
```

---

#### Search for node
Scan in orderly from the head node using next pointer. 
At this time the head node is not dummy node but next node of dummy node. In other words, location of the node to start searching is not `list->head` but `list->head->next`. 

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/140598099-f6e56b0f-4c84-4f03-b1e2-e50ea457de34.png"> 

```c
/*--- search for a node equal to x with compare function ---*/
Dnode* search(Dlist* list, const Member* x, int compare(const Member* x, const Member* y))
{
	Dnode* ptr = list->head->next;
	while (ptr != list->head) {
		if (compare(&ptr->data, x) == 0) {
			list->crnt = ptr;
			return ptr;     /* searh sucess */
		}
		ptr = ptr->next;
	}
	return NULL;        /* searh fail */
}
```

---

#### Inserts a node immediately after
Insert a node immediately after the p node.
Since the dummy node is at the head of the list, there is no need to process differently for that case inserting into an empty list and inserting at the head of the list.

* Perform logic
    1. Create node to insert, set front pointer of node point to A and next pointer point to C.
    2. Update `p->next` next pointer of node A and `p->next->prev` front pointer of node C to point to node B.
    3. Update selected node `crnt` to point to inserting node.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/140598111-2b2682cd-5b5d-416c-b469-832d3ce43783.png"> 

```c
/*--- insert a node immediately after the p node ---*/
void InsertAfter(Dlist* list, Dnode* p, const Member* x)
{
	Dnode* ptr = AllocDNode();
	Dnode* nxt = p->next;
	p->next = p->next->prev = ptr;
	SetDNode(ptr, x, p, nxt);
	list->crnt = ptr;   /* crnt set inserting node */
}
```

---

#### Insert node into head
Insert a node after the doummy node pointed to by `list->head` using InsertAfter function.

```c
/*--- insert node into a head ---*/
void InsertFront(Dlist* list, const Member* x)
{
	InsertAfter(list, list->head, x);
}
```

---

#### Insert node into tail
`list->head->prev` point to tail node

```c
/*--- insert node into a tail ---*/
void InsertRear(Dlist* list, const Member* x)
{
	InsertAfter(list, list->head->prev, x);
}
```

#### Remove node 
Function that remove the node pointed to by p pointer.

* Perform logic
    1. Update `p->prev->next` next pointer of A node to `p->next` C node.
    2. Update `p->next->prev` front pointer of C node to `p->prev` A node.
    3. Free the memory pointed to by p.
    4. Update `crnt` selected node to point to the node in front of deleted node.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/140598255-51a9a33e-3d87-4e78-988b-b3758017d716.png">

```c
/*--- remove the node pointed to by p pointer ---*/
void Remove(Dlist* list, Dnode* p)
{
	p->prev->next = p->next;
	p->next->prev = p->prev;
	list->crnt = p->prev;   /* select deleted node */
	free(p);
	if (list->crnt == list->head)
		list->crnt = list->head->next;
}
```

#### Remove head node
Remove the head node using Remove function.
Head node location is `list->head->next`.

```c
/*--- remove head node ---*/
void RemoveFront(Dlist* list)
{
	if (!IsEmpty(list))
		Remove(list, list->head->next);
}
```

#### Remove tail node
Remove the tail node using Remove function.
Tail node location is `list->head->prev`.

```c
/*--- remove tail node ---*/
void RemoveRear(Dlist* list)
{
	if (!IsEmpty(list))
		Remove(list, list->head->prev);
}
```

#### Remove selected node
Remove the selected node, delete node pointed to by `list->crnt`.

```c
/*--- remove selected node ---*/
void RemoveCurrent(Dlist* list)
{
	if (list->crnt != list->head)
		Remove(list, list->crnt);
}
```

#### Clear all node
Delete nodes until the list is empty.

```c
/*--- remove all node ---*/
void Clear(Dlist* list)
{
	while (!IsEmpty(list))  /* until the list is empty */
		RemoveFront(list);  /* remove head node */     
}
```

#### Terminate circular doubly linked list
Call the Clear function to delete all nodes and clear the memory area of ​​the dummy node.

```c
/*--- remove circular doubly linked list ---*/
void Terminate(Dlist* list)
{
	Clear(list);    /* delete all node */              
	free(list->head);   /* dummy node memory area */                   
}
```

<br/>

### Program using circular doubly linked list

```c
/* Program using circular doubly linked list */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MEMBER_NO 	1 	/* integer value representing a number */
#define MEMBER_NAME	2 	/* constant value representing the name *

/*--- member data ---*/
typedef struct {
	int no; 			/* no */
	char name[20];      /* name */
} Member;/

/*--- Node ---*/
typedef struct __node {
	Member data;            /* data */
	struct __node *prev;    /* pointer to front node */
	struct __node *next;    /* pointer to next node */
} Dnode;

/*--- Dlist manage circular doubly linked list ---*/
typedef struct { 
	Dnode *head;    /* pointer for head dummy node */
	Dnode *crnt;    /* pointer for selected node */
} Dlist;

/*--- member no compare function ---*/
int MemberNoCmp(const Member* x, const Member* y)
{
	return x->no < y->no ? -1 : x->no > y->no ? 1 : 0;
}

/*--- member name compare function ---*/
int MemberNameCmp(const Member* x, const Member* y)
{
	return strcmp(x->name, y->name);
}

/*--- represent member data ---*/
void PrintMember(const Member* x)
{
	printf("%d %s", x->no, x->name);
}

/*--- represent member data(including \n) ---*/
void PrintLnMember(const Member* x)
{
	printf("%d %s\n", x->no, x->name);
}


/*--- read member data ---*/
Member ScanMember(const char* message, int sw)
{
	Member temp;
	printf("Enter the data to %s.\n", message);
	if (sw & MEMBER_NO) { printf("no : "); scanf("%d", &temp.no); }
	if (sw & MEMBER_NAME) { printf("name : "); scanf("%s", temp.name); }
	return temp;
}

/*--- create node dynamically ---*/
static Dnode* AllocDNode(void)
{
	return calloc(1, sizeof(Dnode));
}

/*--- 노드의 각 멤버에 값을 설정 set each member of node ----*/
static void SetDNode(Dnode* n, const Member* x, const Dnode* prev, const Dnode* next)
{
	n->data = *x;                              /* 데이터 */
	n->prev = prev;                            /* 앞쪽노드에 대한 포인터 */
	n->next = next;                            /* 뒤쪽노드에 대한 포인터 */
}

/*--- is empty? ---*/
static int IsEmpty(const Dlist* list)
{
	return list->head->next == list->head;
}

/*--- list initialize ---*/
void Initialize(Dlist* list)
{
	Dnode* dummyNode = AllocDNode();       /* create dummy node */
	list->head = list->crnt = dummyNode;	/* on the head */
	dummyNode->prev = dummyNode->next = dummyNode; /* point to itself */
}

/*--- 주목노드의 데이터를 출력 ---*/
void PrintCurrent(const Dlist* list)
{
	if (IsEmpty(list))
		printf("주목노드가 없습니다.");
	else
		PrintMember(&list->crnt->data);
}

/*--- 주목노드의 데이터를 출력(줄바꿈 문자 붙임) ---*/
void PrintLnCurrent(const Dlist* list)
{
	PrintCurrent(list);
	putchar('\n');
}

/*--- 함수 compare로 x와 같은 노드를 검색 ---*/
Dnode* search(Dlist* list, const Member* x, int compare(const Member* x, const Member* y))
{
	Dnode* ptr = list->head->next;
	while (ptr != list->head) {
		if (compare(&ptr->data, x) == 0) {
			list->crnt = ptr;
			return ptr;                 /* 검색 성공 */
		}
		ptr = ptr->next;
	}
	return NULL;                        /* 검색 실패 */
}

/*--- 모든 노드의 데이터를 리스트 순으로 출력 ---*/
void Print(const Dlist* list)
{
	if (IsEmpty(list))
		puts("노드가 없습니다.");
	else {
		Dnode* ptr = list->head->next;
		puts("【 모두 보기 】");
		while (ptr != list->head) {
			PrintLnMember(&ptr->data);
			ptr = ptr->next;                /* 뒤쪽노드에 주목 */
		}
	}
}

/*--- 모든 노드의 데이터를 리스트 순서 거꾸로 출력 ---*/
void PrintReverse(const Dlist* list)
{
	if (IsEmpty(list))
		puts("노드가 없습니다.");
	else {
		Dnode* ptr = list->head->prev;
		puts("【 모두 보기 】");
		while (ptr != list->head) {
			PrintLnMember(&ptr->data);
			ptr = ptr->prev;                /* 앞쪽노드에 주목 */
		}
	}
}

/*--- 주목노드를 하나 뒤쪽으로 나아가도록 ---*/
int Next(Dlist* list)
{
	if (IsEmpty(list) || list->crnt->next == list->head)
		return 0;                           /* 나아갈 수 없음 */
	list->crnt = list->crnt->next;
	return 1;
}

/*--- 주목노드를 하나 앞쪽으로 되돌아가도록 ---*/
int Prev(Dlist* list)
{
	if (IsEmpty(list) || list->crnt->prev == list->head)
		return 0;                           /* 되돌아갈 수 없음 */
	list->crnt = list->crnt->prev;
	return 1;
}

/*--- p가 가리키는 노드를 삭제 ---*/
void Remove(Dlist* list, Dnode* p)
{
	p->prev->next = p->next;
	p->next->prev = p->prev;
	list->crnt = p->prev;                   /* 삭제한 노드의 앞쪽노드에 주목 */
	free(p);
	if (list->crnt == list->head)
		list->crnt = list->head->next;
}

/*--- 머리노드를 삭제 ---*/
void RemoveFront(Dlist* list)
{
	if (!IsEmpty(list))
		Remove(list, list->head->next);
}

/*--- 꼬리노드를 삭제 ---*/
void RemoveRear(Dlist* list)
{
	if (!IsEmpty(list))
		Remove(list, list->head->prev);
}

/*--- 주목노드를 삭제 ---*/
void RemoveCurrent(Dlist* list)
{
	if (list->crnt != list->head)
		Remove(list, list->crnt);
}

/*--- 모든 노드를 삭제 ---*/
void Clear(Dlist* list)
{
	while (!IsEmpty(list))                    /* 텅 빌 때까지 */
		RemoveFront(list);                    /* 머리노드를 삭제 */
}

/*--- 원형 이중 연결 리스트의 뒤처리 ---*/
void Terminate(Dlist* list)
{
	Clear(list);                              /* 모든 노드를 삭제 */
	free(list->head);                         /* 더미 노드를 삭제 */
}

/*--- p가 가리키는 노드 바로 뒤에 노드를 삽입 ---*/
void InsertAfter(Dlist* list, Dnode* p, const Member* x)
{
	Dnode* ptr = AllocDNode();
	Dnode* nxt = p->next;
	p->next = p->next->prev = ptr;
	SetDNode(ptr, x, p, nxt);
	list->crnt = ptr;                         /* 삽입한 노드에 주목 */
}

/*--- 머리에 노드를 삽입 ---*/
void InsertFront(Dlist* list, const Member* x)
{
	InsertAfter(list, list->head, x);
}

/*--- 꼬리에 노드를 삽입 ---*/
void InsertRear(Dlist* list, const Member* x)
{
	InsertAfter(list, list->head->prev, x);
}

/*--- menu ---*/
typedef enum {
	TERMINATE, INS_FRONT, INS_REAR, RMV_FRONT, RMV_REAR, PRINT_CRNT,
	RMV_CRNT, SRCH_NO, SRCH_NAME, PRINT_ALL, NEXT, PREV, CLEAR
} Menu;

Menu SelectMenu(void)
{
	int i, ch;
	char* mstring[] = {
		"Insert node into head",   "Insert node into tail",   "Delete head node",
		"Delete tail node",     "Print seleted node",   "Delete selected node",
		"Searh by no",          "Search by number",        "Print data of all node",
		"Delete all node",
	};

	do {
		for (i = TERMINATE; i < CLEAR; i++) {
			printf("(%2d) %-18.18s ", i + 1, mstring[i]);
			if ((i % 3) == 2)
				putchar('\n');
		}
		printf("( 0) Exit : ");
		scanf("%d", &ch);
	} while (ch < TERMINATE || ch > CLEAR);

	return (Menu)ch;
}

/*--- main ---*/
int main(void)
{
	Menu menu;
	Dlist list;
	Initialize(&list);  /* initialize circular doubly linked list */
	do {
		Member x;
		switch (menu = SelectMenu()) {

		case INS_FRONT:
			x = ScanMember("머리에 삽입", MEMBER_NO | MEMBER_NAME);
			InsertFront(&list, &x);
			break;

		case INS_REAR:
			x = ScanMember("꼬리에 삽입", MEMBER_NO | MEMBER_NAME);
			InsertRear(&list, &x);
			break;

		case RMV_FRONT:
			RemoveFront(&list);
			break;

		case RMV_REAR:
			RemoveRear(&list);
			break;

		case PRINT_CRNT:
			PrintLnCurrent(&list);
			break;

		case RMV_CRNT:
			RemoveCurrent(&list);
			break;

		case SRCH_NO:
			x = ScanMember("검색", MEMBER_NO);
			if (search(&list, &x, MemberNoCmp) != NULL)
				PrintLnCurrent(&list);
			else
				puts("그 번호의 데이터가 없습니다.");
			break;

		case SRCH_NAME:
			x = ScanMember("검색", MEMBER_NAME);
			if (search(&list, &x, MemberNameCmp) != NULL)
				PrintLnCurrent(&list);
			else
				puts("그 이름의 데이터가 없습니다.");
			break;

		case PRINT_ALL:
			Print(&list);
			break;

		case NEXT:
			Next(&list);
			break;

		case PREV:
			Prev(&list);
			break;

		case CLEAR: /* delete all node */
			Clear(&list);
			break;
		}
	} while (menu != TERMINATE);

	Terminate(&list);			/* terminate circular doubly linked list */

	return 0;
}
```