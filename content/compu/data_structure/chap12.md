---
title: "Chap12. Search-2"
description: "Search-2"
menuTitle : "Search-2"
weight: 12
date: 2021-03-07T14:35:25+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

### 12.1 균형 잡힌 이진 탐색 트리 : AVL 트리의 이해

---

- 트리 → 이진트리 → 이진탐색트리 → 균형잡힌 이진탐색트리
- 이진 탐색 트리의 연산은 원래 \\(O(\log_2n)\\)의 시간 복잡도를 가지지만 트리 균형이 맞지 않을수록 \\(O(n)\\)에 가까운 시간 복잡도를 보인다. ⇒ **약간의 순서 변화로 균형이 잡힌다.**
- AVL 는 균형 인수(Balance Factor)를 기준으로 트리의 균형을 잡기 위한 리밸런싱시기를 결정한다.
  - 균형 인수 = 왼쪽 서브 트리의 높이 - 오른쪽 서브 트리의 높이
  - **균형 인수의 절댓값이 2 이상인 경우 리밸런싱 진행!**

##### 리밸런싱

- LL회전 : 왼쪽 자식노드로 만 이어져 있는 트리를 오른쪽 회전을 통해 밸런스 잡기

- RR 회전 : 오른쪽 자식노드로 만 이어져 있는 트리를 왼쪽 회전을 통해 밸런스 잡기

- LR 상태 : 트리 이루를한번의 회전으로 균형이 잡히는 LL 혹은 RR 상태를 만든 후 회전을 시킨다.

  - LL, RR은 부모 자식의 관계가 바뀌는 부수적인 효과가 있다.

  ![image](/images/compu/DS/chap12/1.png)

  - LR 회전

  ![image](/images/compu/DS/chap12/2.png)

  → LR 상태 LR 회전, 그리고 RL상태 RL회전은 방향에서만 차이를 보인다



### 2. AVL 트리의 구현

- BST 함수 트리에 노드 추가 및 제거 과정에서 트리의 균형이 깨지게 되므로 해당 함수(BSTInsert, BSTRemove)에 리밸런싱 단계를 추가한다.

```c
// 두 서브 트리의 높이의 차(균형 인수)를 반환
int GetHeightDiff(BTreeNode * bst)
{
  int lsh;		// left sub tree height
  int rsh;		// right sub tree height
  
  if(bst == NULL)
    return 0;
  
  lsh = GetHeight(GetLeftSubTree(bst));
  rsh = GetHeight(GetRightSubTree(bst));
  
  return lsh - rsh;
}

// 트리의 높이를 계산하여 반혼
int GetHeight(BTreeNode * bst)
{
  int leftH;		// left height
  int rightH;		// right height
  
  if(bst == NULL)
    return 0;
  
  leftH = GetHeight(GetLeftSubTree(bst));
  rightH = GetHeight(GetRightSubTree(bst));
  
  // 큰 값의 높이를 반환한다.
  if(leftH > rightH)	// 높이가 작은 값 버림
    return leftH + 1;
  else
    return rightH + 1;
}
```

##### 리밸런싱 도구

```c
// LL 회전
BTreeNode * RotateLL(BTreeNode * bst)
{
  BTreeNode * pNode;		// parent node
  BTreeNode * cNode;		// child node
  
  // pNode와 cNode가 LL회전을 위해 적절한 위치를 가리키게 한다.
  pNode = bst;
  cNode = GetLeftSubTree(pNode);
  
  // LL회전
  ChangeLeftSubTree(pNode, GetRightSubTree(cNode));
  ChangeRightSubTree(cNode, pNode);
  
  // LL회전으로 인해 변경된 루트 노드의 주소값 반환
  return cNode;
}

// RR 회전
BTreeNode * RotateRR(BTreeNode * bst)
{
  BTreeNode * pNode;		// parent node
  BTreeNode * cNode;		// child node

	// pNode와 cNode가 RR회전을 위해 적절한 위치를 가리키게 한다.
  pNode = bst;
  cNode = GetRightSubTree(pNode);
  
  // RR 회전
  ChangeRightSubTree(pNode, GetLeftSubTree(cNode));
  ChangeLeftSubTree(cNode, pNode);
  
  // 변경된 주소값 반환
  return cNode;
}

// LR회전 : 부분적 RR회전에 이어서 LL회전 진행
BTreeNode * RotateLR(BTreeNode * bst)
{
  BTreeNode * pNode;		// parent node
  BTreeNode * cNode;		// child node
  
  // pNode와 cNode가 LR회전을 위해 적절한 위치를 가리키게 한다.
  pNode = bst;
  cNode = GetLeftSubTree(pNode);
  
  // LR회전 : 부분적 RR회전 후 LR 회전
  ChangeLeftSubTree(pNode, RotateRR(cNode));
  return RotateLL(pNode);
}

// RL회전 : 부분적 LL회전에 이어서 RR회전 진행
BTreeNode * RotateRL(BTreeNode * bst)
{
  BTreeNode * pNode;			// parent node
  BTreeNode * cNode;			// child node
  
  // pNode와 cNode가 RL회전을 위해 적절한 위치를 가리키게 한다.
  pNode = bst;
  cNode = GetRightSubTree(pNode);
  
  // RL회전 : 부분적 LL회전 후 RR회전
  ChangeRightSubTree(pNode, RotateLL(cNode));
  return RotateRR(pNode);
}

```

- LL, LR, RR, RL 상태의 구분

![image](/images/compu/DS/chap12/3.png)

```c

BTreeNode * Rebalance(BTreeNode ** pRoot) // 상황에 따른 적절한 함수 실행
{
  int hDiff = GetHeightDiff(*pRoot);	// 	균형 인수 계산
  
  // 균형 인수가 +2 이상이면 왼쪽으로 길게 불균형이므로 LL 또는 LR상태
  if(hDiff > 1)		// 왼쪽 서브트리 방향 높이가 2이상 크다면
  {
    if(GetHeightDiff(GetLeftSubTree(*pRoot))>0)
  	 *pRoot = RotateLL(*pRoot);
	  else
   	 *pRoot = RotateLR(*pRoot);
  }
    

// 균형 인수가 -2 이하면 오른쪽으로 길계 불균형이므로 RR 또는 RL상태
	if(hDiff < -1)	// 오른쪽 서브트리 방향으로 2 이상 크다면
  {
    if(GetHeightDiff(GetRightSubTree(*pRoot)) < 0)
      *pRoot = RotateRR(*pRoot);
    else
      *pRoot = RotateRL(*pRoot);
  }
  
  return *pRoot;
}
```



- BSTInsert 함수의 마지막 부분에서 루트 노드를 기준으로 리밸런싱의 필요성을 판단하고 리밸런싱 진행 → 하위 노드 기준으로는 불균형이 또 일어날 수 있다.

  ⇒ 노드 추가 후 모든 노드의 균형을 검사하도록 **재귀적으로** 재구성 한다.

  - 루트 노드를 대상으로 하여 데이터의 저장 시도(함수 호출)
    1. 루트 노드에 저장된 데이터와 새 데이터 비교
    2. 비교하여 새 데이터의 값이 작으면 왼쪽 자식 노드를 루트 노드로 하여 데이터의 저장 시도
    3. 비교하여 새 데이터의 값이 크면 오른쪽 자식 노드를 루트 노드로 하여 데이터의 저장 시도
    4. 저장이 완료되면 해당 루트 노드를 기준으로 리밸런싱 

```c
BTreeNode * BSTInsert(BTreeNode ** pRoot, BSTData data)
{
  if(*pRoot == NULL)	// 아무것도 없는 상태
  {
    *pRoot = MakeBTreeNode();
    SetData(*pRoot, data);
  }
  else if(data < GetData(*pRoot))
  {
    BSTInsert(&((*pRoot)->left), data);
    *pRoot = Rebalance(pRoot); // 자리를 다 잡고 올라가면 Rebalance 호출
  }
  else if(data > GetData(*pRoot))
  {
    BSTInsert(&((*pRoot)->right), data);
    *pRoot = Rebalance(pRoot);
  }
  else
  {
    return NULL;		// 키의 중복 허용하지 않음
  }
  
  return *pRoot;
}
```

