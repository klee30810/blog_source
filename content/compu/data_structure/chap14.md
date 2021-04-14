---
title: "Chap14. Graph "
description: "Graph"
menuTitle : "Graph"
weight: 14
date: 2021-03-07T14:35:29+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structures"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

### 14.1 그래프 이해와 종류

---

- 무방향 완전 그래프 : 연락의 방향성이 없음 → 안전한 구조 (둘 다 연락 시 백업이 되므로)
- 방향 완전 그래프 : 위험성 존재
- 부분 그래프 : 일부 정점과 간선으로 구성이 된 그래프

##### 그래프의 집합 표현

- 정점 집합 : {A, B, C, D}
- 간선 집합 : {(A,B), (A,D), (C,D)} → 무방향 / 방향 있을 경우 <>

##### 그래프의 ADT

- 그래프 최종목적 : 특정 정보를 그대로 담을 수 있도록 표현

```c
enum {A, B, C, D, E, F}	// 정점 이름 선언

void GraphInit(UALGraph * pg, int nv);
// 그래프 초기화, 두번째 인자로 정점의 수 전달

void GraphDestory(UALGraph * pg);
// 그래프 초기화 과정 중, 할당한 리소스 반환

void AddEdge(UALGraph * pg, int fromV, int toV);
// fromV와 toV로 전달된 정점을 연결하는 간선을 그래프에 추가

void ShowGraphEdgeInfo(UALGraph * pg);
// 그래프 간선정보 출력
```



##### 그래프를 구현하는 두 가지 방법 

- 인접 행렬 기반 : 배열 선언 후 간선 정보 기록

- 인접 리스트 기반 : A → B → C → D 와 같은 방식

  → B, C 가 연결된 것이 아니라 A와 연결된 정점이 B,C,D이다.

  → 무방향일 경우 반대 방향도 연결



### 14.2 인접 리스트 기반의 그래프 구현

---

##### 그래프 헤더파일

```c
// 연결 리스트 사용
#include "DLinkedList.h"

// 정점 상수화
enum {A, B, C, D, E, F, G, H, I, J}

typedef struct _ual
{
  int numV;			// 정점 수
  int numE;			// 간선 수
  List * adjList;		// 간선 정보
} ALGraph;

void GraphInit(ALGraph * pg, int nv);
void GraphDestroy(ALGraph * pg);
void AddEdge(ALGraph * pg, int fromV, int toV);
void ShowGraphEdgeInfo(ALGraph * pg);
```



```c
int main()
{
  ALGraph graph;
  GraphInit(&graph, 5);
  
  AddEdge(&graph, A, B);
  AddEdge(&graph, A, B);
  AddEdge(&graph, A, B);
  AddEdge(&graph, A, B);
  AddEdge(&graph, A, B);
  AddEdge(&graph, A, B);
  
  ShowGraphEdgeInfo(&graph);
  GraphDestory(&graph);
  
  return 0;
}
```



```c
int WhoIsPrecede(int data1, int data2)
{
  if(data1<data2)
    return 0;
  else
    return 1;
}

// 그래프 초기화
void GraphInit(ALGraph * pg, int nv)
{
  int i;
  
  // 정점의 수에 해당하는 길이의 리스트 배열 생성
  pg->adjList = (List*)malloc(sizeof(List)*nv);	// 간선 정보 저장할 리스트 생성
  
  pg->numV = nv;		// 정점 수는 nv에 저장된 값으로 결정
  pg->numE = 0;			// 초기 간선 수 0개
  
  // 정점 수 만큼 생성된 리스트 초기화
  for(i=0; i<nv; i++)
  {
    ListInit(&(pg->adjList[i]));
    SetSortRule(&(pg->adjList[i]), WhoIsPrecede);
    // 그래프와 연관성이 없지만 연결 리스트가 요구하므로 함수 등록
  }
}

// 그래프 리소스 해제
void GraphDestory(ALGraph * pg)
{
  if(pg->adjList != NULL)
    free(pg->adjList);		// 동적 연결 리스트 소멸
}

// 간선 추가 - 무방향 그래프
void AddEdge(ALGraph * pg, int fromV, int toV)
{
  LInsert(&(pg->adjList[fromV]), toV);
  LInsert(&(pg->adjList[toV]), fromV);
  
  pg->numE += 1;
}

// 간선 정보 출력
void ShowGraphEdgeInfo(ALGraph * pg)
{
  int i, vx;
  
  for(i=0; i<pg->numV; i++)
  {
    printf("%c와 연결된 정점 : ", i+65);
    
    if(LFirst(&(pg->adjList[i]), &vx))
    {
      printf("%c ", vx+65);
      while(LNext(&(pg->adjList[i]), &vx))
        printf("%c ", vx+65);
    }
    printf("\n");
  }
}
```



### 14.3 그래프의 탐색

---

- 그래프는 정형회된 틀이 아니므로 내용에 따라 형태가 어떻게 바뀔지 모름 → 탐색이 어려움

##### 깊이 우선 탐색 - Depth First Search

- 한명씩 깊이로 가다보면 연결되니 결국 다 연결되지 않을까?
- 한명씩 깊이로 연락을 취해감
- 연락할 곳이 없으면 역으로 되돌아가면서 연락 취할 곳을 찾는다
- 시작점으로 되돌아 오면 연락 끝
- 시작점에서 다른 노드가 있다면 → 내 주변 연락가능지점을 다시 찾고 그사람으로 부터 다시 돌아옴

##### 너비 우선 탐색 - Breath First Search

- 내 주변 직접 연결된 모든 정점에 연락



##### DFS 구현

- 방문 정보 기록의 목적으로 **배열** , 경로 정보 추적의 목적으로 **스택**을 이용
- 다른 정점으로 이동될 때 이전 정점의 정보가 스택으로 이동한다.
- 이미 주변 정점이 모두 연락을 받았을 경우(연락 할 사람이 없는 경우), *되돌아갈 정보를 스택에서 하나 얻는다.*
  - 이전의 방문한 정점에 가더라도 스택에 정점 정보는 저장된다
- 스택에 저장된 정보를 마지막까지 꺼내어 역으로 그 경로를 추적하다 보면 시작위치로 이동 가능하다.

```c
// ALGraphDFS.h
enum {A,B,C,D,E,F,G,H,I,J}

typedef struct _ual
{
  int numV;		// 정점 수
  int numE;		// 간선 수
  List * adjList;		// 간선 정보
  int * visitInfo;	// 탐색 과정에서 탐색이 진행된 정점 정보를 담기 위한 멤버 추가
} ALGraph;

// graph initialization
void GraphInit(ALGraph * pg, int nv);

// graph resource cancel
void GraphDestory(ALGraph * pg);

// add edge
void AddEdge(ALGraph * pg, int fromV, int toV);

// edge info print
void ShowGraphEdgeInfo(ALGraph * pg);

// Show Vertex info based on DFS
void DFShowGraphVertex(ALGraph * pg, int startV);

// visitInfo related added codes
void GraphInit(ALGraph * pg, int nv)
{
  ....
  // 정점의 수를 길이로 하여 배열을 할당
  pg->visitInfo = (int*)malloc(sizeof(int) * pg -> numV);
  
  // 배열 모든 요소를 0으로 초기화
  memset(pg->visitInfo, 0, sizeof(int)*pg->numV);
}

void GraphDestroy(ALGraph * pg)
{
  ....
   // allocated array destory
  if(pg->visitInfo != NULL)
    free(pg->visitInfo);
}
```



```c
// 방문한 정점의 정보를 그래프 멤버 visitInfo가 가리키는 배열에 등록하는 기능 제공
int VisitVertex(ALGraph * pg, int visitV)
{
  if(pg->visitInfo[visitV] == 0)		// 처음 방문
  {
    pg->visitInfo[visitV] = 1;	// visitV에 방문 기록
    printf("%c ", visitV + 65); // 정점 이름 출력
    return TRUE;	// 방문 성공
  }
  return FALSE;			// 방문 실패(이미 방문)	
}

void DFShowGraphVertex(ALGraph * pg, int startV)
{
  // initizliation
  Stack stack;
  int visitV = startV, nextV;
  StackInit(&stack);
  VisitVertex(pg, visitV);	// 시작 정점 방문
  SPush(&stack, visitV);	// 시작 정점 떠나며 스택으로
  
  while(LFirst(&(pg->adjList[visitV]), &nextV) == TRUE)  // 연결된 정점 정보를 얻어서
  {
    int visitFlag = FALSE;
    if(VisitVertex(pg, nextV) == TRUE)
    {
      // 방문 시도했는데 방문 성공 시
      // 방문 정점 떠나기 위해 해당 정보를 스택으로
      SPush(&stack, visitV);
      visitV = nextV;
      visitFlag = TRUE;
    }
    else
    {
      // 방문 시도했는데 방문한 적이 있다면 연결리스트의 정보를 가져옴
      // 연결된 다른 정점을 찾아서 방문을 시도하는 일련의 과정
      while(LNext(&(pg->adjList[visitV]), &nextV) == TRUE)
      {
        if(VisitVertex(pg, nextV) == TRUE)
        {
          SPush(&stack, visitV);
          visitV = nextV;
          visitFlag = TRUE;
          break;  // 방문 성공
        }
      }
    }
    
    if(visitFlag == FALSE)  // 연결된 정점과의 방문 완료시
    {
      if(SIsEmpty(&stack) == TRUE)
        break;  // 스택 비면 종료
      else
        visitV = SPop(&Stack); // 되돌아가기 위한 Pop 연산
    }
  }
  // 마무리
  memset(pg->visitInfo, 0, sizeof(int)*pg->numV);
}
```



##### BFS 구현 

- 방문 정보 기록을 위한 배열 선언
- 방문 차례 기록을 위한 Queue 선언
- 가까운 것 중 누구에게 먼저 가는지는 그렇게 중요하지 않으며 대상의 정보를 큐에 저장
- 큐에 꺼내어 방문을 진행하고, 방문한 정점의 정보를 다시 큐에 넣는 일련의 과정을 *큐가 빌 때까지* 반복

```c
// ALGraphBFS.h
enum {A,B,C,D,E,F,G,H,I,H};

typedef struct _ual
{
  int numV;
  int numE;
  List * adjList;
  int * visitInfo;
} ALGraph;

void GraphInit(ALGraph * pg, int nv);
void GraphDestory(ALGraph * pg);
void AddEdge(ALGraph * pg, int fromV, int toV);
void ShowGraphEdgeInfo(ALGraph * pg);
void BFShowGraphVertex(ALGraph * pg, int startV);
```

```c
void BFShowGraphVertex(ALGraph * pg, int startV)
{
  Queue queue;
  int visitV = startV;
  int nextV;
  
  QueueInit(&queue);
  VisitVertex(pg, visitV);
  
  while(LFirst(&(pg->adjList[visitV]), &nextV) == TRUE)
  {
    if(VisitVertex(pg, nextV) == TRUE)
      Enqueue(&queue, nextV);
    
    while(LNext(&(pg->adjList[visitV]), &nextV) == TRUE)
    {
      if(VisitVertex(pg, nextV) == TRUE)
        Enqueue(&queue, nextV);
    }
    
    if(QIsEmpty(&queue) == TRUE)
      break;	// 큐가 비면 탈출조건 성립
    else
      visitV = Dequeue(&queue);
  }
  memset(pg->visitInfo, 0, sizeof(int)*pg->numV);
}
```



### 14.4 최소 비용 신장 트리

---

- 단순경로 : 중복된 간선을 포함하지 않는 경로
- 사이클 : 시작점과 끝점이 같은 단순 경로(폐공간)
- 신장 트리 : 어떻게 경로를 구성하더라도 '사이클'을 형성하지 않는 그래프
  - 그래프의 모든 정점이 간선에 의해서 하나로 연결되어 있다.
  - 그래프 내에서 사이클을 형성하지 않는다.

##### 크루스칼 알고리즘

- 가중치를 기준으로 간선을 정렬한 후에 MST가 될때 까지 간선을 하나씩 선택 또는 삭제해나가는 과정
  - 간선을 다 지우고 가중치가 낮은 것부터 추가 + 사이클 형성 간선은 건너뜀
  - 최소 비용 신장 트리의 조건인 **간선의 수 + 1 = 정점의 수** 만족시 최소 비용 신장 트리 형성 완료
- 높은 가중치의 간선을 하나씩 빼는 방식
  -  독립정점을 만들지 않고 신장 트리 조건을 항상 확인
  - 최소 비용 신장 트리의 조건인 **간선의 수 + 1 = 정점의 수** 만족시 최소 비용 신장 트리 형성 완료

##### 구현

- 가중치를 기준으로 간선을 내림차순으로 정렬한 다음 높은 가중치의 간선부터 시작해서 하나씩 그래프에서 제거하는 방식
- "이 간선을 삭제한 후에도 이 간선에 의해 연결된 두 정점을 연결하는 경로가 있는가?" → DFShowGraphVertex 함수 확장
- "그래프를 구성하는 간선들을 가중치를 기준으로 정렬할 수 있어야 함" → 우선순위 큐 활용

```c
// header
enum {A,B,C,D,E,F,G,H,I,j}

typedef struct _ual
{
  int numV;
  int numE;
  List * adjList;
  int * visitInfo;
  PQueue pqueue;		// 간선의 가중치 정보 저장
} ALGraph;

void GraphInit(ALGraph * pg, int nv);
void GraphDestory(ALGraph * pg);
void AddEdge(ALGraph * pg, int fromV, int toV, int weight);
void ShowGraphEdgeInfo(ALGraph * pg);
void DFShowGraphVertex(ALGraph * pg, int startV);
void ConKruskalMST(ALGraph * pg);		// 최소 비용 신장 트리의 구성
void ShowGraphEdgeWeightInfo(ALGraph * pg);		// 가중치 정보 출력
```



```c
int PQWeightComp(Edge d1, Edge d2)
{
  return d1.weight - d2.weight;
} // 가중치 기준 내림차순으로 간선 정보 꺼내기 위한 정의

void GraphInit(ALGraph * pg, int nv)
{
  ....
   
  // 우선순위 큐 초기화
  PQueueInit(&(pg->pqueue), PQWeightComp);
}

void AddEdge(ALGraph * pg, int fromV, int toV, int weight)
{
  Edge edge = {fromV, toV, weight};		// 간선 가중치 정보 담음
  LInsert(&(pg->adjList[fromV]), toV);
  LInsert(&(pg->adjList[toV]), fromV);
  pg->numE += 1;
  
  // 간선의 가중치 정보 우선순위 큐에 저장
  PEnqueue(&(pq->pqueue), edge);
}

void ConKruskalMST(ALGraph * pg)	// 크루스칼 알고리즘 기반 MST 구성
{
  Edge recvEdge[20];		// 복원 간선 정보 저장
  Edge edge;
  int eidx=0, i;
  
  // MST 형성할 때까지 while문 반복
  while(pg->numE+1 > pg->numV)		// MST 간선의 수 + 1 == 정점의 수
  {
    edge = PDequeue(&(pg->pqueue));	// 가중치 순으로 간선 정보 획득
    RemoveEdge(pg, edge.v1, edge.v2);	// 획득한 정보의 간선 실제 삭제
    
    if(!IsConnVertex(pg, edge.v1, edge.v2))		// 삭제 후 두 정점 연결경로 있는지 확인 ("삭제 해도 연결 되는가?")
    {
      RecoverEdge(pg, edge.v1, edge.v2, edge.weight);	// 연결 경로 없으면 간선 복원
      recvEdge[eidx++] = edge;
    }
  }
  
  // 우선순위 큐에서 삭제된 간선의 정보를 회복
  for(i=0; i<eidx; i++)
    PEnqueue(&(pg->pqueue), recvEdge[i]);
}

```

```c
// 간선의 소멸
void RemoveEdge(ALGraph * pg, int fromV, int toV)
{
  RemoveWayEdge(pg, fromV, toV);
  RemoveWayEdge(pg, toV, fromV);
  (pg->numE)--;
}  // 인접 리스트 기반 무방향 그래프인 관계로 하나의 간선을 완전히 소멸하기 위해서는 두 개의 간선 정보를 소멸시켜야 한다.

void RecoverEdge(ALGraph * pg, int fromV, int toV, int weight)
{
  LInsert(&(pg->adjList[fromV]), toV);
  LInsert(&(pg->adjList[toV]), fromV);
  (pg->numE)++;
}		// AddEdge 함수와 달리 간선의 가중치 정보를 별도로 저장하지 않는다. 

// 한쪽 방향 간선의 소멸
void RemoveWayEdge(ALGraph * pg, int fromV, int toV)
{
  int edge;
  // 지워질 때까지 찾아가서 삭제
  if(LFirst(&(pg->adjList[fromV]), &edge))
  {
    if(edge == toV)
    {
      LRemove(&(pg->adjList[fromV]));
      return;
    }
    while(LNext(&(pg->adjList[fromV]), &edge))
    {
      if(edge == toV)
      {
        LRemove(&(pg->adjList[fromV]));
        return;
      }
    }
  }
}

// 인자로 전달된 두 정점이 연결되어 있다면 TRUE, 그렇지 않다면 FALSE
int IsConnVertex(ALGraph * pg, int v1, int v2)
{
  Stack stack;
  int visitV = v1;
  int nextV;
  
  StackInit(&stack);
  VisitVertex(pg, visitV);
  SPush(&stack, visitV);
  
  while(LFirst(&(pg->adjList[visitV]), &nextV) == TRUE)
  {
    int visitFlag = FALSE;
    
    // 정점을 돌아다니는 도중 목표를 찾는다면 TRUE 반환
    if(next == v2)
    {
      // 함수가 반환하기 전 초기화 진행
      memset(pg->visitInfo, 0, sizeof(int)*pg->numV);
      return TRUE;	// 목표를 찾음
    }
    
    if(VisitVertex(pg, nextV) == TRUE)
    {
      SPush(&stack, visitV);
      visitV = nextV;
      visitFlag = TRUE;
    }
    else
    {
      while(LNext(&(pg->adjList[visitV]), &nextV) == TRUE)
      {
        // 정점을 돌아다니는 도중 목표를 찾는다면 TRUE 반환
        if(nextV == v2)
        {
          // 함수가 반환하기 전 초기화 진행
          memset(pf->visitInfo, 0, sizeof(int)*pg->numV);
          return TRUE;		// 목표 찾음
        }
        if(VisitVertex(pg, nextV) == TRUE)
        {
          SPush(&stack, visitV);
          visitV = nextV;
          visitFlag = TRUE;
          break;
        }
      }
    }
    if(visitFlag == FALSE)
    {
      if(SIsEmpty(&stack) == TRUE)
        break;
      else
        visitV = SPop(&stack);
    }
  }
  memset(pg->visitInfo, 0, sizeof(int)*pg->numV);
  return FALSE;
}
```













