---
title : "Circular Doubly Linked List"
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

Linked list easy to find next node but can't find previous node. That's why the doubly linked list was devised.
Each node has two pointers which point to next node and previous node.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/140598091-4159be2f-c6c3-4e79-9339-aa7861bba4f1.png"> 

```c
typedef struct __node {
    Member data;    /* data */
    struct __node *prev;    /* pointer for previous node */ 
    struct __node *next;    /* pointer for next node */
} Dnode;
```

---

* Check if it is head node
    ```c
    p == list->head /* p is node type pointer variable */
    p->prev == NULL 
    ```

---

* Check if it is tail node
    ```c
    p->next == NULL
    ```

<br/>

## Circular Doubly Linked List
Circular Doubly Linked List is a combination of the two concepts above.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/140598093-084b2d8d-cd6a-457b-9fbf-bf9d0f862c0c.png"> 

* Dnode is structure node
    1. `data` : member that stores data
    2. `next` : pointer to next node
    3. `prev` : pointer to previous node

```c
/*--- Node ---*/
typedef struct __node {
	Member data;            /* data */
	struct __node *prev;    /* pointer to previous node */
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
#### Create node
Create Dnode type object and return the created pointer of object.
This function only run in this source file when insert node. Therefore make it a static function.

 ```c
/*--- create node dynamically ---*/
static Dnode* AllocNode(void)
 {
     return calloc(1, sizeof(Dnode));
}
```

---

#### Set a member value of node
Set the value of two member of a Dnode type object.  
Substitute `x` point to vlaue for n point to new Dnode type object.  
Substitute passed to next as parameter for new node type object.
Substitute passed to prev as parameter for previous node type object.
This function only run in this source file when insert node. Therefore make it a static function.
```c
/*--- set a value each member of node point to by n ---*/
static void SetDNode(Dnode* n, const Member* x, const Dnode* prev, const Dnode* next)
{
	n->data = *x;   /* data */         
	n->prev = prev; /* previous node pointer */                   
	n->next = next; /* next node pointer */   
}
```

---

#### Initialize circular doubly linked list
Function that create circular doubly linked list, list initialize by create empty node. The node on head of list in order to perform insertion and deletion of node. It is called 'dummy node'. Using the dummy node provide consistency in code when inserting, searching, deleting node. The dummy node has previous pointer and next pointer point to itself.

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

#### Check list is empty
 The dummy node has previous pointer and next pointer point to itself.

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
    1. Create node to insert, set previous pointer of node point to A and next pointer point to C.
    2. Update `p->next` next pointer of node A and `p->next->prev` previous pointer of node C to point to node B.
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

---

#### Remove node 
Function that remove the node pointed to by p pointer.

* Perform logic
    1. Update `p->prev->next` next pointer of A node to `p->next` C node.
    2. Update `p->next->prev` previous pointer of C node to `p->prev` A node.
    3. Free the memory pointed to by p.
    4. Update `crnt` selected node to point to the node in previous of deleted node.

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

---

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

---

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

---

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

---

#### Move to next node

```c
/*--- Move seleced node to next node ---*/
int Next(Dlist* list)
{
	if (IsEmpty(list) || list->crnt->next == list->head)
		return 0;                        
	list->crnt = list->crnt->next;
	return 1;
}
```

---

#### Move to previous node

```c
/*--- Move seleced node to previous node ---*/
int Prev(Dlist* list)
{
	if (IsEmpty(list) || list->crnt->prev == list->head)
		return 0;                         
	list->crnt = list->crnt->prev;
	return 1;
}
```

---

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

---

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
	struct __node *prev;    /* pointer to previous node */
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

/*--- set a value each member of node point to by n ---*/
static void SetDNode(Dnode* n, const Member* x, const Dnode* prev, const Dnode* next)
{
	n->data = *x;   /* data */         
	n->prev = prev; /* previous node pointer */                   
	n->next = next; /* next node pointer */   
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
	dummyNode->prev = dummyNode->next =cd dummyNode; /* point to itself */
}

/*--- print data of seleting node ---*/
void PrintCurrent(const List* list)
{
	if (list->crnt == NULL)
		printf("There is no node.");
	else
		PrintMember(&list->crnt->data);
}

/*---  print data of selecting nodes in list order(including \n) ---*/
void PrintLnCurrent(const List* list)
{
	PrintCurrent(list);
	putchar('\n');
}

/*--- print data of all nodes in list order ---*/
void Print(const List* list)
{
	if (list->head == NULL)
		puts("There are no nodes.");
	else {
		Node* ptr = list->head;
		puts("【 View all 】");
		while (ptr != NULL) {
			PrintLnMember(&ptr->data);
			ptr = ptr->next;    /* select next node */
		}
	}
}


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

/*--- print data of all nodes list in reverse order ---*/
void PrintReverse(const Dlist* list)
{
	if (IsEmpty(list))
		puts("There are no nodes.");
	else {
		Dnode* ptr = list->head->prev;
		puts("【 View all 】");
		while (ptr != list->head) {
			PrintLnMember(&ptr->data);
			ptr = ptr->prev;    /* select previous node */
		}
	}
}

/*--- move the current node next one ---*/
int Next(Dlist* list)
{
	if (IsEmpty(list) || list->crnt->next == list->head)
		return 0;                           
	list->crnt = list->crnt->next;
	return 1;
}

/*--- move the current node previous one ---*/
int Prev(Dlist* list)
{
	if (IsEmpty(list) || list->crnt->prev == list->head)
		return 0;                       
	list->crnt = list->crnt->prev;
	return 1;
}

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

/*--- remove head node ---*/
void RemoveFront(Dlist* list)
{
	if (!IsEmpty(list))
		Remove(list, list->head->next);
}

/*--- remove tail node ---*/
void RemoveRear(Dlist* list)
{
	if (!IsEmpty(list))
		Remove(list, list->head->prev);
}

/*--- remove selected node ---*/
void RemoveCurrent(Dlist* list)
{
	if (list->crnt != list->head)
		Remove(list, list->crnt);
}

/*--- remove all node ---*/
void Clear(Dlist* list)
{
	while (!IsEmpty(list))  /* until the list is empty */
		RemoveFront(list);  /* remove head node */     
}

/*--- remove circular doubly linked list ---*/
void Terminate(Dlist* list)
{
	Clear(list);    /* delete all node */              
	free(list->head);   /* dummy node memory area */                   
}

/*--- insert a node immediately after the p node ---*/
void InsertAfter(Dlist* list, Dnode* p, const Member* x)
{
	Dnode* ptr = AllocDNode();
	Dnode* nxt = p->next;
	p->next = p->next->prev = ptr;
	SetDNode(ptr, x, p, nxt);
	list->crnt = ptr;   /* crnt set inserting node */
}

/*--- insert node into a head ---*/
void InsertFront(Dlist* list, const Member* x)
{
	InsertAfter(list, list->head, x);
}

/*--- insert node into a tail ---*/
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
			x = ScanMember("Inserto into head", MEMBER_NO | MEMBER_NAME);
			InsertFront(&list, &x);
			break;

		case INS_REAR:
			x = ScanMember("Inserto into tail", MEMBER_NO | MEMBER_NAME);
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
				puts("There is no data matching that number");
			break;

		case SRCH_NAME:
			x = ScanMember("Search", MEMBER_NAME);
			if (search(&list, &x, MemberNameCmp) != NULL)
				PrintLnCurrent(&list);
			else
				puts("There is no data matching that nanme");
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

		case CLEAR: 
			Clear(&list);
			break;
		}
	} while (menu != TERMINATE);

	Terminate(&list);   /* terminate circular doubly linked list */

	return 0;
}
```