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

* After data inserting
A person with a mem_no 44 was inserted between men_no 22 and 66.  

    |0|1|2|3|4|5|
    |:-:|:-:|:-:|:-:|:-:|:-:|
    |11|22|<span style="color:blue">44</span>|<span style="color:red">66</span>|<span style="color:red">77</span>|0|

<br/>

**Problem of linear list**

1. The efficiency is not good because all data must <span style="color:red">move</span> according to data <span style="color:blue">insertion and deletion</span>.
2. Need to know the size of the accumulated data in advance.

---

## Linked List using pointer
Create node type object when insert to data for linked list. Each node's next pointer to the next node. The first node of linked list is the head node and last node is tail node. The tail node has null value.

* Node type object consist of data and next.
    1. data : member that stores data
    2. next : pointer to a struct like itself

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139399619-e5cbfd67-f281-45b5-8f34-534880b55668.png"> 

```c
tyedef struct node {
   Member data; // data
   struct node *next; // pointer to the next node 
} Node;
```

Struct Member in Node be used as data in any form.

```c
typedef struct {
    // data_type x;
    // data_type y;
} Member;
```

 **If delete node object when delete data, solve the problem that push and pull data.**

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139523058-0f51d7c1-3260-4ae4-a430-d70f1fd17b0c.png">

### Struct List mange a liked list 
List manage a liked list, consist of two member and have point data type for node.

```c
/*--- linked list ---*/
typedef struct {
	Node* head;     /* pointer for head node */
	Node* crnt;	/* pointer for selected node */
} List;
```
<br/>

**Linked List header file**
```c
/* Linked List using pointer(header) */
#ifndef ___LinkedList
#define ___LinkedList

#include "Member.h"

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

/*--- initialize linked list ---*/
void Initialize(List* list);

/*--- search for a node equal to x with compare function ---*/
Node* search(List* list, const Member* x, int compare(const Member* x, const Member* y));

/*--- insert a node into the head ---*/
void InsertFront(List* list, const Member* x);

/*--- insert a node into the tail ---*/
void InsertRear(List* list, const Member* x);

/*--- remove head node ---*/
void RemoveFront(List* list);

/*--- remove tail node ---*/
void RemoveRear(List* list);

/*--- remove selected node ---*/
void RemoveCurrent(List* list);

/*--- remove all nodes ---*/
void Clear(List* list);

/*--- print data of seleted node ---*/
void PrintCurrent(const List* list);

/*--- print data of all nodes in list order ---*/
void Print(const List* list);

/*--- terminate linked list ---*/
void Terminate(List* list);
#endif
```

<br/>

**Main function**

* Initialize linked list  
    Initialize before use linkde list.  
    Create empty linked list by substitue NULL value for head pointing to the head node.   
    crnt pointer is the currently selected node. It's used for select and delete node searched.  

    <img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139399661-f7199682-7d2d-4359-8cae-6fc598e5677d.png"> 

    ```c
    /*--- initialize linked list ---*/
    void Initialize(List* list)
    {
        list->head = NULL;	/* head node */
        list->crnt = NULL;	/* selected node */
    }
    ```

<br/>

* Create node  
    Create node type object and return the created pointer of object.
    This function only run in this source file when insert node. Therefore make it a static function.
    ```c
    /*--- create node dynamically ---*/
    static Node* AllocNode(void)
    {
        return calloc(1, sizeof(Node));
    }
    ```

<br/>

*  Set a member value of node  
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

<br/>

* Searh node
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

<br/>

* Insert node to head  
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

<br/>

* Insert node to tail  
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

<br/>

* Delete head node  
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

<br/>

* Delete tail node  
    * Processing according to the number of nodes
        1. Node 1 quantity in list
            Same that delete head node so run RemoveFront function.
        2. Node more than two in list
            Decalre pre pointer that node in front of the node currently being scanned. Free memory tail node and pre pointer link to linked list.
            
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
                list->crnt = pre;   /* link to linked list */
            }
        }
    }
    ```       

<br/>

* Delete selected node  
    * Processing according to logic
        1. crnt is head node
            Delete the head node with RemoveFont function.
        2. crnt is not head node
            Find a selected node. If terminate while loop, ptr in front of seleted node to delete. Free memory selected node and update crnt point to node in fornt of delted node(ptr).

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

<br/>

**Linked List source file**
```c
/* Linked List using pointer(source) */
#include <stdio.h>
#include <stdlib.h>
#include "Member.h"
#include "LinkedList.h"

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
		if (compare(&ptr->data, x) == 0) {	/* same the key value */
			list->crnt = ptr;
			return ptr;			/* search sucess */
		}
		ptr = ptr->next;			/* select next node */
	}
	return NULL;					/* search fail */
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
		Node* ptr = list->head->next;		/* pointer about second node */
		free(list->head);			/* free head node */
		list->head = list->crnt = ptr;		/* new head node */
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
			free(ptr);		   /* free tail node */
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
	while (list->head != NULL)		/* until empty */
		RemoveFront(list);		/* delete head node */
	list->crnt = NULL;
}
```

-------

## Linked List using cursor
Linked list using pointer needs to allocate and free memory whenever node insert or delete. This cost is never small. If programmer can know in advance the maximum value of the number of data and the number of data does not change significantly, can operate efficiently using array with index. The index that acts as a pointer is called a cursor.

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139524347-d762ff3a-d204-450f-b17a-fb8f7a775212.png">

Linked list using curosr need to make an array of sufficient size by calculating in advance the maximum value of the number of data.
The cursor value of array is the index containing the next node.
The cursor of tail node value set -1, can not exist as array index.
The value of head have 1, index of array containing head node A because of head that represents head node is cursor. In this way, no need to move data when insert or delete.

* insert into head
<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139524351-9fce8e89-1db9-4c46-bf3c-7b326abd165c.png">

    Update 1 to 3 in head and substitue 1 for node D.

### processing emptied elemnet of array
Using cursor way have a problem when to delete. It occurs emptied memory so need to manage deleted node.

<br/>

**Array Linked List header file**

```c
/* Linked List by cursor(header) */
#ifndef ___ArrayLinkedList
#define ___ArrayLinkedList

#include "Member.h"

#define Null -1					/* empty cursor */

typedef int Index;				/* cursor type */

/*--- Node ---*/
typedef struct {
	Member data;				/* data */
	Index next;					/* next node */
	Index Dnext;				/* next node of free list */
} Node;

/*--- Linked List ---*/
typedef struct {
	Node* n;					/* list(array) */
	Index head;					/* head node */
	Index max;					/* tail recode in use */
	Index deleted;				/* head node of free list */
	Index crnt;					/* selected node */
} List;

/*--- initalize linked list ---*/
void Initialize(List* list, int size);

/*--- search for a node equal to x with compare function ---*/
Index search(List* list, const Member* x,
	int compare(const Member* x, const Member* y));

/*--- insert a node into the head ---*/
void InsertFront(List* list, const Member* x);

/*--- insert a node into the tail ---*/
void InsertRear(List* list, const Member* x);

/*--- remove head node ---*/
void RemoveFront(List* list);

/*--- remove tail node ---*/
void RemoveRear(List* list);

/*--- remove selected node ---*/
void RemoveCurrent(List* list);

/*--- remove all nodes ---*/
void Clear(List* list);

/*--- print data of seleted node ---*/
void PrintCurrent(const List* list);

/*---  print data of all nodes in list order(including \n) ---*/
void PrintLnCurrent(const List* list);

/*--- print data of all nodes in list order ---*/
void Print(const List* list);

/*--- terminate linked list ---*/
void Terminate(List* list);
#endif
```

```c
/* Linked List using pointer(source) */
#include <stdio.h>
#include <stdlib.h>
#include "Member.h"
#include "ArrayLinkedList.h"

/*--- Get the index of the record to be inserted.(index allocate)---*/
static Index GetIndex(List *list)
{
	if (list->deleted == Null)	/* nothing record to delete */
		return ++(list->max);	/* record number of tail node 1 increase */
	else {
		Index rec = list->deleted;	/* substitute record for head node of free list */
		list->deleted = list->n[rec].Dnext; /* subtitiue next node of free list for head node of free list */
		return rec; // index 
	}
}

/*--- register designed record to the delete list  ---*/
static void DeleteIndex(List *list, Index idx)
{
	if (list->deleted == Null) {	/* nothing record to delete */
		list->deleted = idx;	/* parameter idx as head of free list*/
		list->n[idx].Dnext = Null;	/* next node of free list as -1 */
	}
	else {
		Index ptr = list->deleted;	/* head */
		list->deleted = idx;	/* insert deleted node into head of free list */
		list->n[idx].Dnext = ptr;	/* swap head with free list */
	}
}

/*--- set a value each member of node point to by n ---*/
static void SetNode(Node *n, const Member *x, Index next)
{
	n->data = *x;     /* data */
	n->next = next;   /* pointer for next node */
}

/*--- initialize linked list ---*/
void Initialize(List *list, int size)
{
	list->n = calloc(size, sizeof(Node));
	list->head = Null;	/* head node */
	list->crnt = Null;	/* selected node */
	list->max = Null;	/* record number of tail node */
	list->deleted = Null;   /* head of free list */
}

/*--- search for a node equal to x with compare function ---*/
Index search(List *list, const Member *x, int compare(const Member *x, const Member *y))
{
	Index ptr = list->head;	
	while (ptr != Null) {	
		if (compare(&list->n[ptr].data, x) == 0) {
			list->crnt = ptr;
			return ptr;   /* sucess search */
		}
		ptr = list->n[ptr].next;
	}

	return Null;    /* search fail */
}

/*--- insert a node into the head ---*/
void InsertFront(List *list, const Member *x)
{
	Index ptr = list->head;
	list->head = list->crnt = GetIndex(list);	/* create node and insert the node to head*/
	SetNode(&list->n[list->head], x, ptr);
}

/*--- insert a node into the tail ---*/
void InsertRear(List *list, const Member *x)
{
	if (list->head == Null)         /* is empty */
		InsertFront(list, x);   /* insert to head */
	else {
		Index ptr = list->head;
		while (list->n[ptr].next != Null)
			ptr = list->n[ptr].next;
		list->n[ptr].next = list->crnt = GetIndex(list); /* index allocate */
		SetNode(&list->n[list->n[ptr].next], x, Null);
	}
}

/*--- remove head node ---*/
void RemoveFront(List *list)
{
	if (list->head != Null) {
		Index ptr = list->n[list->head].next;
		DeleteIndex(list, list->head);
		list->head = list->crnt = ptr;
	}
}

/*--- remove tail node ---*/
void RemoveRear(List *list)
{
	if (list->head != Null) {
		if (list->n[list->head].next == Null)	/* node just 1 quantity */
			RemoveFront(list);		/* delete head node */
		else {
			Index ptr = list->head;
			Index pre;
			while (list->n[ptr].next != Null) {
				pre = ptr;
				ptr = list->n[ptr].next;
			}
			list->n[pre].next = Null;
			DeleteIndex(list, ptr);
			list->crnt = pre;
		}
	}
}

/*--- remove selected node ---*/
void RemoveCurrent(List *list)
{
	if (list->head != Null) {
		if (list->crnt == list->head)   /* select head node is head */
			RemoveFront(list);      /* delete head node */
		else {
			Index ptr = list->head;
			while (list->n[ptr].next != list->crnt)
				ptr = list->n[ptr].next;
			list->n[ptr].next = list->n[list->crnt].next;
			DeleteIndex(list, list->crnt);
			list->crnt = ptr;
		}
	}
}

/*--- remove all nodes ---*/
void Clear(List *list)
{
	while (list->head != Null)  /* until empty */
		RemoveFront(list);  /* delete head node */
	list->crnt = Null;
}

/*--- print data of seleted node ---*/
void PrintCurrent(const List *list)
{
	if (list->crnt == Null)
		printf("There is no node.");
	else
		PrintMember(&list->n[list->crnt].data);
}

/*---  print data of all nodes in list order(including \n) ---*/
void PrintLnCurrent(const List *list)
{
	PrintCurrent(list);
	putchar('\n');
}

/*--- print data of all nodes in list order ---*/
void Print(const List *list)
{
	if (list->head == Null)
		puts("There are no nodes.");
	else {
		Index ptr = list->head;
		puts("【 View all 】");
		while (ptr != Null) {
			PrintLnMember(&list->n[ptr].data);
			ptr = list->n[ptr].next;    /* select rear node */
		}
	}
}

/*--- terminate linked list ---*/
void Terminate(List *list)
{
	Clear(list);    /* delete all node */
	free(list->n);
}
```
