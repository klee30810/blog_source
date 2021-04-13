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











