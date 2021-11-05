---
title : "Linked List"
categories : 
    - Data Structure
tag :
    - [Linear List, Linked List]  # [C, python]
author_profile: false
sidebar:
    nav: "docs"
use_math: true
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

## Linear List  
Linear List made by array 

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

---

* After data inserting
A person with a mem_no 44 was inserted between men_no 22 and 66.  

    |0|1|2|3|4|5|
    |:-:|:-:|:-:|:-:|:-:|:-:|
    |11|22|<span style="color:blue">44</span>|<span style="color:red">66</span>|<span style="color:red">77</span>|0|

**Problem of linear list**

1. The efficiency is not good because all data must <span style="color:red">move</span> according to data <span style="color:blue">insertion and deletion</span>.
2. Need to know the size of the accumulated data in advance.

<br/>

## Linked List using pointer
Linear list problems can be solved using linked lists. Create node type object when insert to data for linked list. Each node's next pointer to the next node. The first node of linked list is the head node and last node is tail node. The tail node has null value.

* Node type object consist of data and next.
    1. data : member that stores data
    2. next : pointer to a struct like itself

```c
tyedef struct node {
   Member data; // data
   struct node *next; // pointer to the next node 
} Node;
```

---

Struct Member in Node be used as data in any form.
```c
typedef struct {
    // data_type x;
    // data_type y;
} Member;
```

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139399619-e5cbfd67-f281-45b5-8f34-534880b55668.png"> 

 **If delete node object when delete data, solve the problem that push and pull data.**

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139523058-0f51d7c1-3260-4ae4-a430-d70f1fd17b0c.png">

### Struct List
List manage a liked list, consist of two member and have point data type for node.

```c
/*--- linked list ---*/
typedef struct {
	Node* head;     /* pointer for head node */
	Node* crnt;	/* pointer for selected node */
} List;
```

<br/>

### Main function
#### Initialize linked list
Initialize before use linked list.  
Create empty linked list by substitue NULL value for head pointing to the head node. crnt pointer is the currently selected node. It's used for select and delete node searched. 

 <img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139399661-f7199682-7d2d-4359-8cae-6fc598e5677d.png"> 

```c
/*--- initialize linked list ---*/
void Initialize(List* list)
{
    list->head = NULL;	/* head node */
    list->crnt = NULL;	/* selected node */
}
```

---

#### Create node
Create node type object and return the created pointer of object.
This function only run in this source file when insert node. Therefore make it a static function.
 ```c
/*--- create node dynamically ---*/
static Node* AllocNode(void)
 {
     return calloc(1, sizeof(Node));
}
```

---

#### Set a member value of node
Set the value of two member of a node type object.  
Substitute x point to vlaue for n point to new node type object.  
Substitute passed to next as parameter for new node type object.   
This function only run in this source file when insert node. Therefore make it a static function.
```c
/*--- set a value each member of node point to by n ---*/
static void SetNode(Node* n, const Member* x, const Node* next)
{
    n->data = *x;		/* data */
    n->next = next;		/* pointer for next node */
}
```


---

#### Searh node
Search for nodes that match the condition.  
Seraching is linear scan and start from head node.  
Return value is pointer about the found node.  

* Termination condition
    1. Don't find the node matching condition and shortly before passing tail node
    2. Search for nodes matching condition.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139396570-22ff3c3d-0f5f-4034-94c3-525390f9346e.png">

```c
/*--- search for a node equal to x with compare function ---*/
Node* search(List* list, const Member* x, int compare(const Member* x, const Member* y))
{
    Node* ptr = list->head;
    while (ptr != NULL) {
        if (compare(&ptr->data, x) == 0) {	/* return 0 if same the key value */
            list->crnt = ptr;
            return ptr;		/* search sucess */
        }
        ptr = ptr->next;	/* next node */
    }
    return NULL;		/* search fail */
}
```

---

#### Insert node to head
Update head node and crnt to point to the newly created node.  
Set a value by calling SetNode function.  
    
<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139523150-95260178-04a7-42f7-b9a5-b4cb5106bb46.png">

```c
/*--- insert a node into the head ---*/
void InsertFront(List* list, const Member* x)
{
    Node* ptr = list->head; /* substitute pointer about head node for ptr */
    list->head = list->crnt = AllocNode();  /* create node and insert the node to head*/
    SetNode(list->head, x, ptr);    /* set a value of node */
}
```

---

#### Insert node to tail
* Processing according to logic
    1. List is empty
        Same do that insert node to head so do as InserFront function.
    2. List isn't empty
        Insert newly node to tail.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139523081-4159c9cd-e277-486b-8672-2754a531dc12.png">

```c
/*--- insert a node into the tail ---*/
void InsertRear(List* list, const Member* x)
{
    if (list->head == NULL)	/* is empty */
        InsertFront(list, x);	/* insert to head */
    else {
        Node* ptr = list->head;
        while (ptr->next != NULL)   /* find tail node */
            ptr = ptr->next; 
        ptr->next = list->crnt = AllocNode();   /* create node and insert the node to tail */
        SetNode(ptr->next, x, NULL);    /* tail node point to noting */
    }
}
```

---

#### Delete head node 
If list is not empty, execute deletion.  
Update ptr pointer to second node and free memory for head.  
Set head to ptr pointer and crnt pointer after free.  

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139523083-17f7729b-da0d-469a-920f-c2a8c8344ab9.png">

```c
/*--- remove head node ---*/
void RemoveFront(List* list)
{
    if (list->head != NULL) {
        Node* ptr = list->head->next;		/* pointer about second node */
        free(list->head);			/* free head node */
        list->head = list->crnt = ptr;		/* new head node */
    }
}
```

---

#### Delete tail node
* Processing according to the number of nodes
    1. One node in list
        Same that delete head node so run RemoveFront function.
    2. Node more than two in list
        <span style='background-color: #dcffe4'>The node in front of the tail node must have a NULL value as the next value.</span> Therfore decalre pre pointer that node in front of the node currently being scanned.
                                Update crnt node in fornt of deleting node.
        
<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139523088-fe8746c0-ff3f-4500-a0e7-6c9f56182434.png">

```c
/*--- remove tail node ---*/
void RemoveRear(List* list)
{
    if (list->head != NULL) { /* isn't empty */
        if ((list->head)->next == NULL)	/* node just 1 quantity */
            RemoveFront(list);			/* delete head node */
        else {
            Node* ptr = list->head; /* head node */
            Node* pre = NULL;   /* node in front of the node currently being scanned */
            while (ptr->next != NULL) { /* find tail node */
                pre = ptr; //
                ptr = ptr->next;
            }

            pre->next = NULL;	/* in front of tail node */
            free(ptr);		/* free tail node */
            list->crnt = pre;   /* update crnt node in fornt of deleting node */
        }
    }
}
```       

---

#### Delete selected node 
* Processing according to logic
    1. crnt is head node
        Delete the head node with RemoveFont function.
    2. crnt is not head node
            <span style='background-color: #dcffe4'>Find a selected node and ptr pointer link to next node of crnt.</span> 
            Free memory selected node and update crnt point to node in fornt of deleted node(ptr).

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139523092-ddc134bd-18d9-4803-b90b-95043c3b7515.png">

```c
/*--- remove selected node ---*/
void RemoveCurrent(List* list)
{
    if (list->head != NULL) {
        if (list->crnt == list->head)	/* select head node */
            RemoveFront(list);			/* delete head node */
        else {
            Node* ptr = list->head;	/* head node */
            while (ptr->next != list->crnt) /* find selected node */
                ptr = ptr->next;
            ptr->next = list->crnt->next; /* ptr move next node pointer of crnt */
            free(list->crnt); /* delete current node */
            list->crnt = ptr; /* update crnt node in fornt of deleting node */
        }
    }
}
````

---

**Linked List program source file**
```c
/* Linked List using pointer(source) */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MEMBER_NO 	1   /* integer value representing a number */
#define MEMBER_NAME	2   /* constant value representing the name */

/*--- member data ---*/
typedef struct {
	int no;         /* no */
	char name[20];  /* name */
} Member;

/*--- node ---*/
typedef struct __node {
	Member data;		/* data */
	struct __node* next;	/* pointer for next node */
} Node;

/*--- linked list ---*/
typedef struct {
	Node* head;		/* pointer for head node */
	Node* crnt;		/* pointer for selected node */
} List;

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
static Node* AllocNode(void)
{
	return calloc(1, sizeof(Node));
}

/*--- set a value each member of node point to by n ---*/
static void SetNode(Node* n, const Member* x, const Node* next)
{
	n->data = *x;		/* data */
	n->next = next;		/* pointer for next node */
}

/*--- initialize linked list ---*/
void Initialize(List* list)
{
	list->head = NULL;	/* head node */
	list->crnt = NULL;	/* selected node */
}

/*--- search for a node equal to x with compare function ---*/
Node* search(List* list, const Member* x, int compare(const Member* x, const Member* y))
{
	Node* ptr = list->head;
	while (ptr != NULL) {
		if (compare(&ptr->data, x) == 0) {  /* same the key value */
			list->crnt = ptr;
			return ptr;     /* search sucess */
		}
		ptr = ptr->next;        /* select next node */
	}
	return NULL;		        /* search fail */
}

/*--- insert a node into the head ---*/
void InsertFront(List* list, const Member* x)
{
	Node* ptr = list->head; /* substitute pointer about head node for ptr */
	list->head = list->crnt = AllocNode(); /* create node and insert the node to head*/
	SetNode(list->head, x, ptr); /* set a value of node */
}

/*--- insert a node into the tail ---*/
void InsertRear(List* list, const Member* x)
{
	if (list->head == NULL)		/* is empty */
		InsertFront(list, x);	/* insert to head */
	else {
		Node* ptr = list->head;
		while (ptr->next != NULL) /* find tail node */
			ptr = ptr->next; 
		ptr->next = list->crnt = AllocNode(); /* create node and insert the node to tail */
		SetNode(ptr->next, x, NULL); /* tail node point to noting */
	}
}

/*--- remove head node ---*/
void RemoveFront(List* list)
{
	if (list->head != NULL) {
		Node* ptr = list->head->next;	/* pointer about second node */
		free(list->head);		/* free head node */
		list->head = list->crnt = ptr;	/* new head node */
	}
}

/*--- remove tail node ---*/
void RemoveRear(List* list)
{
	if (list->head != NULL) {   /* isn't empty */
		if ((list->head)->next == NULL) /* node just 1 quantity */
			RemoveFront(list);	/* delete head node */
		else {
			Node* ptr = list->head; /* head node */
			Node* pre = NULL;   /* node in front of the node currently being scanned */
			while (ptr->next != NULL) { /* find tail node */
				pre = ptr; 
				ptr = ptr->next;
			}

			pre->next = NULL;	/* in front of tail node */
			free(ptr);		/* free tail node */
			list->crnt = pre;       /* connect linked list */
		}
	}
}

/*--- remove selected node ---*/
void RemoveCurrent(List* list)
{
	if (list->head != NULL) {
		if (list->crnt == list->head)   /* select head node */
			RemoveFront(list);	/* delete head node */
		else {
			Node* ptr = list->head;	/* head node */
			while (ptr->next != list->crnt) /* find selected node */
				ptr = ptr->next;
			ptr->next = list->crnt->next; /* ptr move next node pointer of crnt */
			free(list->crnt); /* delete current node */
			list->crnt = ptr; update crnt node in fornt of deleting node */
		}
	}
}

/*--- remove all nodes ---*/
void Clear(List* list)
{
	while (list->head != NULL)	/* until empty */
		RemoveFront(list);	/* delete head node */
	list->crnt = NULL;
}

/*--- print data of seleted node ---*/
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
			ptr = ptr->next;    /* select rear node */
		}
	}
}

/*--- terminate linked list ---*/
void Terminate(List* list)
{
	Clear(list);    /* delete all node */
}

/*--- menu ---*/
typedef enum {
	TERMINATE, INS_FRONT, INS_REAR, RMV_FRONT, RMV_REAR, PRINT_CRNT,
	RMV_CRNT, SRCH_NO, SRCH_NAME, PRINT_ALL, CLEAR
} Menu;

/*--- select menu ---*/
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
	List list;

	Initialize(&list);	/* initialize linear list */
	do {
		Member x;
		switch (menu = SelectMenu()) {
			
		case INS_FRONT: /* insert node into head */
			x = ScanMember("Insert into the head", MEMBER_NO | MEMBER_NAME);
			InsertFront(&list, &x);
			break;

		case INS_REAR:	/* insert node into tail */
			x = ScanMember("Insert into the tail", MEMBER_NO | MEMBER_NAME);
			InsertRear(&list, &x);
			break;

		case RMV_FRONT:	/* delete head node */
			RemoveFront(&list);
			break;

		case RMV_REAR:	/* delete tail node */
			RemoveRear(&list);
			break;

		case PRINT_CRNT: /* print seleted node */
			PrintLnCurrent(&list);
			break;

		case RMV_CRNT: /* delete selected node */
			RemoveCurrent(&list);
			break;

		case SRCH_NO: /* searh by no */
			x = ScanMember("Search", MEMBER_NO);
			if (search(&list, &x, MemberNoCmp) != NULL)
				PrintLnCurrent(&list);
			else
				puts("There is no data for that number.");
			break;

		case SRCH_NAME: /* search by number */
			x = ScanMember("Search", MEMBER_NAME);
			if (search(&list, &x, MemberNameCmp) != NULL)
				PrintLnCurrent(&list);
			else
				puts("There is no data for that name.");
			break;

		case PRINT_ALL: /* print data of all node */
			Print(&list);
			break;

		case CLEAR: /* delete all node */
			Clear(&list);
			break;
		}
	} while (menu != TERMINATE);

	Terminate(&list);               

	return 0;
}
```