---
title: "Chap13. Table & Hash*"
description: "Table & Hash"
menuTitle : "Table & Hash"
weight: 13
date: 2021-03-07T14:35:27+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

### 13.1 빠른 탐색을 보이는 해쉬 테이블

---

##### 테이블 자료구조

- **데이터가 key와 value로 한 쌍을 이루며, key가 데이터의 저장 및 탐색의 도구가 된다.**
- **테이블 자료구조에서는 원하는 데이터를 단번에 찾을 수 있다.**
- key 역시 의미있는 데이터로 정의하는 것이 좋다.
  - *데이터에 근거하여 생성되어야 하며 중복되면 안된다.*
- 테이블 자료구조의 탐색 연산은 O(1)의 시간복잡도를 보인다.
- 사전구조 혹은 맵(map)이라고 불린다.

##### 배열 기반 테이블

```c
typedef struct _empInfo
{
  int empNum;		// 고유번호 : key
  int age;			// 나이 : value
} Empinfo;


int main()
{
  EmpInfo empInfoArr[1000];
  EmpInfo ei;
  int eNum;
  
  printf("Enter empnum & age: ");
  scanf("%d %d", &(ei.empNum), &(ei.age));
  empInfoArr[ei.empNum] = ei; // key가 배열의 idx로 활용됨
  
  printf("Enter empnum you want to look: ");
  scanf("%d", &eNum);
  
  ei = empInfoArr[eNum];		// 단번에 탐색
  printf("empnum %d, age %d \n", ei.empNum, ei.age);
  
  return 0;
}
```

→ 테이블의 개념적인 이해를 도우나, 해쉬의 개념이 빠져 효율적인 테이블이라고 볼 수 없다.

```c
// hash : key를 유효한 주소값으로 연결
int GetHashValue(int empNum)
{
  return empNum % 100;
}

int main()
{
  EmpInfo empInfoArr[100];
  
  EmpInfo emp1 = {20120003, 42};
  EmpInfo emp2 = {20130012, 33};
  EmpInfo emp3 = {20170049, 27};
  
  EmpInfo r1, r2, r3;
  
  // 키를 인덱스 값으로 이용해서 저장
  empInfoArr[GetHashValue(emp1.empNum)] = emp1;
  empInfoArr[GetHashValue(emp2.empNum)] = emp2;
  empInfoArr[GetHashValue(emp3.empNum)] = emp3;
  
  // 키를 인덱스 값으로 이용해서 탐색
  r1 = empInfoArr[GetHashValue(20120003)];
  r2 = empInfoArr[GetHashValue(20130012)];
  r3 = empInfoArr[GetHashValue(20170049)];
  
  printf("empnum: %d, age: %d \n", r1.empNum, r1.age);
    printf("empnum: %d, age: %d \n", r2.empNum, r2.age);
    printf("empnum: %d, age: %d \n", r3.empNum, r3.age);
    
  return 0;
}
```

- 해쉬 함수를 통해 합리적인 메모리 할당을 도울 수 있으나, 다른 데이터가 해쉬값이 같아 충돌이 발생 가능
- 예제 1

```c
// 해쉬 테이블 예제
typedef struct _person
{
  int ssn;			// 주민번호
  char name[STR_LEN];		// 이름
  char addr[STR_LEN];		// 주소
} Person;

int GetSSN(Person * p)
{
  return p->ssn;		// key 반환
}
void ShowPerInfo(Person * p)
{
  printf("ssn : %d \n", p->ssn);
  printf("name : %s \n", p->name);
  printf("addr : %s \n", p->addr);
}

Person * MakePersonData(int ssn, char * name, char * addr)
{
  Person * newP = (Person*)malloc(sizeof(Person));
  newP->ssn = ssn;
  strcpy(newP->name, name);
  strcpy(newP->addr, addr);
  
  return newP;
}
```

- 예제 2

```c
// 해쉬 예제 2
typedef int Key;		// ssn
typedef Person * Value;		// slot에는 구조체 변수 주소값으로 저장될 것

enum SlotStatus {EMPTY, DELETED, INUSE};

typedef struct _slot
{
  Key key;
  Value val;
  enum SlotStatus status;
} Slot;		// 하나의 key & value를 저장할 공간
```

 - 슬롯의 상태 : 충돌 문제의 해결 감안
   	- EMPTY : 이 슬롯에는 데이터가 저장된 바 없다.
   	- DELETED : 이 슬롯에는 데이터가 저장된 바 있으나 현재는 비워진 상태이다.
   	- INUSE : 이 슬롯에는 현재 유효한 데이터가 저장되어 있다.

##### 해쉬 테이블의 헤더파일과 helper 함수

- **해쉬 함수는 등록 또는 변경이 가능하도록 정의하는 것이 좋으며, 일반적으로 삽입, 삭제, 탐색의 과정에서 키를 별도로 전달하도록 함수가 정의됨**

```c
typedef int HashFunc(key k);

typedef struct _table
{
  Slot tbl[MAX_TBL];
  HashFunc * hf;		// 해쉬함수 등록
} Table;

// 테이블 초기화
void TBLInit(Table * pt, HashFunc * f)
{
  int i;
  
  // 모든 슬롯 초기화
  for(i=0; i<MAX_TBL; i++)
    (pt->tbl[i]).status = EMPTY;
  
  pt->hf = f;		// 해쉬 함수 등록
}

// 테이블에 키와 값을 저장
void TBLInsert(Table * pt, Key k, Value v)
{
  int hv = pt->hf(k);		// 해쉬 값을 얻는다. 이후 인덱스로 사용
  pt->tbl[hv].val = v;	
  pt->tbl[hv].key = k;
  pt->tbl[hv].status = INUSE;
}

// 키를 근거로 테이블에서 데이터 삭제
Value TBLDelete(Table * pt, Key k)
{
	int hv = pt->hf(k);		// 해쉬 값을 얻는다. 이후 인덱스로 사용
  
  if((pt->tbl[hv]).status != INUSE )
  {
    return NULL;
  }
  else
  {
    (pt->tbl[hv]).status = DELETED;
    return (pt->tbl[hv]).val;	// 삭제되는 데이터 반환
  }
}

// 키를 근거로 테이블에서 데이터 탐색
Value TBLSearch(Table * pt, Key k)
{
  int hv = pt->hf(k);		// 해쉬 값을 얻는다. 이후 인덱스로 사용
  
  if((pt->tbl[hv]).status != INUSE)
    return NULL;
  else
    return (pt->tbl[hv]).val;	// 탐색대상 반환
}
```

```c
int MyHashFunc(int k)
{
  return k % 100;
}

int main()
{
  Table myTbl;
  Person * np, * sp, * rp;
  
  TBLInit(&myTbl, MyHashFunc);
  
  // data insert
  np = MakePersonData(20120003, "Lee", "Seoul");
  TBLInsert(&myTbl, GetSSN(np), np);
  
  np = MakePersonData(20130012, "Kim", "Jeju");
  TBLInsert(&myTbl, GetSSN(np), np);
  
  np = MakePersonData(20170049, "Han", "Kangwon");
  TBLInsert(&myTbl, GetSSN(np), np);
  
  // data search
  sp = TBLSearch(&myTbl, 20120003);
  if(sp != NULL)
    ShowPerInfo(sp);
  
  sp = TBLSearch(&myTbl, 20130012);
  if(sp != NULL)
    ShowPerInfo(sp);
  
  sp = TBLSearch(&myTbl, 20170049);
  if(sp != NULL)
    ShowPerInfo(sp);
  
  // data delete
  rp = TBLDelete(&myTbl, 20120003);
  if(rp != NULL)
    free(rp);
  
  rp = TBLDelete(&myTbl, 20130012);
  if(rp != NULL)
    free(rp);
  
  rp = TBLDelete(&myTbl, 20170049);
  if(rp != NULL)
    free(rp);
  
  return 0;
}
```

##### 좋은 해쉬함수의 조건

- Default hash function을 너무 믿지 마라
- **해쉬 결과의 데이터의 저장 위치가 적당히 분산되어 있어야 함**
- **좋은 해쉬 함수는 키의 일부분을 참조하여 해쉬값을 만들지 않고, 키 전체를 참조하여 해쉬 값을 만들어 낸다.**
- 자릿수 선택 방법 or 자릿수 폴딩 방법 etc.....



### 13.2 충돌 문제의 해결책

---

##### 선형 조사법 (Linear Probing)

- 1차 저장 후 같은 해쉬 결과를 가지는 값이 만들어지면 충돌이 일어난다.
- 이 결과 바로 다음 오른쪽 자리에 저장 된다.

$$f(k)+1 \rightarrow f(k)+2 \rightarrow f(k)+3 \rightarrow f(k)+4\quad .... $$

- 단순하지만 충돌의 횟수가 증가함에 따라서 클러스터 현상(특정 영역에 데이터가 몰리는 현상)이 발생한다

##### 이차 조사법

- 선형 조사법 보다 멀리서 빈자리를 찾는다.

$$f(k)+1^2 \rightarrow f(k)+2^2 \rightarrow f(k)+3^2 \rightarrow f(k)+4^2\quad .... $$

- DELETED : 유효한 데이터가 있었고 충돌이 가능함을 암시 → 오른쪽 봐야함 암시, 동일한 해쉬 값의 데이터 저장을 의심

##### 이중 해쉬

- 두 개의 해쉬 함수를 활용

- 해쉬 값이 같으면, 충돌 시, 빈 슬롯을 찾기 위한 접근 위치가 늘 동일하다는 문제점을 해결
  - 1차 해쉬 함수 : 위치 찾을 때 사용 →  \\(h1(k) = k % 15\\), 배열 길이가 15인 경우
  - 2차 해쉬 함수 : 충돌시 얼마나 떨어진 곳의 빈자리 찾을지 결정 → \\(h2(k)=1+(k%c)\\), 15보다 작은 소수 c를 결정 ⇒ 빈 자리를 찾는 위치는 달라지게 된다.
    - +1 : 2차 해쉬 값이 0이 되는 것 방지
    - c<15 : 배열의 길이가 15이므로
    - c = 소수 : 클러스터 현상을 낮춘다는 통계 근거

##### 체이닝 (닫힌 어드레싱 모델)

- 한 해쉬값에 다수의 데이터를 저장할 수 있도록 배열을 2차원의 형태로 선언하는 모델
- 한 해쉬값에 다수의 데이터를 저장할 수 있도록 각 해쉬 값 별로 연결리스트를 구성하는 모델

```c
typedef int Key;			// ssn
typedef Person * Value;

// enum SlotStatus {EMPTY, DELETED, INUSE}; 체이닝 기반에서는 충돌시 다른 위치에 저장하므로 슬롯의 상태정보를 유지하지 않아도 된다.

typedef struct _slot
{
  Key key;
  Value Val;
  // enum SlotStatus status;
} Slot;

typedef int HashFunc(Key k);

typedef struct _table
{
  Slot tbl[MAX_TBL];		// List tbl[MAX_TBL] 가능
  HashFunc * hf;
}

// 테이블 초기화
void TBLInit(Table * pt, HashFunc * f)
{
  int i;
  
  for(i=0; i<MAX_TBL; i++)
    ListInit(&(pt->tbl[i]));
  
  pt->hf = f;
}

// 테이블 키와 값 저장
void TBLInsert(Tablt * pt, Key k, Value v)
{
  int hv = pt->hf(k);
  Slot ns = {k, v};
  
  if(TBLSearch(pt, k) != NULL) // 키 중복시
  {
    printf("키 중복 \ㅜ");
    return ;
  }
  else
  {
    LInsert(&(pt->tbl[hv]), ns);	// 해쉬 값 기반 삽입
  }
}

// 키를 근거로 테이블에서 데이터 삭제
Value TBLDelete(Table * pt, Key k)
{
  int hv = pt->hf(k);
  Slot cSlot;
  
  if(LFirst(&(pt->tbl[hv]), &cSlot))
  {
    if(cSlot.key == k)
    {
      LRemove(&(pt->tbl[hv]));
      return cSlot.val;
    }
    else
    {
      while(LNext(&(pt->tbl[hv]), &cSlot))
      {
        if(cSlot.key == k)
        {
          LRemove(&(pt->tbl[hv]));
          return cSlot.val;
        }
      }
    }
  }
  
  return NULL;
}

// 키를 근거로 테이블에서 데이터 탐색
Value TBLSearch(Table * pt, Key k)
{
  int hv = pt->hf(k);
  Slot cSlot;
  
  if(LFirst(&(pt->tbl[hv]), &cSlot))
  {
    if(cSlot.key == k)
    {
      return cSlot.val;
    }
    else
    {
      while(LNext(&(pt->tbl[hv]), &cSlot))
      {
        if(cSlot.key == k)
          return cSlot.val;
      }
    }
  }
  
  return NULL;
}
```

