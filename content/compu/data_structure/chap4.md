---
title: "Chap4. Linked List 2"
description: "Linked List 2"
menuTitle : "Linked List 2"
weight: 4
date: 2021-03-07T14:35:05+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

## 1. 연결 리스트의 개념적인 이해

---

- 배열 :  순차적 접근이 index를 통해서 가능 / 크기가 미리 결정 되어야 하고 변경이 불가능
- 동적할당 : 순차 접근이 불가능 ⇒ 연결 리스트로 관리할 것

```cpp
typedef struct _node
{
		int data; // 데이터를 담을 공간
		struct _node * next; // 연결의 도구! 
} Node;
```

- 노드를 연결
- 다른 노드를 가리키기 위한 포인터 변수 존재

### - 예제 LinkedRead.c의 분석

```cpp
// 초기화
typedef struct _node
{
		int data;
		struct _node * next;
} Node;

int main(void)
{
		Node * head = NULL;
		Node * tail = NULL;
		Node * cur = NULL;

		Node * newNode = NULL;
		int readData;
		. . . .
}

// 삽입 1회전
while(1)
{
		printf("자연수 입력: ");
		scanf("%d", &readData);
		if(readData < 1)
				break;

		// 노드의 추가과정
		newNode = (Node*)malloc(sizeof(Node));
		newNode->data = readData;
		newNode->next = NULL;

		if(head == NULL) // 첫 삽입
				head = newNode;
		else  // 두번째 이후
				tail->next = newNode;

		tail = newNode;
}

// 데이터 조회
if(head == NULL)
{
		printf("저장된 자연수가 존재하지 않습니다. \n");
}
else
{
		cur = head; // 첫번째 노드 가리킴
		printf("%d ", cur->data);
		while(cur->next != NULL)
		{
				cur = cur->next; 
				printf("%d ", cur->data);
		}
}
```

```cpp
// 데이터 삭제
if(head == NULL)
{
		return 0;
}
else
{
		Node * delNode = head;
		Node * delNextNode = head->next;
		printf("%d을 삭제\n", head->data);
		free(delNode);

		while(delNextNode != NULL)
		{
				delNode = delNextNode;
				delNextNode = delNextNode->next;
				printf("%d을 삭제\n", delNode->data);
				free(delNode);
		}
}
```

- head : 1st node
- tail : last note
- cur : 순차 접근 시 현재 위치 참조

- 분기가 적을 수록 버그 발생 위험이 낮아 좋은 코드이다

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap4/1.png)

- 분기 존재
- cur→next를 이용하기 위해서 사라지기 전 백업하기 위해 포인터 변수 2개(dN, dNN) 필요

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap4/2.png)



## 2. 단순 연결 리스트의 ADT와 구현

---

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap4/3.png)

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap4/4.png)

- 새 노드를 머리에 추가하는 경우
  - 장점 : 포인터 변수 tail이 불필요하다.
  - 단점 : 저장된 순서를 유지하지 않는다.
- 새 노드를 꼬리에 추가하는 경우
  - 장점 : 저장된 순서가 유지된다.
  - 단점 : 포인터 변수 tail이 필요하다.

### - SetSortRule 함수 선언에 대한 이해

```cpp
void SetSortRule(List * plist, int (*comp)(LData d1, LData d2) );

-> int (*comp)(LData d1, LData d2) // 반환형이 int이고, LData형 인자를 
                                   // 두 개 전달받는 함수의 주소값을 전달
```

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap4/5.png)

이렇듯 결정된 약속을 근거로 함수가 정의되어야 하며, 연결 리스트 또한 이를 근거로 구현되어야 한다.

### - 더미 노드 기반 연결 리스트

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap4/6.png)

- dummy node : 유효한 데이터가 없는 노드

→ 더미 노드를 처음에 미리 초기화를 시키고 이후의 모든 연산이 처음 노드를 건너뛰고 연산을 진행 할 수 있도록 한다. ⇒ **노드 추가 및 삭제 방식이 항상 일정해짐! 분기 줄어듦**

### - 정렬 기능 추가된 연결 리스트의 구조체

```cpp
typedef struct _linkedList
{
		Node * head;                  // 더미 노드를 가리킴
		Node * cur;                   // 참조 및 삭제 용
		Node * before;                // 삭제를 도움
		int numOfData;                // 저장된 데이터의 수
		int (*comp)(LData d1, LData d2);  // 정렬의 기준을 등록하기 위한 멤버
}

typedef LinkedList List;  // 별도 이름을 주어 헤더파일에서 임의로 간편히 자료형 변환
```

### - 더미 노드 연결 리스트 구현

```cpp
// 초기화
void ListInit(List * plist)
{
		plist->head = (Node*)malloc(sizeof(Node));   // dummy node 생성
		plist->head->next = NULL;
		plist->comp = NULL;
		plist->numOfData = 0;
}
```

```cpp
// 삽입 
void LInsert(List * plist, LData data)
{
	if(plist->comp == NULL)    // 정렬 기준 없으면
		FInsert(plist, data);    // 머리에 노드 추가
	else                       // 정렬 기준 있다면
		SInsert(plist, data);    // 정렬 기준에 따라서 노드 추가
}

void FInsert(List * plist, LData data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	newNode->next = plist->head->next;    // 새 노드가 다른 노드를 가리키게 함
	plist->head->next = newNode;          // 더미 노드가 새 노드를 가리키게 함

	(plist->numOfData)++;
}
```

```cpp
// 참조 1
int LFirst(List * plist, LData * pdata)
{
	if(plist->head->next == NULL)
		return FALSE;

	plist->before = plist->head;
	plist->cur = plist->head->next;

	*pdata = plist->cur->data;
	return TRUE;
}
```

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap4/7.png)

```cpp
// 참조 2
int LNext(List * plist, LData * pdata)
{
	if(plist->cur->next == NULL)
		return FALSE;

	plist->before = plist->cur;
	plist->cur = plist->cur->next;

	*pdata = plist->cur->data;
	return TRUE;
}
```

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap4/8.png)

```cpp
// 삭제
LData LRemove(List * plist)
{
  // 데이터 백업
	Node * rpos = plist->cur;     
	LData rdata = rpos->data;

  // 데이터 이동
	plist->before->next = plist->cur->next;
	plist->cur = plist->before;

  // 데이터 삭제
	free(rpos);
	(plist->numOfData)--;
	return rdata;
}
```

- cur 이전 데이터로 값을 얻을 수 없으므로 before 변수를 통해서 미리 값 저장

## 3. 연결 리스트의 정렬 삽입의 구현

---

- 단순 연결 리스트의 정렬 관련 3 요소

  - 정렬기준이 되는 함수를 등록하는 **SetSortRule** 함수
  - SetSortRule 함수로 전달된 함수정보 저장을 위한 LinkedList의 멤버 **comp [정의기준]**
  - comp에 등록된 정렬기준을 근거로 데이터를 저장하는 **SInsert** 함수

  ⇒“**SetSortRule** 함수가 호출되면서 정렬의 기준이 리스트의 멤버 **comp**에 등록되면, **SInsert** 함수 내에서는 **comp**에 등록된 정렬의 기준을 근거로 데이터를 정렬하여 저장한다.”

  ```cpp
  void SInsert(List * plist, LData data)
  {
  	Node * newNode = (Node*)malloc(sizeof(Node));  // 노드 생성
  	Node * pred = plist->head;         // 더미 노드 가리킴 (A-B 중 A로 B 알수있음
  	newNode->data = data;
  
  	// 새 노드가 들어갈 위치를 찾기 위한 반복문!
  	while(pred->next != NULL && plist->comp(data, pred->next->data) != 0)
  	// 리스트의 끝 && 들어온 데이터의 정렬 순서가 뒤로 가야하는 상황
  	{
  		pred = pred->next; // 다음 노드로 이동
  	}
  
  	// 데이터 추가
  	newNode->next = pred->next;
  	pred->next = newNode;
  	(plist->numOfData)++;
  }
  ```

  ![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap4/9.png)