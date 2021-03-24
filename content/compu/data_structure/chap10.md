---
title: "Chap10. Sort"
description: "Sort"
menuTitle : "Sort"
weight: 10
date: 2021-03-07T14:35:22+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- 3대 연산 : 삽입, 삭제, 탐색
- 정렬은 탐색을 이야기 하기 위함 ⇒ 탐색 전에 정렬이 필요!

### 10.1 단순한 정렬 알고리즘

---

##### 버블 정렬 (Bubble Sort)

```c
void BubbleSort(int arr[], int n)
{
  int i, j;
  int temp;
  
  for(i=0; i<n-1; i++)
  {
    for(j=0; j<(n-i)-1; j++)
    {
      if(arr[j] > arr[j+1])
      {
        // 데이터의 교환
        temp = arr[j];
        arr[j] = arr[j+1];
        arr[j+1] = temp;
      }
    }
  }
}
```

```c
int main()
{
  int arr[4] = {3,2,4,1};
  int i;
  
  BubbleSort(arr, sizeof(arr)/sizeof(int));
  
  for(i=0; i<4; i++)
    printf("%d ", arr[i]);
  
  printf("\n");
  return 0;
}
```

- 성능평가
  - 비교 횟수 : 두 데이터간의 비교 연산 횟수
  - 이동 횟수 : 위치의 변경을 위한 데이터 이동 횟수

$$(n-1)+...+2+1 \Rightarrow \displaystyleΣ^{n-1}_{i=1}i=\frac{n(n-1)}{2}=\frac{n^2-n}{2}\Rightarrow O(n^2) $$

- 최악의 경우에는 비교의 횟수와 이동의 횟수는 일치한다.



##### 선택 정렬(Selection Sort)

- 하나씩 선택해서 정렬 결과를 완성 → *별도의 메모리 공간이 요구됨* ⇒ 실제 구현시에는 별도 공간을 만들지 말고 하나 옮기면 빈 공간 이용

```c
void SelSort(int arr[], int n)
{
  int i,j;
  int maxIdx;
  int temp;
  
  for(i=0; i<n-1; i++)
  {
    maxIdx = 1;
    
    for(j=i+1; j<n; j++)    // 최소값 탐색
    {
      if(arr[j] < arr[maxIdx])   // 비교 연산
        maxIdx = j;
    }
    
    // 교환 -> 이동 연산
    temp = arr[i];
    arr[i] = arr[maxIdx];
    arr[maxIdx] = temp;
  }
}

int main()
{
  int arr[4] = {3,4,2,1};
  int i;
  
  SelSort(arr, sizeof(arr)/sizeof(int));
  
  for(i=0; i<4; i++)
    printf("%d ", arr[i]);
  
  printf("\n");
  return 0;
}
```

- 성능평가

  $$(n-1)+(n-2)+...+2+1 \Rightarrow Σ^{n-1}_{i=1}=\frac{n(n-1)}{2}=\frac{n^2-n}{2} \Rightarrow O(n^2) $$

  → 바깥 for 문에 있으므로 데이터 이동의 횟수는 동일하다

##### 삽입 정렬 (Insertion Sort)

![image](/images/compu/DS/chap10/1.png)

- 정렬이 완료된 영역과 그렇지 않은 영역을 구분하고 점진적으로 정렬 영역을 넓혀감
  - 어느 위치로 가느냐에 따라 이동해야 할 거리가 달라진다.

![image](/images/compu/DS/chap10/2.png)

- 위치와 이동을 함께 구현하면 성능이 좋지 않아진다.
- 대소 비교 후 해당 위치를 찾고 바로 이동하도록 구현한다.

```c
void InsertSort(int arr[], int n)
{
  int i, j;
  int insData;
  
  for(i=1; i<n; i++) // idx 0 데이터는 이미 정렬 됬으므로 i=1
  {
    insData = arr[i];
    
    for(j=i-1; j>=0; j--)
    {
      if(arr[j] > insData)
        arr[j+1] = arr[j];  // 비교 대상 한 칸 밀기
      else
        break;	// 적정 위치 찾았으므로 빠져나감
    }
    arr[j+1] = insData; // 찾은 위치에 정렬 대상 삽입
  }
}

int main()
{
  int arr[5] = {5,3,2,4,1};
  int i;
  
  InsertSort(arr, sizeof(arr)/sizeof(int));
  
  for(i=0; i<5; i++)
    printf("%d ", arr[i]);
  
  printf("\n");
  return 0;
}
```

- 성능 평가
  - 비교 및 이동 연산 모두 안쪽 for문에 존재하므로 최악의 경우는 \\(O(n^2)\\)



### 10.2 복잡하지만 효율적인 정렬 알고리즘

---

##### 힙 정렬 (Heap Sort)

- 힙의 특성을 활용하여 힙에 정렬할 대상을 모두 넣었다가 다시 꺼내어 정렬을 진행한다.

```c
int PriComp(int n1, int n2)
{
  return n2-n1;     // 오름차순 정렬
  // return n1-n2;
}

void HeapSort(int arr[], int n, PriorityComp pc)   // n: 길이
{
  Heap heap;
  int i;
  
  HeapInit(&heap,pc);
  
  // 정렬 대상으로 힙 구성
  for(i=0; i<n; i++)
    HInsert(&heap, arr[i]);
  
  // 순서대로 하나씩 꺼내서 정렬 완성
  for(i=0; i<n; i++)
    arr[i] = HDelete(&heap);
}

int main()
{
  int arr[4] = {3,4,2,1};
  int i;
  
  HeapSort(arr, sizeof(arr)/sizeof(int), PriComp);
  
  for(i=0; i<4; i++)
    printf("%d ", arr[i]);
  
  printf("\n");
  return 0;
}
```

- 성능 평가

  - 하나의 데이터 → 저장 시간 복잡도 \\(O(\log_2n)\\) & 삭제 시간복잡도 \\(O(\log_2n)\\)

    ⇒ n개의 데이터 : \\(O(n\log_2n)\\)

##### 병합 정렬 (Merge Sort) : Divide And Conquer

> 1. 분할 (Divide) : 해결이 용이한 단계까지 문제를 분할
> 2. 정복 (Conquer) : 분할된 문제 해결
> 3. 결합 (Combine) : 해결한 결과를 결합하여 마무리

![image](/images/compu/DS/chap10/3.png)

- 분할 과정 : 재귀적, 구분 될 때 까지만 분할 진행
- 병합 과정 : 핵심 연산, 재귀적으로 조금씩 병합해나감

```c
void MergeSort(int arr[], int left, int right)
{
  int mid;
  
  if(left < right) // left가 더 작음 : 더 나눌 수 있음
  {
    // 중간지점 계산
    mid = (left+right)/2;
    
    // 둘로 나눠서 각각 정렬
    MergeSort(arr, left, mid); // left ~ mid 정렬
    MergeSort(arr, mid+1, right) // mid+1 ~ right 정렬
      
    // 정렬된 두 배열 병합
    MergeTwoArray(arr, left, mid, right);
  }
}

void MergeTwoArray(int arr[], int left, int mid, int right)
{
  int fIdx = left; int rIdx = mid+1; int i;
  int * sortArr = (int*)malloc(sizeof(int) * (right+1));  // 병합 결과를 담을 메모리 공간 할당 
  // -> 내부에 데이터를 옮기기 번거로워 임시로 공간 할당
  int sIdx = left;
  
  // 병합 할 두 영역의 데이터를 비교하여 sortArr에 담음
  while(fIdx <= mid && rIdx <= right)
  {
    if(arr[fIdx] <= arr[rIdx])
      sortArr[sIdx] = arr[fIdx++];
    else
      sortArr[sIdx] = arr[rIdx++];
    sIdx++;
  }
  
  // 배열의 앞 부분이 sortArr로 모두 이동되어서 
  // 배열 뒷부분에 남은 데이터를 모두 sortArr로 이동
  if(fIdx > mid)
  {
    for(i=rIdx; i<=right; i++, sIdx++)
      sortArr[sIdx] = arr[i];
  }
  // 배열의 뒷 부분이 sortArr로 모두 이동되어서 
  // 배열 앞부분에 남은 데이터를 모두 sortArr로 이동
  else
  {
    for(i=fIdx; i <= mid; i++, sIdx++)
      sortArr[sIdx] = arr[i];
  }
  // 병합 결과를 옮겨 담는다.
  for(i=left; i<=right; i++)
    arr[i] = sortArr[i];
  
  free(sortArr);
}
```

- 성능 평가

  - 데이터 비교 및 이동은 MergeTwoArea 함수 중심으로 진행
  - 비교 연산

  "정렬 대상 데이터 수가 n개 일때, 각 병합 단계마다 최대 n번의 비교 연산 진행" → \\(n\log_2n\\) → \\(n\log_2n\\)

  - 이동 연산

  병합 한번 & 저장된 데이터 옮기는 과정 한번 \\(2n\log_2n \rightarrow n\log_2n\\)



##### 퀵 정렬(Quick Sort)

- 1) 초기화 
  - left : 정렬 대상 가장 왼쪽 지점
  - right : 정렬 대상 가장 오른쪽 지점
  - pivot : 중심점 
  - low : 피벗을 제외한 가장 왼쪽
  - high : 피벗을 제외한 가장 오른쪽
- 2) low & high 이동
  - low의 오른쪽 이동 : 피벗보다 정렬의 우선순위가 낮은 데이터를 만날 때 까지
  - high의 왼쪽 이동 : 피벗보다 정렬의 우선순위가 높은 데이터를 만날 때 까지
- 3) low & high 교환

![image](/images/compu/DS/chap10/4.png)

- 4) pivot의 이동
  - 왼쪽은 pivot 보다 높은 우선순위 보장
  - 오른쪽은 pivot 보다 낮은 우선순위 보장 
  - 역전 시에는 pivot과 high 교환
  - 영역 나누어 반복 실행
  - left > right ⇒ 쪼갤 영역이 없는 상태

![image](/images/compu/DS/chap10/5.png)

```c
int Partition(int arr[], int left, int right)
{
  int pivot = arr[left];
  int low = pivot +1;
  int high = right;
  
  while(low <= high)
  {
    // 피벗보다 큰 값을 찾는 과정
    while(pivot > arr[low]) // 우선순위 낮은 것 만날때까지
      low++;
    
    // 피벗보다 작은 값을 찾는 과정
    while(pivot < arr[high]) // 우선순위 높은 것 만날 때까지
      high--;
    
    // 교차되지 않는다면 swap
    if(low <= high)
      Swap(arr, low, high);
  }
  Swap(arr, left, high);	// 피벗과 high의 교환
  return high		// 옮겨진 피벗의 위치정보 반환
}
```

- 재귀적 완성과 수정

```c
void QuickSort(int arr[], int left, int right)
{
  if(left <= right)
  {
    int pivot = Partition(arr, left, right);
    QuickSort(arr, left, pivot-1); // 왼쪽 영역 정렬
    QuickSort(arr, pivot+1, right); // 오른쪽 영역 정렬
  }
}
```

- arr={3,3,3}의 경우

```c
int Partition(int arr[], int left, int right)
{
  ...
  while(low <= high)
  {
    // while(pivot > arr[low]), 정렬의 범위를 넘지 않도록 경계 검사
    while(pivot >= arr[low] && low <= right)
      low++;
    // while(pivot < arr[high]), 정렬 범위 넘지 않도록 경계 검사
    while(pivot <= arr[high] && high >= (left+1))
      high--;
  }
}
```

- **데이터가 불규칙적으로 나열되어 있고 피벗이 중간에 해당하는 값에 가깝게 선택이 될 수록 최상의 경우가 보임** → 불규칙적 나열시 아무거나 선택해도 중간을 선택할 확률이 높다.
- 중간에 가까운 값으로 빅-오를 선택하려는 노력을 조금만 하더라도 퀵정렬은 최악의 경우를 만들지 않는다. \\(\Rightarrow O(n\log_2n)\\)

##### 기수 정렬 (Radix Sort)

> - 정렬순서의 앞서고 뒤섬을 비교하지 않는다. 
> - 정렬 알고리즘의 한계로 알려진 \\(O(n\log_2n)\\)을 뛰어 넘을 수 있다. 
> - 적용할 수 있는 대상이 매우 제한적이다. **길이가 동일한 데이터들**의 정렬에 용이하다! 

![image](/images/compu/DS/chap10/6.png)

- 자리를 찾기 위한 비교연산이 필요할 수 있으나, 버킷을 배열로 만들면 비교 불필요
  - 기수(radix) : 주어진 데이터를 구성하는 기본 요소
  - 버킷(bucket) : 기수의 수에 해당하는 만큼의 버킷 활용
- LSD : Least Significant Digit를 시작으로 정렬 진행
  - LSD 먼저 연산 : "앞 자리가 값이 같다면" 이라는 가정 ⇒ 다음 연산에서 이전 연산 아랫자리 비교 결과 순서는 바꾸지 않는다.

![image](/images/compu/DS/chap10/7.png)

- MSD : Most Significant Digit
  - 정렬의 기준 선정 방향 반대
  - 점진적으로 정렬이 완성되어가므로, 중간 중간에 정렬이 완료된 데이터는 더 이상 정렬과정은 진행하지 않아야 함 (**선별 과정에서 연산 낭비 증가**)

```c
#include "ListBaseQueue.h"
#define BUCKET_NUM	10

void RadixSort(int arr[], int num, int maxLen)
{
  Queue buckets[BUCKET_NUM];
  int bi, pos, id, divfac=1, radix;
  
  // 총 10개 버킷 초기화
  for(bi=0; bi<BUCKET_NUM; bi++)
    QueueInit(&buckets[bi]);
  
  // 가장 긴 데이터 길이만큼 반복
  for(pos=0; pos<maxLen; pos++)
  {
    // 정렬 대상의 수만큼 반복
    for(di=0; di<num; di++)
    {
      // N번째 자리의 숫자 추출
      radix = (arr[di] / divfac) % 10;
      // 추출한 숫자를 근거로 버킷에 데이터 저장
      Enqueue(&buckets[radix], arr[di]);
    }
    
    // 버킷 수만큼 반복
    for(bi=0, di=0; bi<BUCKET_NUM; bi++)
    {
      // 버킷 저장된 것 순서대로 다 꺼내서 arr에 저장
      while(!QIsEmpty(&buckets[bi]))
      {
        arr[di++] = Dequeue(&buckets[bi]); // FIFO
      }
    }
    
    // N번째 자리 추출 : 1 -> 10 -> 100
    divfac *= 10;
  }
}
```

- 성능평가

  - 버킷으로의 데이터 삽입과 추출을 근거로 빅-오 결정

  - 삽입과 추출 연산을 한 쌍으로 묶으면

    $$ data 길이 \times data\# =O(l\cdot n)$$

⇒ 보편적으로 퀵정렬 → 적용가능시 기수 정렬