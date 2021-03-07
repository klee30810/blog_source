---
title: "Chap5. Linked List 3"
description: "Linked List 3"
menuTitle : "Linked List 3"
weight: 5
date: 2021-03-07T14:35:08+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

## 1. 원형 연결 리스트

---

- 단순 연결 리스트의 마지막 노드는 NULL을 가리키지만, 원형 연결 리스트의 마지막 노드는 첫 번째 노드를 가리킨다.
- 모든 노드가 원의 형태를 이루면서 연결되어 있기 때문에 원형 연결 리스트 에서는 사실상 **머리와 꼬리의 구분이 없다.**

### 1. 원형 연결 리스트의 대표적인 장점

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap5/1.png)

- **단순 연결 리스트처럼 머리와 꼬리를 가리키는 포인터 변수를 각각 두지 않아도, 하나의 포인터 변수만 있어도 머리 또는 꼬리에 노드를 간단히 추가할 수 있다.”**
- 앞서 소개한 모델을 기반으로는 위의 장점을 살리기 어렵다. 그래서 우리는 변형된, 그러나 보다 일반적이라고 인식이 되고 있는 **변형된 원형 연결 리스트**를 구현한다
- 꼬리를 가리키는 포인터 변수는 tail이고, 머리를 가리키는 포인터 변수는 tail→next이다. 리스트의 꼬리와 머리 주소값을 쉽게 알 수 있으므로 **포인터 변수는 하나만 있으면 된다.**

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap5/2.png)

```cpp
typedef int Data

typedef struct _node
{
	Data data;
	struct _node * next;
} Node;

typedef struct _CLL
{
	Node * tail;
	Node * cur;
	Node * before;
	int numofData;
} CList;
```

```cpp
// 모든 멤버를 NULL & 0으로 초기화
void ListInit(List * plist)
{
	plist->tail = NULL;
	plist->cur = NULL;
	plist->before = NULL;
	plist->numOfData = 0;
}
```

### 2. 원형 연결 리스트 구현: 첫 번째 노드 삽입

```cpp
// LInsert(꼬리 삽입) & LInsertFront(머리 삽입) 공통부분
void LInser~(List * plist, Data data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	// 첫 노드는 그 자체가 머리이자 꼬리이므로 뒤이던 앞이던 결과는 동일
	if(plist->tail==NULL) // 첫번째 노드
	{
		plist->tail = newNode; 
		newNode->next = newNode;
	}
	else
	{
		...
	}

	(plist->numOfData)++;
}
```

### 3. 원형 연결 리스트 구현: 두 번째 이후 노드 머리로 삽입

```cpp
// 노드 머리 삽입
void LInsertFront(List * plist, Data data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	if(plist->tail == NULL) 
	{
		plist->tail = newNode;
		newNode->next = newNode;
	}
	else // 두번째 이후 노드 머리 추가
	{
		newNode->next = plist->tail->next;
		plist->tail->next = newNode;
	}

	(plist->numOfData)++;
}
```

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap5/3.png)

→ tail이 가리키는 노드가 차이가 난다!

```cpp
// 노드 꼬리 삽입
void LInsert(List * plist, Data data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	if(plist->tail == NULL) 
	{
		...
	}
	else 
	{
		newNode->next = plist->tail->next;
		plist->tail->next = newNode;
			plist->tail = newNode;// 가리키는 노드의 조정 => 유일한 차이
	}

	(plist->numOfData)++;
}
```

### 4. 조회 LFirst & LNext

```cpp
int LFirst(List * plist, Data * pdata)
{
	if(plist->tail == NULL)    // 저장된 노드가 없다면
		return FALSE;

	plist->before = plist->tail;   // 삭제를 위한 백업
	plist->cur = plist->tail->next;

	*pdata = plist->cur->data;
	return TRUE;
}

int LNext(List * plist, Data * pdata)
{
	if(plist->tail == NULL)    // 저장된 노드가 없다면
		return FALSE;

	plist->before = plist->cur;
	plist->cur = plist->cur->next;

	*pdata = plist->cur->data;
	return TRUE;
}
```

- 원형 연결 리스트 이므로 리스트의 끝을 검사하는 코드가 없다!

### 5. 노드의 삭제

(복습)

- 핵심연산 1. 삭제할 노드의 이전 노드가, 삭제할 노드의 다음 노드를 가리키게 한다.
- 핵심연산 2. 포인터 변수 cur을 한 칸 뒤로 이동시킨다

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap5/4.png)

```cpp
LData LRemove(List * plist) // 더미 기반 단순 연결 리스트의 삭제
{
	Node * rpos = plist->cur;
	LData rdata = rpos->data;

	plist->before->next = plist->cur->next;  // 연산 1
	plist->cur = plist->before;    //  연산 2

	free(rpos);
	(plist->numOfData)--;
	return rdata;
}
```

- 원형 연결 리스트 노드 삭제 구현

1. 예외 1 : 삭제할 노드를 tail이 가리키는 경우
2. 예외 2 : 삭제할 노드를 tail이 가리키는데, 그 노드가 마지막 노드라면

```cpp
Data LRemove(List * plist)
{
	Node * rpos = plist->cur;
	Data rdata = rpos->data;

	if(rpos == plist->tail)    // 삭제 대상을 tail이 가리킨다면
	{
		if(plist->tail == plist->tail->next)    // 그리고 마지막 남은 노드라면
			plist->tail = NULL;
		else
			plist->tail = plist->before;
	}

	plist->before->next = plist->cur->next;   // 단순 연결과 동일
	plist->cur = plist->before;

	free(rpos);
	(plist->numOfData)--;
	return rdata;
}
```

## 2. 양방향 연결 리스트

---

- 양방향으로 연결하는 이유
  - 단순 연결 리스트와 같이 before를 유지할 필요가 없다
  - 이전 노드로의 이동이 용이하며, 양방향인 것이 더 복잡한 것은 아니다

```cpp
typedef int Data;

typedef struct _node
{
	Data data;
	struct _node * next;
	struct _node * prev;
} Node;

typedef struct _dbLinkedList
{
	Node * head;
	Node * cur;
	int numOfData;
} DBLinkedList;
```

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap5/5.png)

- 더미가 없으므로 첫 노드와 두 번째 노드 삽입 과정이 달라짐

```cpp
void ListInit(List * plist)
{
	// cur은 조회과정 중 초기화 되므로 초기화 필요 X
	plist->head = NULL;
	plist->numOfData = 0;
}

// 노드 삽입
void LInsert(List * plist, Data data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	newNode->next = plist->head; // 새 노드와 기존 노드 연결
	if(plist->head != NULL)  // 두 번째 노드 이후
		plist->head->prev = newNode;

	newNode->prev = NULL;
	plist->head = newNode;

	(plist->numOfData)++;
}
```

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap5/6.png)

```cpp
int LFirst(List * plist, Data * pdata)
{
	if(plist->head == NULL)
		return FALSE;

	plist->cur = plist->head;
	*pdata = plist->cur->data;

	return TRUE;
}

int LNext(List * plist, Data * pdata)
{
	if(plist->cur->next == NULL)
		return FALSE;

	plist->cur = plist->cur->next;
	*pdata = plist->cur->data;

	return TRUE;
}

int LPrevious(List * plist, Data * pdata)
{
	if(plist->cur->prev == NULL)
		return FALSE;

	plist->cur = plist->cur->prev;
	*pdata = plist->cur->data;

	return TRUE;
}
```