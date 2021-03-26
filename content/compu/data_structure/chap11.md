---
title: "Chap11. Search-1"
description: "Search-1"
menuTitle : "Search-1"
weight: 11
date: 2021-03-07T14:35:23+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- "탐색을 잘하기 위한 저장 방법은 무엇인가?"
- **탐색은 이전에 데이터 정렬이 필수적**

### 1. 탐색의 이해와 보간 탐색

---

- 효율적인 탐색을 위한 저장방법, 효율적인 탐색이 가능한 대표적인 자료구조는 "이진트리"이다.

##### 보간 탐색

- 이진 탐색 : **무조건 중간**에 위치한 데이터를 탐색의 위치로 결정
- 보간 탐색 : **대상에 비례하여** 탐색의 위치를 결정 → 단순 비례 가정 후 비슷한 위치 찾기 노력
- 비례식 구성 → **완전 비례 가정**
  - s : 탐색 대상이 저장된 인덱스 값 

$$ A:Q=(high-low):(s-low) \Rightarrow s=\frac{Q}{A}(high-low)+low \\ \Rightarrow s=\frac{x-arr[low]}{arr[high]-arr[low]}(high-low)+low$$

##### 탐색 키 & 탐색 데이터

- 실제 프로그램 개발에 있어서 **탐색의 대상은 '데이터'가 아닌 '키(key)' 이다.**

```c
typedef Key int;			// search key
typedef Data double;  // search data

// 원래는 구분이 되어야 좋다
typedef struct item
{
  Key searchKey;		// search key
  Data searchData;	// search data
} Item;
```

```c
// 이진 탐색의 보간 탐색으로의 개선
int BSearchRecur(int ar[], int first, int last, int target)
{
  int mid;
  /*
  if(first > last)   => 탐색 대상이 존재하지 않는 경우 범위를 넘어서게 됨
    return -1; */
  
  // 범위에 없다면
  if(ar[first]>target || ar[last]<target)
    return -1; // -1은 반환의 실패
  
  // mid = (first+last)/2;
  mid = ((double)(target-ar[first]) / (ar[last]-ar[first]) * (last-first)) + first;
  
  if(ar[mid] == target)
    return mid;
  else if(target < ar[mid])
    return BSearchRecur(ar, first, mid-1, target);
  else
    return BSearchRecur(ar, mid+1, last, target);
  
}
```



### 2. 이진 탐색 트리

---

- 이진 탐색 트리 = 이진 트리 + 데이터의 저장규칙
  - 이진 탐색 트리 노드에 저장된 키(key)는 유일
  - 루트 노드의 키 > 왼쪽 서브 트리 키
  - 루트 노드의 키 < 오른쪽 서브 트리 키

##### 노드 추가 및 탐색 과정

![image](/images/compu/DS/chap11/1.png)

##### 구현 방안

⇒ 이진 탐색 트리도 이진 트리이니, 이전에 구현한 이진 트리를 활용하여 구현한다. → BinaryTree2.h & BinaryTree2.c  활용 / 기능이 부족하다면 도구의 성능을 확장 및 개선시키면 된다.

```c
// BinaryTree2.h

BTreeNode * MakeBTreeNode(void); 
// 노드를 동적으로 할당해서 그 노드의 주소값을 반환

BTData GetData(BTreeNode * bt);
// 노드에 저장된 데이터를 반환

void SetData(BTreeNode * bt, BTData data);
// 인자로 전달된 데이터를 노드에 저장

BTreeNode * GetLeftSubTree(BTreeNode * bt);
// 인자로 전달된 노드의 왼쪽 자식 노드 주소값 반환

BTreeNode * GetRightSubTree(BTreeNode * bt);
// 인자로 전달된 노드의 오른쪽 자식 노드 주소값 반환

void MakeLeftSubTree(BTreeNode * main, BTreeNode * sub);
// 인자로 전달된 노드의 왼쪽 자식 노드 삭제

void MakeRightSubTree(BTreeNode * main, BTreeNode sub);
// 인자로 전달된 노드의 오른쪽 자식 노드 교체
```

```c
// BinarySearchTree.h

void BSTMakeAndInit(BTreeNode ** pRoot);
// BST의 생성 및 초기화

BSTData BSTGetNodeData(BTreeNode * bst);
// 노드에 저장된 데이터 반환

void BSTInsert(BTreeNode ** pRoot, BSTData data);
// BST를 대상으로 하여 데이터 저장 (노드의 생성과정 포함)

BTreeNode * BSTSearch(BTreeNode * bst, BSTData target);
// BST를 대상으로 데이터 탐색
```

```c
int main()
{
  BTreeNode * bstRoot;		// bstRoot는 BST 루트 노드를 가리킴
  BTreeNode * sNode; 		
  
  BSTMakeAndInit(&bstRoot);		// Binary Search Tree의 생성 및 초기화
  
  BSTInsert(&bstRoot, 1);
  BSTInsert(&bstRoot, 2);
  BSTInsert(&bstRoot, 3);
  
  // 노드 탐색
  sNode = BSTSearch(bstRoot, 1);
  
  // 사용자 입장에서는 단순히 데이터를 저장하고 탐색하면 되고 트리가 어떤 형태인지는 알 필요 없다
  if(sNode == NULL)
    printf("탐색 실패 \n");
  else
    printf("탐색 키 값: %d \n", BSTGetNodeData(sNode));
  
  return 0;
}
```

- 삽입과 탐색 : 가는 경로 동일 → 비교 대상이 없을 때까지 내려간다. 비교대상이 없는 그 위치가 새 데이터가 저장될 위치이다.

```c
void BSTInsert(BTreeNode ** pRoot, BSTData data)
{
  BTreeNode * pNode = NULL;			// parent node
  BTreeNode * cNode = *pRoot;		// current node
  BTreeNode * nNode = NULL;			// new node
  
  // 새로운 노드가(새 데이터가 담긴 노드가 추가될 위치를 찾는다.)
  while(cNode != NULL)
  {
    if(data == GetData(cNode))
      return;			// 데이터의 중복 허용 금지
    
    pNode = cNode;		// parent node 백업 -> 자식 노드 확인시 필요
    
    if(GetData(cNode) > data)
      cNode = GetLeftSubTree(cNode);
    else
      cNode = GetRightSubTree(cNode);
    
    // pNode의 자식 노드로 새 노드를 추가
    if(pNode != NuLL)		// 새 노드가 루트 노드가 아니라면
    {
      if(data < GetData(pNode))
        MakeLeftSubTree(pNode, nNode);
      else
        MakeRightSubTree(pNode, nNode);
    }
    else			// 새 노드가 루트 노드라면,
    {
      *pRoot = nNode;
    }
  }
}

BTreeNode * BSTSearch(BTreeNode * bst, BSTData target)
{
  BTreeNode * cNode = bst;		// current node
  BSTData cd; 								// current data
  
  while(cNode != NULL)			// 삽입 과정을 근거로 탐색 진행
  {
    cd = GetData(cNode);
    
    if(target == cd)
      return cNode;		// 탐색 성공시 해당 노드 주소값 반환
    else if(target < cd)
      cNode = GetLeftSubTree(cNode);
    else
      cNode = GetRightSubTree(cNode);
  }
  
  return NULL;		// 탐색 대상 저장되어있지 않음
}
```



##### 이진 탐색 트리 삭제 구현

- 삭제는 해당 노드의 부모 노드의 주소값을 알아야 함
- 삭제로 빈 자리를 어떻게 채워야 할까?
- 루트 노드가 루트 노드가 아닌 것처럼 취급할 것 ⇒ 삭제 이전에 노드를 하나 만들어서 root가 서브트리인 것처럼 할 것
  1. 삭제할 노드가 단말 노드인 경우
  2. 삭제할 노드가 하나의 자식 노드(서브 트리)를 갖는 경우
  3. 삭제할 노드가 두 개의 자식 노드(서브 트리)를 갖는 경우

1. 삭제 노드가 단말 노드인 경우

```c
// dNode와 pNode는 삭제할 노드와 이의 부모 노드를 가리키는 포인터 변수
if(삭제할 노드가 단말 노드이다!)
{
  if(GetLeftSubTree(pNode) == dNode)		// 삭제할 노드가 왼쪽 자식 노드라면,
    RemoveLeftSubTree(pNode);			// 왼쪽 자식 노드 트리에서 제거
  else													// 삭제할 노드가 오른쪽 자식 노드라면,
    RemoveRightSubTree(pNode);		// 오른쪽 자식 노드 트리에서 제거
}
```

2. 삭제할 노드가 하나의 자식 노드를 갖는 경우

   → 삭제 후 자식 노드를 가져다 놓는다.

```c
// dNode와 pNode는 삭제할 노드와 이의 부모 노드를 가리키는 포인터 변수
if(삭제할 노드가 하나의 자식 노드를 지닌다)
{
  BTreeNode * dcNode;			// 삭제 대상의 자식 노드를 가리키는 포인터 변수
  
  // 삭제 대상의 자식 노드를 찾는다. 자식노드 백업
  if(GetLeftSubTree(dNode) != NULL)    // 왼쪽 자식 노드
    dcNode = GetLeftSubTree(dNode);
  else									// 오른쪽 자식 노드
    dcNode = GetRightSubTree(dNode);
  
  // 삭제 대상의 부모 노드와 자식 노드를 연결한다
  if(GetLeftSubTree(pNode) == dNode) 		// 삭제 대상이 왼쪽 자식 노드라면,
    ChangeLeftSubTree(pNode, dcNode);		// 왼쪽 연결
  else 					// 삭제 대상이 오른쪽 자식 노드 이면,
    ChangeRightSubTree(pNode, dcNode);	// 오른쪽 연결
}

```

3. 삭제할 노드가 두 개의 자식 노드(서브 트리)를 갖는 경우
   - 삭제 대상을 **왼쪽 서브 트리에서 가장 큰 값, 혹은 오른쪽 서브 트리에서 가장 작은 값**으로 대체 → 대체하다보면 구조가 바뀜

![image](/images/compu/DS/chap11/2.png)

- 삭제 대상을 직접 연결하는 것이 아니라 채우기 위한 연결이다.

```c
// dNode와 pNode는 각각 삭제할 노드와 이의 부모 노드를 가리키는 포인터 변수
if(삭제할 노드가 두 개의 자식 노드를 지닌다)
{
  BTreeNode * mNode = GetRightSubTree(dNode);	// mNode는 대체노드
  BTreeNode * mpNode = dNode		// mpNode는 대체노드의 부모 노드
  ....
  // 1. 삭제 대상의 대체 노드를 찾는다.
  while(GetLeftSubTree(mNode) != NULL)
  {
    mpNode = mNode;
    mNode = GetLeftSubTree(mNode);
  }
  
  // 2. 대체할 노드에 저장된 값을 삭제할 노드에 대합니다
  SetData(dNode, GetData(mNode));
  
  // 3. 대체할 노드의 부모 노드와 자식 노드를 연결한다.
  if(GetLeftSUbTree(mpNode) == mNode)		// 대체할 노드가 왼쪽 자식 노드라면
  {
    // 대체할 노드의 자식 노드를 부모 노드의 왼쪽에 연결
    ChangeLeftSubTree(mpNode, GetLeftSubTree(mNode));		// 자식 노드가 있다면 왼쪽 자식노드 이다.
  }
  else 		// 대체할 노드가 오른쪽 자식 노드라면
  {
    // 대체할 노드의 자식 노드를 부모 노드의 오른쪽에 연결
    ChangeRightSubTree(mpNode, GetRightSubTree(mNode));		// 자식 노드가 있다면 오른쪽노드 이다.
  }
}
```

- 삭제를 위한 이진 트리의 확장

```c
// 왼쪽 자식 노드를 트리에서 제거, 제거된 노드의 주소값 반환
BTreeNode * RemoveLeftSubTree(BTreeNode * bt)
{
  BTreeNode * delNode;
  
  if(bt != NULL)
  {
    delNode = bt->left;
    bt->left = NULL;
  }
  
  return delNode;
}

// 오른쪽 자식 노드를 트리에서 제거, 제거된 노드의 주소값 반환
BTreeNode * RemoveRightSubTree(BTreeNode * bt)
{
  BTreeNode * delNode;
  
  if(bt != NULL)
  {
    delNode = bt->right;
    bt->right = NULL;
  }
  
  return delNode;
}

// 메모리 소멸을 수반하지 않고 main의 왼쪽 자식 노드 변경
void ChangeLeftSubTree(BTreeNode * main, BTreeNode * sub)
{
  main->left = sub;
}

// 메모리 소멸을 수반하지 않고 main의 오른쪽 자식 노드 변경
void ChangeRightSubTree(BTreeNode * main, BTreeNode * sub)
{
  main->right = sub;
}
```

- 이진 탐색 트리 삭제의 완전한 구현
  - 가상 루트 노드 형성 : 삭제할 노드가 루트 노드인 경우의 예외적인 삭제 흐름을 일반화 하기 위함

```c
BTreeNode * BSTRemove(BTreeNode ** pRoot, BSTData target)
{
  // Initialization
  BTreeNode * pVRoot = MakeBTreeNode();
  BTreeNode * pNode = pVRoot;
  BTreeNode * cNode = *pRoot;
  BTreeNode * dNode;
  
  ChangeRightSubTree(pVRoot, *pRoot);
  
  /* 준비 과정 */
  // 삭제 대상 노드 탐색
  // cNode의 부모 노드를 pNode가 가리키게 해야해서 BSTSearch 호출로 대신할 수 없다.
  while(cNode != NULL && GetData(cNode) != target)
  {
    pNode = cNode;
    
    if(target < GetData(cNode))
      cNode = GetLeftSubTree(cNode);
    else
      cNode = GetRightSubTree(cNode);
  }
  
  if(cNode == NULL)			// 삭제 대상이 없다면
    return NULL;
  
  dNode = cNode;		// 삭제 대상을 dNode가 가리킴
  
  /* 구현 */
  // 1. 삭제 대상이 단말 노드인 경우
  if(GetLeftSubTree(dNode) == NULL && GetRightSubTree(dNode) == NULL)
  {
    if(GetLeftSubTree(pNode) == dNode)
      RemoveLeftSubTree(pNode);
    else
      RemoveRightSubTree(pNode);
  }
  
  // 2. 삭제 대상이 하나의 자식 노드를 갖는 경우
  else if (GetLeftSubTree(dNode) == NULL || GetRightSubTree(dNode) == NULL)
  {
    BTreeNode * dcNode;
    
    if(GetLeftSubTree(dNode) != NULL)
      dcNode = GetLeftSubTree(dNode);
    else 
      dcNode = GetRightSubTree(dNode);
    
    if(GetLeftSubTree(pNode) == dNode)
      ChangeLeftSubTree(pNode, dcNode);
    else
      ChangeRightSubTree(pNode, dcNode);
  }
  
  // 3. 두 개의 자식 노드를 모두 갖는 경우
  else
  {
    BTreeNode * mNode = GetRightSubTree(dNode);	// 대체 노드 가리킴
    BTreeNode * mpNode = dNode;		// 대체 노드의 부모 노드 가리킴
    int delData;
    
    // 삭제 대상의 대체 노드를 찾는다.
    while(GetLeftSubTree(mNode) != NULL)
    {
      mpNode = mNode;
      mNode = GetLeftSubTree(mNode);
    }
    
    // 대체 노드에 저장된 값을 삭제할 노드에 대입한다.
    delData = GetData(dNode);
    SetData(dNode, GetData(mNode));
    
    // 대체 노드의 부모 노드와 자식 노드를 연결
    if(GetLeftSubTree(mpNode) == mNode)
      ChangeLeftSubTree(mpNode, GetRightSubTree(mNode));
    else
      ChangeRightSubTree(mpNode, GetRightSubTree(mNode));
    
    dNode = mNode;
    SetData(dNode, delData); 		// 백업 데이터 복원
  }
  
  // 삭제된 노드가 루트 노드인 경우에 대한 추가 처리
  if(GetRightSubTree(pVRoot) != *pRoot)
    *pRoot = GetRightSubTree(pVRoot);		// 루트 노드의 변경 반영
  
  free(pVRoot);			// 가상 루트 노드의 소멸
  return dNode;			// 삭제 대상의 반환
}
```



