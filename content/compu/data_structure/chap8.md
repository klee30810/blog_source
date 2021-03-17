---
title: "Chap8. Tree"
description: "Tree Data Structure"
menuTitle : "Tree"
weight: 8
date: 2021-03-07T14:35:17+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- 대부분의 비선형 자료구조는 표현을 목적으로 하며 Tree 구조 또한 그렇다.

## 1. 트리의 개요

---

#### (1) 이진트리

- 연결리스트 : 노드를 추가하지 않고 데이터를 삽입했다.
- 트리 : 구조체를 만들고 삽입, 삭제, 추출 등 구조체를 위한 도구가 될 함수를 만들 것이다. 방향보다는 서로간 연결이 더 중요하다. 각자 서로의 주소값을 알고 있으면 연결정보가 있는 것이다.
- **계층적 관계(Hierarchical Relationship)** 을 표현하는 자료구조
- **데이터 표현을 위한 도구**
  - 노드 node: 트리의 구성요소, 실제 data를 담을 수 있는 구조
  - 간선 edge: 노드 간 연결선 
  - 루트 노드 root node: 트리 최상위 노드
  - 단말 노드 terminal code : 아래로 다른 노드가 연결되어 있지 않는 노드
  - 내부 노드 internal node : 단말 노드를 제외한 모든 노드
  - 노드간 관계 : 부모 노드, 자식 노드, 형제 노드
- 서브 트리 : 각 트리는 또다른 서브 트리로 이뤄져 있고 그 주고는 **재귀적**이다.
- 이진트리의 조건

> **1. 루트 노드를 중심으로 두 개의 서브 트리로 나뉘어 진다.**
>
> **2. 나뉘어진 두 서브트리도 모두 이진트리어어야 한다.**

- 이진 트리는 구조가 **재귀적** 이고, 트리와 관련된 연산은 재귀적으로 사고하고 재귀적으로 구현할 필요가 있다.

#### (2) 공집합 노드

- 이진 트리의 재귀성을 계속하기 위하여 공집합(Empty Set)도 노드로 간주한다. 
- 트리의 높이와 레벨의 최대값은 같다
  - 포화 이진 트리 : 모든 레벨에 노드가 꽉찬 트리
  - 완전 이진 트리 : 빈 틈 없이 채워진 이진 트리 ⇒ 위에서 아래로 왼쪽에서 오른쪽으로 채워진 트리

### 2. 이진 트리의 구현

---

- 이진 트리 활용 도구 만들기
- 배열 기반 : 노드에 번호를 부여하고 그 번호에 해당하는 값을 배열의 인덱스 값으로 활용, 편의상 배열의 첫 번째 요소는 사용하지 않는다.
- 연결리스트 기반 : 트리의 구조와 리스트의 연결 구조가 일치한다. → 직관적 이해

```c
typedef struct _bTreeNode
{
  BTData data;
  struct _bTreeNode * left;
  struct _bTreeNode * right;
} BTreeNode;
```

- 이진 트리의 모든 노드는 직/간접적으로 연결되어있다. 따라서 **루트 노드의 주소값** 만 기억하면, 이진 트리 전체를 가리키는 것과 다름없다.
- 논리적으로도 하나의 노드는 그 자체로 이진 트리다. 따라서 노드를 표현한 구조체는 실제로 이진 트리를 표현한 구조체이다.

```c
BTreeNode * MakeBTreeNode(); // 노드의 생성
// 노드를 동적으로 할당하고 SetData 함수로 채우고, left & right는 NULL로 자동 초기화

BTData GetData(BTreeNOde * bt); // 노드에 저장된 데이터 반환

void SetData(BTreeNode * bt, BTData data) // 노드에 데이터 저장
// 노드에 직접 접근하는 것 보다 함수를 통한 접근이 보다 좋은 접근
// 구조체가 정의 되면 관련 연산은 함수를 통하는 것이 좋다.

// 하나의 노드도 서브트리 이므로 SubTree로 이름 짓는 것이 타당
BTreeNode * GetLeftSubTree(BTreeNode * bt); // 왼쪽 서브 트리의 주소 값 반환

BTreeNode * GetRightSubTree(BTreeNode * bt); // 오른쪽 서브 트리의 주소 값 반환

void MakeLeftSubTree(BTreeNode * main, BTreeNode * sub); // main의 서브 왼쪽 서브 트리로 sub 연결
void MakeRightSubTree(BTreeNode * main, BTreeNode * sub); // main의 오른쪽 서브 트리로 sub 연결
```



```c
int main()
{
  BTreeNode * ndA = MakeBTreeNode();  // 노드 A 생성
  BTreeNode * ndB = MakeBTreeNode();  // 노드 B 생성
  BTreeNode * ndC = MakeBTreeNode();  // 노드 C 생성
  
  // ndA, ndB, ndC를 이용한 SetData 함수 호출을 통해 데이터를 채운 후...
  
  // 노드 A의 왼쪽 자식 노드로 노드 B 연결
  MakeLeftSubTree(ndA, ndB);
  // 노드 A의 오른쪽 자식 노드로 노드 C 연결
  MakeRightSubTree(ndA, ndB);
  
  ....
}
```

- 이진 트리의 구현

```c
BTreeNode * MakeBTreeNode()
{
  BTreeNode * nd = (BTreeNode *)malloc(sizeof(BTreeNode));
  nd->left = NULL;
  nd->right = NULL;
  return nd;
}

BTData GetData(BTreeNode * bt)
{
  return bt->data;
}

void SetData(BTreeNode * bt, BTData data)
{
  bt->data = data;
}

BTreeNode * GetLeftSubTree(BTreeNode * bt)
{
  return bt->left;
}

BTreeNode * GetRightSubTree(BTreeNode * bt)
{
  return bt->right;
}

void MakeLeftSubTree(BTreeNode * main, BTreeNode * sub)
{
  if(main->left != NULL)
    free(main->left);  // 만약 하부 노드가 있었으면 메모리 누수
  
  main->left = sub;
}

void MakeRightSubTree(BTreeNode * main, BTreeNode * sub)
{
  if(main->right != NULL)
    free(main->right);
  
  main->right = sub;
}
```



### 3. 이진 트리의 순회(Travelsal)

---

- 순회의 세가지 방법 : 중위, 후위, 전위 ⇒ 루트 노드를 방문하는 시점에 따라서 나뉘어 짐

```c
// 이진트리 중위순회
void InorderTraverse(BTreeNode * bt)
{
  InorderTraverse(bt->left);
  printf("%d \n", bt->data); // root
  InorderTraverse(bt->right);
  // 노드에 트리가 들어오는 경우 & 탈출 경우 없음
}

// 재귀 탈출 조건 포함
void InorderTraverse(BTreeNode * bt)
{
  if(bt==NULL)		// 노드가 단말 노드이면 단말 노드의 자식노드는 NULL (들어가봐야 앎)
    return;
  
  InorderTraverse(bt->left);
  printf("%d \n", bt->data); // root
  InorderTraverse(bt->right);
}

// 전위 순회
void PreorderTraverse(BTreeNode * bt)
{
  if(bt==NULL)		
    return;
  
  printf("%d \n", bt->data); // root
  InorderTraverse(bt->left);
  InorderTraverse(bt->right);
}

// 후위 순회
void PostorderTraverse(BTreeNode * bt)
{
  if(bt==NULL)		
    return;
  
  InorderTraverse(bt->left);
  InorderTraverse(bt->right);
  printf("%d \n", bt->data); // root
}


```



- 함수 포인터를 이용한 노드의 방문이유 구성하기

```c
// 함수 포인터 형 정의
typedef void VisitFuncPtr(BTData data) // (* VisitFuncPtr 가능)
  
void InorderTraverse(BTreeNode * bt, VisitFuncPtr action)
{
  if(bt == NULL)
    return;
  
  InorderTraverse(bt->left, action);
  action(bt->data);    // 노드의 방문
  InorderTraverse(bt->right, action);
}

void ShowIntData(int data) // BTData가 int 가정
{
  printf("%d ", data);
  // 목적을 사용자가 직접 정의하는 부분
}
```



### 4. 수식 트리 (Expression Tree)의 구현

---

- 수식 트리의 이해 : 중위 표기법의 수식을 수식 트리로 변환하는 프로그램의 작성이 목적!
  - 중위 표기법 수식은 사람이 인식하기 좋은 수식이나 컴퓨터는 인식이 어렵다.
  - 수식 트리는 해석이 쉽다.
  - 연산자 우선순위를 고려하지 않아도 되고, In & Post & Pre로 면환이 쉽다. 

중위 표기법 수식 → 후위 표기법 수식 → 수식 트리

```c
#include "BinaryTree2.h"

BTreeNode * MakeExpTree(char exp[]); 	// 수식 트리 구성
// 후위 표기법의 수식을 인자로 받아서 수식 트리를 구성하고 루트 노드의 주소 값을 반환한다.

int EvaluateExpTree(BTreeNode * bt);	// 수식 트리 계산

void ShowPrefixTypeExp(BTreeNode * bt);	// 전위 표기법 기반 출력
void ShowInfixTypeExp(BTreeNode * bt);	// 중위 표기법 기반 출력
void ShowPostfixTypeExp(BTreeNode * bt);	// 후위 표기법 기반 출력
```

- 후위 표기법의 수식에서 먼저 등장하는 피연산자와 연산자를 이용해서 트리의 하단부터 구성해 나가고 이어서 윗부분을 구성해나간다.
- 차례차례 스택을 쌓는다. 서브트리가 있다면 스택에 그대로 쌓으면 된다. → 루트 노드가 쌓이면 그 자체로 서브트리로 인식

```c
BTreeNode * MakeExpTree(char exp[])
{
  /*
  - 피연산자는 스택으로 옮긴다.
  - 연산자를 만나면 스택에서 두 개의 피연산자 꺼내 자식노드로 연결
  - 자식 노드를 연결해서 만들어진 트리는 다시 스택으로 옮긴다.
  - 완성 결과를 스택에 저장하고, 수식트리의 root node 주소값을 반환한다.
  */
  Stack stack;
  BTreeNode * pnode;
  int expLen = strlen(exp);
  int i;
  
  for(i=0; i<expLen; i++)
  {
    pnode = MakeBTreeNode();
    if(isdigit(exp[i]))					// 피연산자라면
    {
      SetData(pnode, exp[i]-'0');  // 문자를 정수로 변경
    }
    else	// 연산자라면
    {
      MakeRightSubTree(pnode, SPop(&stack));
      MakeLeftSubTree(pnode, SPop(&stack));
      SetData(pnode, exp[i]);
    }
    SPush(&stack, pnode);
  }
  
  return SPop(&stack);
}

// 계산 기본구성
int EvaluateExpTree(BTreeNode * bt)
{
  int op1, op2;
  
  // 단말노드라면 바로 탈출
  if(GetLeftSubTree(bt)==NULL && GetRightSubTree(bt)==NULL)
    return GetData(bt);
  
  // SubTree 도 올 수 있으므로 재귀적 표현 필요!
  op1 = EvaluateExpTree((GetLeftSubTree(bt)));
  op2 = EvaluateExpTree((GetRightSubTree(bt)));
  
  switch(GetData(bt))
  {
    case '+':
      return op1+op2;
    
    case '-':
      return op1-op2;

    case '*':
      return op1*op2;

    case '/':
      return op1/op2;
  }

  return 0;
}
```







