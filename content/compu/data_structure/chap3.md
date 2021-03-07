---
title: "Chap3. Linked List 1"
description: "Linked List 1"
menuTitle : "Linked List 1"
weight: 3
date: 2021-03-07T14:35:03+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

## ADT : Abstract Data Type

---

- 추상 자료형(ADT) : 구체적인 기능의 완성과 정을 언급하지 않고, 순수하게 **기능**이 무엇인지를 나열한 것
- **자료형은 기능의 명세이다! 기능 사용 설명서**

ex) int 자료형 : integer를 통해서 가능한 연산(기능)이 나열한 것

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/1.png)

- 구조체 멤버를 밖에서 접근할 필요 없도록 함수적 기능의 구현 필요
- **구조체 : Data + 관련 연산**

⇒ 관련 연산이 함께 정의되어야 함!

- C : [구조체&함수 함께 정의: 자료형] + [함수]

## 배열을 이용한 리스트의 구현

---

### 1. 리스트의 이해

- 리스트의 구분 : 구현 방법 기준, 기본 기능은 동일
  - 순차 리스트 : 배열을 기반으로 구현된 리스트
  - 연결 리스트 : 메모리의 동적 할당을 기반으로 구현된 리스트
- 리스트의 특징
  - 저장 형태 데이터를 나란히(하나의 열로) 저장한다.
  - 저장 특성 중복이 되는 데이터의 저장을 허용한다.

### 2. 리스트 자료구조의 ADT

* 자료구조 특성 & 활용 방안 파악

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/2.png)

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/3.png)

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/4.png)s

- 리스트 초기화
  - C++은 인스턴스 만들지만 C는 따로 해줘야 함
- 데이터 저장
- 저장된 데이터의 탐색 및 초기화
- **연산 결과는 변수 주소값으로 전달하는 것이 보편적**
- LData는 저장 대상의 자료형을 결정할 수 있도록 typedef로 선언된 자료형의 이름이다.

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/5.png)

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/6.png)

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/7.png)

- 다음 데이터의 참조를 목적으로 호출

- 바로 이전 참조가 이루어진 데이터 삭제

- 현재 저장된 데이터 수 반환

### 3. 리스트의 데이터 참조 과정

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/8.png)

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/9.png)

- 주소값을 참조 함수에 넣고 반환된 T/F로 나옴에 따라서 이후 다음 참조

- 데이터 삭제는 데이터 참조 과정이 먼저 필요!
- 프로그램 구현에 있어서 헤더파일을 조정하는 것이 바람직하며, 특별한 경우가 아니라면 소스 파일을 건드리는 것은 없어야 한다.

### 4. 배열 기반 리스트의 초기화

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/10.png)

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/11.png)

### 5. 배열 기반 리스트의 조회

- 0 : 저장된 데이터 없음
- -1 : 참조 위치 없음
- 예외처리 추가
- numOfData로 배열 indexing
- data 수 증가

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/12.png)

- 중간  지점에서 다시 처음부터 참조하기 원할 수 있으므로 LFirst 따로 참조
- curPosition 변화를 통해서 indexing
- 값의 반환은 매개변수를 통해서! 함수의 반환은 성공여부를 알리기 위해서!

### 6. 배열 기반 리스트의 삭제

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/13.png)

- **삭제 되는 데이터는 반환의 과정을 통해서 되돌려 주어야 한다!**
- 삭제 시 빈 공간을 채워 주어야 하며, 참조 위치를 되돌려 indexing시 빠지지 않도록

### 7. 리스트에 int 대신 구조체 변수 저장하기

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap3/14.png)



### 8. PointListMain.c의 일부

```cpp
int main(void)
{
		. . . . . .
		/*** xpos가 2인 모든 데이터 삭제 ***/
		compPos.xpos=2;
		compPos.ypos=0;

		if(LFirst(&list, &ppos)) {
				if(PointComp(ppos, &compPos)==1) {
						ppos=LRemove(&list);   // 메모리 공간 삭제는 따로 하고, 주소값을 반환
						free(ppos);            // 반환한 주소값을 할당 해제
				}
				while(LNext(&list, &ppos)) {
						if(PointComp(ppos, &compPos)==1) {
									ppos=LRemove(&list);
									free(ppos);
				}
		}
		. . . . . .
}
```

### 9. 배열 기반 리스트의 장점과 단점

- 배열 기반 리스트의 단점
  - 배열의 길이가 초기에 결정되어야 핚다. 변경이 불가능하다.
  - 삭제의 과정에서 데이터의 이동(복사)가 매우 빈번히 일어난다.
- 배열 기반 리스트의 장점
  - 데이터 참조가 쉽다. 인덱스 값 기준으로 어디든 핚 번에 참조 가능!