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

* A-n,mfter data inserting
A person with a mem_no 44 was inserted between men_no 22 and 66.  

    |0|1|2|3|4|5|
    |:-:|:-:|:-:|:-:|:-:|:-:|
    |11|22|<span style="color:blue">44</span>|<span style="color:red">66</span>|<span style="color:red">77</span>|0|

<br/>

**Problem of linear list**

1. The efficiency is not good because all data must <span style="color:red">move</span> according to data <span style="color:blue">insertion and deletion</span>.
2. Need to know the size of the accumulated data in advance.

---

## Pointer linked list
Create node type object when insert to data for linked list. Each node's pointer to the next node. The first node of linked list is the head node and last node is tail node. The tail node has null value.

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

 **If delete node object when delete data, solve the problem that push and pull data.**

<img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139426745-cab9589e-5070-4037-aa9f-2c399b3da2c6.png"> 


### Struct list mange a liked list 
List manage a liked list, consist of two member and have point data type for node.

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
* Create node  
    Create node type object and return the created pointer of object.

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
    Substitute x point to vlaue for n point to node type object.  
    Substitute passed to next as parameter for next of n.  

    ```c
    /*--- set a value each member of node point to by n ---*/
    static void SetNode(Node* n, const Member* x, const Node* next)
    {
        n->data = *x;		/* data */
        n->next = next;		/* pointer for next node */
    }
    ```

<br/>

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

* Searh  
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
            ptr = ptr->next;	/* select next node */
        }
        return NULL;		/* search fail */
    }
    ```

<br/>

* Insert node to head  
    Update head node also crnt to point to the newly created node.  
    Set a value by calling SetNode function.  
    
    <img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139411165-900bedf4-ebcd-40dc-85ec-e700c45add9b.png">
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
    * processing according to logic
        1. List is empty
            Same do that insert node to head so do as InserFront function.
        2. List isn't empty
            Insert newly node to tail.
    
    <img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139427588-6854e975-cdf7-45ca-8d4b-7202d5353fa7.png">

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
    Update ptr pointer to second node and free memmory for head.  
    Set head to ptr pointer and crnt pointer after free.  

     <img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139428395-ab26bef6-b5ad-4b8e-a2db-7efa4cacd8de.png">

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

     <img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139428431-0b432078-587f-46e1-918b-d7fa0556fa97.png">

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
    * processing according to logic
        1. crnt is head node
            Delete the head node with RemoveFont function.
        2. crnt is not head node
            Find a selected node. If terminate while loop, ptr in front of seleted node to delete. Free memory selected node and ptr pointer link to linked list.

    <img width="800" alt="computer_inside" src="https://user-images.githubusercontent.com/92430498/139428439-60913c8f-646c-4b2c-9a9d-8d4e348d2670.png">

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
                list->crnt = ptr; /* ptr link to linked list */
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
			list->crnt = ptr; /* ptr link to linked list */
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

/*--- print data of seleted node ---*/
void PrintCurrent(const List* list)
{
	if (list->crnt == NULL)
		printf("There is no node.");
	else
		PrintMember(&list->crnt->data);
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
			ptr = ptr->next;	/* select rear node */
		}
	}
}

/*--- terminate linked list ---*/
void Terminate(List* list)
{
	Clear(list);	v/* delete all node */
}
```