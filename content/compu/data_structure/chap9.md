---
title: "Chap9. Priority Queue & Heap"
description: "Priority Queue & Heap"
menuTitle : "Priority Queue & Heap"
weight: 9
date: 2021-03-07T14:35:19+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

### 1. 우선순위 큐의 이해

---

- data의 우선순위 결정 → 들어가는 순서와 상관없이 우선순위 근거로 연산 진행 ⇒ 우선순위는 프로그래머가 직접 결정

##### 구현 방법

1) 배열 기반 구현 : 우선순위 기준을 배열에 추가 →  data가 많아질 경우 배열이 길어지기 때문에 성능 악화

2) 연결리스트 기반 구현 : 원하는 값을 찾기 위해 포인터 이동 연산을 ㅁ많이 해야 함 → 성능 악화

3) 힙을 이용하는 방법

##### 힙

- 완전 이진트리의 일종 → 데이터를 넣고 빼기 쉬워서 성능 문제가 없다.
- 모든 노드에 저장된 값은 자식 노드에 저장된 값보다 크거나 같아야 한다. → 부모 노드 보다만 작으면 된다 ⇒ 루트 노드에 저장된 값이 가장 커야 한다.

### 2. 힙의 구현과 우선순위 큐의 완성

---

- 우선순위는 값이 낮을수록 높다고 가정

- 힙에서 데이터 저장 과정

  - 자식 노드 데이터의 우선순위 \\(\leq\\) 부모 노드 데이터의 우선순위
  - 새 데이터는 우선순위가 낮다는 가정하에 끝에 저장
  - 부모 노드와 비교 진행
  - 자리바꿈
  - 반복

- 힙에서 데이터 삭제 과정

  - 루트 노드 삭제
  - 마지막 노드를 루트 노드로 이동
  - 자식 노드와 비교 후 이동 → 우선순위가 높은 노드만 비교하면 됨
  - 반복 후 자리 확정

- 성능 평가 (삽입 & 삭제)

  1) 배열기반 : O(n), O(1) → 삽입 마다 우선순위 비교 필요

  2) 연결리스트 기반 : O(n), O(1) → 우선순위 비교시 포인터 연산 필요

  3) 힙 기반 : \\(O(\log_2n), O(\log_2n)\\)→ 총 연산이 이진법 깊이와 비슷 → 깊이가 비교 연산의 횟수가 됨

- 연결리스트를 기반으로 힙을 구현하면 새로운 노드를 힙의 마지막 위치에 추가하는 것이 쉽지 않다 (계속 이동해야 하므로)

> 왼쪽 자식 노드의 인덱스 = 부모 노드 인덱스 * 2
>
> 오른쪽 자식 노드 인덱스 = 부모 노드 인덱스 * 2 + 1
>
> 부모 노드 인덱스 = 자식 노드 인덱스 / 2

##### 원리 이해 중심의 힙 구현

```c
typedef char HData;
typedef int Priority;

typedef struct _heapElem
{
  Priority pr; // 값이 작을 수록 높은 우선순위
  HData data;
} HeapElem;   // data와 우선순위 각각 구분

typedef struct _heap
{
  int numOfData;
  HeapElem heapArr[HEAP_LEN];
} Heap;

void HeapInit(Heap * ph);
int HIsEmpty(Heap * ph);

void HInsert(Heap * ph, HData data, Priority pr);
HData HDelete(Heap * ph); // 우선순위 높은 데이터 삭제
```

- 힙은 완전 이진 트리이다.
- 힙은 배열기반으로 구현하며 인덱스 0은 비워둔다.
- **힙에 저장된 노드의 개수와 마지막 노드의 고유번호는 일치한다.**
- 노드의 고유 번호가 저장되는 배열의 인덱스 값이 된다.
- 우선순위를 나타내는 정수 값이 작을수록 높은 우선순위를 나타낸다고 가정한다.

→ 배열을 기반으로 하는 경우 힙에 저장된 노드의 개수와 마지막 노드의 고유번호가 일치하기 때문에 마지막 노드의 인덱스 값을 쉽게 얻을 수 있다.

```c
void HeapInit(Heap * ph)
{
  ph->numOfData = 0;
}

int HIsEmpty(Heap * ph)
{
  if(ph->numOfData == 0)
    return TRUE;
  else
    return FALSE;
}

int GetParentIDX(int idx)
{
  return idx/2;
}

int GetLChildIDX(int idx)
{
  return idx*2;
}

int GetRChildIDX(int idx)
{
  return GetLChildIDX(idx) + 1;
}
```

```c
// 우선 순위가 높은 자식의 인덱스 값 반환
int GetHiPriChildIDX(Heap * ph, int idx)
{
  // 자식 노드가 없다면
  // numOfData는 마지막 노드의 고유번호라서, 자식 노드의 값이 이보다 크면 존재하지 않는 자식 노드이다.
  if(GetLChildIDX(idx) > ph->numOfData)
    return 0;
  
  // 자식 노드가 왼쪽 자식 노드 하나만 존재한다면,
  // 자식 노드가 하나 존재하면 왼쪽 자식노드이다 => 완전 이진 트리
  else if(GetLChildIDX(idx) == ph->numOfData)
    return GetLChildIDX(idx);
  
  // 자식 노드가 둘 다 존재한다면,
  else
  {
    // 오른쪽 자식 노드의 우선순위가 높다면,
    if(ph->heapArr[GetLChildIDX(idx)].pr > ph->heapArr[GetRChildIDX(idx)].pr)
      return GetRChildIDX(idx); // 오른쪽 자식 노드의 인덱스 값 반환
    
    // 왼쪽 자식 노드의 우선순위가 높다면,
    else
      return GetLChildIDX(idx); // 왼쪽 자식 노드 인덱스 값 반환
  }
}
```

```c
// 굳이 루트 노드의 자리로 옮기지 않아도 된다.
HData HDelete(Heap * ph)
{
  HData retData = (ph->heapArr[1]).data; // 삭제할 데이터 저장
  HeapElem lastElem = ph->heapArr[ph->numOfData]; // 힙의 마지막 노드 저장
  
  // parentIdx 에는 마지막 노드가 저장 될 위치 정보가 담긴다
  int parentIdx = 1; // 루트 노드가 위치해야 할 인덱스 값 저장
  int childIdx;
  
  // 루트 노드의 우선순위가 높은 자식 노드를 시작으로 반복문 시작
  while(childIdx = GetHiPriChildIDX(ph, parentIdx))
  {
    if(lastElem.pr <= ph->heapArr[childIdx].pr) // 마지막 노드와 우선순위 비교
      break;   // 마지막 노드의 우선순위가 높으면 반복문 탈출
    
    // 마지막 노드보다 우선순위 높으니, 비교대상 노드의 위치를 한 레벨 올림
    ph->heapArr[parentIdx] = ph->heapArr[childIdx];
    parentIdx = childIdx;  // 마지막 노드가 저장될 위치 정보를 한 레벨 내림
  }  // 반복문 탈출시 parentIdx에는 마지막 노드의 위치정보가 저장됨
  
  ph->heapArr[parentIdx] = lastElem; // 마지막 노드 최종저장
  ph->numOfData -= 1;  // 삭제 했으므로
  
  return retData; // 삭제 데이터 반환
}
```

```c
// 마지막 노드에 저장했다고 가정하고 임시저장 & idx 유지
void HInsert(Heap * ph, HData data, Priority pr)
{
  int idx = ph->numOfData + 1;  // 새 노드가 저장될 인덱스 값을 idx에 저장
  HeapElem nelem = {pr, data};  // 새 노드의 생성 및 초기화
  
  // 새 노드가 저장될 위치가 루트 노드의 위치가 아니라면 while문 반복
  while(idx != 1)
  {
    // 새 노드와 부모 노드의 우선순위 비교
    if(pr < (ph->heapArr[GetParentIDX(idx)].pr)) // 새 노드의 우선순위가 높다면
    {
      // 부모 노드를 한 레벨 내림, 실제로 내림
      ph->heapArr[idx] = ph->heapArr[GetParentIDX(idx)];
      
      // 새 노드를 한 레벨 올림, 실제로 올리지는 않고 인덱스 값만 갱신
      idx = GetParentIDX(idx);
    }
    else    // 새 노드의 우선순위가 높지 않다면 
      break;
  }
  ph->heapArr[idx] = nelem; // 새 노드를 배열에 저장
  ph->numOfData += 1;
}
```

```c
int main()
{
  Heap heap;
  HeapInit(&heap);
  
  HInsert(&heap, 'A', 1);
  HInsert(&heap, 'B', 2);
  HInsert(&heap, 'C', 3);
  printf("%c \n", HDelete(&heap));
  
  HInsert(&heap, 'A', 1);
  HInsert(&heap, 'B', 2);
  HInsert(&heap, 'C', 3);
  printf("%c \n", HDelete(&heap));
  
  while(!HIsEmpty(&heap))
    printf("%c \n", HDelete(&heap));
  
  return 0;
}
```

⇒ 프로그래머가 우선순위를 직접 정해야 함 : 일반적으로 우선순위 정보를 별도로 프로그래머가 전달하는 것은 적합하지 않다. **데이터를 근거로 데이터의 우선순위가 정해지는 것이 바람직하다.**



##### 쓸만한 수준의 힙 변경

```c
/* 우선순위를 별도로 저장하지 않고 함수의 호출 결과로 판단하기 할 것 
 구조체 자료형 HeapElem : 우선순위 정보를 묶기 위함 -> 사용자가 직접 지정해줘야 하기에 직접 사용하기에는 무리가 있음
*/

typedef int PriorityComp(HData d1, HData d2);
// * 없음 : "PriorityComp형 포인터 면수 선언시 반드시 *를 붙여라"

typedef struct _heap
{
  PriorityComp * comp;
  int numOfData;
  HData heapArr[HEAP_LEN];
} Heap;

// 구조체 변경에 따른 초기화 함수 변경
// 프로그래머가 힙의 우선순위 판단 기준을 설정할 수 있어야 함
void HeapInit(Heap * ph, PriorityComp pc)
{
  ph->numOfData = 0;
  ph->comp = pc;
}
```

- PriorityComp 형 함수 정의 기준 (임의 가능)
  - 첫 번째 인자의 우선순위가 높다면 0보다 큰 값 반환
  - 두 번째 인자의 우선순위가 높다면 0보다 작은 값 반환
  - 두 인자의 우선순위가 같다면 0 반환

```c
int GetHiPriChildIDX(Heap * ph, int idx)
{
  if(GetLChildIDX(idx) > ph->numOfData)
    return 0;
  
  else if (GetLChildIDX(idx) == ph->numOfData)
    return GetLChildIDX(idx);
  
  else
  {
    // if(ph->heapArr[GetLChildIDX(idx)].pr > ph->heapArr[GetRChildIDX(idx)].pr) 
    if(ph->comp(ph->heapArr[GetLChildIDX(idx)], ph->heapArr[GetRChildIDX(idx)]) < 0) // comp에 등록된 함수의 호출결과로 우선순위를 판단한다.
      return GetRChildIDX(idx);
    else
      return GetLChildIDX(idx);
  }
}

void HInsert(Heap * ph, HData data)
{
  int idx = ph->numOfData+1;
  
  while(idx != 1)
  {
    // if(pr < (ph->heapArr[GetParentIDX(idx)].pr))
    if(ph->comp(data, ph->heapArr[GetParentIDX(idx)]) > 0)
    {
      ph->heapArr[idx] = ph->heapArr[GetParentIDX(idx)];
      idx = GetParentIDX(idx);
    }
    else
    {
      break;
    }
  }
  
  ph->heapArr[idx] = data;
  ph->numOfData += 1;
}

HData HDelete(Heap * ph)
{
  HData retData = ph->heapArr[1];
  HData lastElem = ph->heapArr[ph->numOfData];
  
  int parentIdx = 1;
  int childIdx;
  
  while(childIdx = GetHiPriChildIDX(ph, parentIdx))
  {
    // if(lastElem.pr <= ph->heapArr[childIdx].pr)
    if(ph->comp(lastElem, ph->heapArr[childIdx]) >= 0) // comp함수가 판단 기준
      break;
    
    ph->heapArr[parentIdx] = ph->heapArr[childIdx];
    parentIdx = childIdx;
  }
  
  ph->heapArr[parentIdx] = lastElem;
  ph->numOfData -= 1;
  
  return retData;
}

int DataPriorityComp(char ch1, char ch2)
{
  return ch2 - ch1; // 내림차순
  // return ch1 - ch2
} // 아스키 코드 값이 작은 문자의 우선순위가 더 높다.

int main()
{
  Heap heap;
  HeapInit(&heap, DataPriorityComp); // 우선순위 판단기준 등록
  
  HInsert(&heap, 'A', 1);
  HInsert(&heap, 'B', 2);
  HInsert(&heap, 'C', 3);
  printf("%c \n", HDelete(&heap));
  
  HInsert(&heap, 'A', 1);
  HInsert(&heap, 'B', 2);
  HInsert(&heap, 'C', 3);
  printf("%c \n", HDelete(&heap));
  
  while(!HIsEmpty(&heap))
    printf("%c \n", HDelete(&heap));
  
  return 0;
}
```

##### 쓸만한 힙을 이용한 우선순위 큐의 구현

```c
#include "UsefulHeap.h"

typedef Heap PQueue;
typedef HData PQData;

void PQueueInit(PQueue * ppq, PriorityComp pc);
int PQIsEmpty(PQueue * ppq);

void PEnqueue(PQueue * ppq, PQData data);
PQData PDequeue(PQueue * ppq);
```

```c
void PQueueInit(PQueue * ppq, PriorityComp pc)
{
  HeapInit(ppq, pc);
}

int PQIsEmpty(PQueue * ppq)
{
  return HIsEmpty(ppq);
}

void PEnqueue(PQueue * ppq, PQData data)
{
  HInsert(ppq, data);
}

PQData PDequeue(PQueue * ppq)
{
  return HDelete(ppq);
}
```













