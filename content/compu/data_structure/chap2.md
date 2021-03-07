---
title: "Chap2. 배열"
description: "Array"
menuTitle : "Array"
weight: 2
date: 2021-03-07T14:35:01+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---



## 1. 배열

- 배열은 크기와 성격이 같은 자료형을 연속된 기억 장소에 저장한 것이다.
- 배열 구조는 선형 리스트(linear list) 또는 순서 리스트(ordered list)를 표현하는 가장 일반적인 형태이다.
- 일반적으로 배열은 배열명(Array name)으로 식별되고, 배열의 요소(원소)는 배열명에 괄호를 붙여 그 안의 첨자(subscript Or index)로 식별한다.

- 1차원 배열 (One-dimensional Array Or vector) A는 A(L:U)로 표시할 수 있다.
- 이 때, L은 배열 첨자의 하한 값으로 배열의 시작 값을 의미하고, U는 상한 값으로 배열의 마지막 값을 의미한다.
- 1차원 배열의 길이는 배열을 구성하는 배열 원소의 개수로 A(L:U)의 길이는 U-L+1이다.
- 2차원 배열(Two-dimensional Array Or matrix) A는 A(L1:U1, L2:U2)로 표현할 수 있다.
- 2차원 배열A의 원소는 일반적으로 A(i,j) 형태로 표시하는데 i는 행을, j는 열을 의미한다.
- 2차원 배열 A(L1:U1, L2:U2)에서 행을 구성하는 원소의 개수는 U1-L1+1, 열을 구성하는 원소의 개수는 U2-L2+1이고, 전체 원소의 개수는 (U1-L1+1)*(U2-L2+1)이다.

- 3차원 배열(Three-dimensional Array)는 3개의 첨자를 사용하여 표현하며, 각각의 첨자를 면(plane), 행(row), 열(column)으로 구분한다.
- 3차원 배열A의 원소는 일반적으로 A(i,j,k) 형태로 표시하는데 i는 면을, j는 행을, k는 열을 의미한다.
- 3차원 배열 A를 A(L1:U1, L2:U2, L3:U3)라 표현하면,
- 면을 구성하는 원소의 개수는  U1-L1+1, 행을 구성하는 원소의 개수는  U2-L2+1, 열을 구성하는 원소의 개수는  U3-L3+1이다.

- 이런식으로 4차 이상의 배열도 충분히 만들 수 있으며, 표현도 동일하게 할 수 있다.
- n차원 배열의 전체 원소 개수는 $\displaystyle\prod_{k=1}^n (U_k-L_k+1)$ 이다.