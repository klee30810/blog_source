---
title: "Chap7. Queue"
description: "Queue"
menuTitle : "Queue"
weight: 7
date: 2021-03-07T14:35:13+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

## 1. 큐의 의해와 ADT 정의

- 큐는 **LIFO(Last-in, First-out)** 구조의 자료구조이다. 때문에 먼저 들어간 것이 먼저 나오는, 일종의 줄서기에 비유할 수 있는 자료구조이다
- 큐에 데이터를 넣는 연산 enqueue &  큐에서 데이터를 꺼내는 연산 dequeue

### ADT 정의

```cpp
void QueueInit(Queue * pq);
// 큐의 초기화 진행, 큐 생성 후 가장 먼저 호출되어야 함

int QIsEmpty(Queue * pq);
// 큐가 빈 경우 TRUE(1)

void Enqueue(Queue * pq, Data data);
// 큐에 data로 전달된 값을 저장

Data Dequeue(Queue * pq);
// 저장 순서가 앞선 데이터를 반환하고 삭제한다.
// 데이터가 하나 이상 존재함이 보장되어야 한다.

Data Qpeek(Queue * pq);
// 저장 순서가 가장 앞선 데이터를 반환하되 삭제하지 않는다.
// 데이터가 하나 이상 존재함이 보장되어야 한다.
```

## 2. 큐의 배열 기반 구현

---

- enqueue : 큐의 꼬리를 의미하는 R을 한 칸 이동시키고 새 데이터를 저장한다.
- dequeue : 큐의 머리를 의미하는 F가 가리키는 데이터를 반홖하고 F를 한 칸 이동시킨다.

### 문제점

- 배열의 끝에서 enqueue의 경우, R을 오른쪽으로 한칸 이동시킬 수 없어서 더 이상 데이터를 추가할 수 없다 ⇒ R을 인덱스 0의 위치로 이동시키는 **원형 큐** 활용

### 원형 큐

- enqueue : 단순 배열 큐와 마찬가지로 R이 이동한 다음에 데이터 저장
- dequeue : 단순 배열 큐와 마찬가지로 F가 가리키는 데이터 반환 후 F 이동

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap7/1.png)

→ F & R 위치관계를 통해 채워졌는지, 비워졌는지 구분이 불가능 하다.

⇒ 데이터 하나를 쓰지 않는다. 

- 데이터가 하나 존재하는 경우 F와 R이 같은 위치를 가리켰는데, 초기화 직후에 이 상태가 되게 한다.
- 그냥 하나 비운 상태에서 FULL로 인정한다! 그러면 Empty와 Full의 구분이 가능하다!
- **R=F : empty      /         R+1=F    ⇒ 구분 가능!**

- Helper Function

```cpp
int NextPosIdx(int pos)
{
	if(pos == QUE_LEN-1)
		return 0;
	else
		return pos+1;
}
// F, R이 이동시 이동해야 할 위치를 알려줌
```

```cpp
void QueueInit(Queue * pq)
{
	pq->front = 0;
	pq->rear = 0;
}

int QIsEmpty(Queue * pq)
{
	if(pq->front == pq->rear)
		return TRUE;
	else
		return FALSE;
}
```

```cpp
int NextPosIdx(int pos)
{
	if(pos == QUE_LEN-1)
		return 0;
	else
		return pos+1;
}
// rear 이동 후 그 위치에 데이터 저장
void Enqueue(Queue * pq, Data data)
{
	if(NextPosIdx(pq->rear) == pq->front)  // 꽉찬지 확인
	{
		printf("Queue Memory Error!");
		exit(-1);
	}

	pq->rear = NextPosIdx(pq->rear);     // 증가
	pq->queArr[pq->rear] = data;
}

// front 이동시키고 그 위치 데이터 반환
Data Dequeue(Queue * pq)
{
	if(QIsEmpty(pq))
	{
		printf("Queue Memory Error!");
		exit(-1);
	}

	pq->front = NextPosIdx(pq->front);
	return pq->queArr[pq->front];
}
```

## 3. 큐의 연결 리스트 기반 구현

---

- 스택과 큐의 유일한 차이점이 앞에서 꺼내느냐 뒤에서 꺼내느냐에 있으니, 이젂에 구현해 놓은 스택을 대상으로 꺼내는 방법만 조금 변경하면 큐가 될 것 같다.

```cpp
void QueueInit(Queue * pq)
{
	pq->front = NULL;
	pq->rear = NULL;
}

int QIsEmpty(Queue * pq)
{
	if(pq->front == NULL)
		return TRUE;
	else
		return FALSE;
}
```

```cpp
void Enqueue(Queue * pq, Data data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));
	newNode->next = NULL;
	newNode->data = data;

	if(QIsEmpty(pq)) // 첫 노드 : F & R 둘다 변경
	{
		pq->front = newNode;
		pq->rear = newNode;
	}
	else    // 이후 노드 : R 만 변경
	{
		pq->rear->next = newNode;     // 바꾸고
		pq->rear = newNode;            // 가리킴
	}
}
```

```cpp
Data Dequeue(Queue * pq)
{
	Node * delNode;
	Data retData;

	if(QIsEmpty(pq))
	{
		printf("Queue Memory Error!");
		exit(-1);
	}

	delNode = pq->front;           // 백업
	retData = delNode->data;
	pq->front = pq->front->next;   // F 노드의 이동

	free(delNode);                 // 삭제
	return retData;
}
```

## 4. 덱(Deque)의 이해와 구현

---

- Double ended queue : 양쪽 방향으로 모두 입출력이 가능 & 스택과 큐의 형태를 모두 사용할 수 있음