---
title: "Dynamic Programming"
description: "Dynamic Programming"
menuTitle : "DP"
weight: 1
date: 2021-11-09T23:17:00+09:00
draft: false
katex: true
markup: mmark
tags: ["algorithm"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

# Dynamic Programming



## Part 1

---

### Motivation

- 피보나치 수열 :  Recursion 사용 시 많은 계산이 중복됨
  - f(7) 계산을 위해서는 f(6), f(5) .... 다 계산해야 함

```c
int fib(int n)
{
	if (n==1 || n==2)
		return 1;
	else
		return fib(n-2) + fib(n-1);
}
```



- Memoization : 중간 결과를 caching 함으로써 중복 계산을 피함, 이미 계산된 결과를 기억해 둠, Top-down, 실제로 필요한 subproblem만 품

  ```c
  int fib(int n)
  {
    if (n==1 || n==2)
      return 1;
    else if (f[n] > -1) // 배열 f가 -1으로 초기화되어 있다고 가정
      return f[n];    // 이미 계산된 값
    else
    {
      f[n] = fib(n-2) + fib(n-1); // 중간 결과 버리지 않고 caching
      return f[n];
    }
  }
  ```

  

- Bottom-up 방식 : 초반 계산이 이미 이루어져 있음, recursion에 수반되는 overhead가 없다

```c
int fib(int n)
{
  f[1] = f[2]=1;
  for (int i=3; i<=n; i++)
    f[n] = f[n-1] + f[n-2];
  return f[n];  
}
```



- ex) Binomial Coefficient

  ```c
  int binomial(int n, int k)
  {
    if (n==k || k==0)
      return 1;
    else
      return binomial(n-1, k) + binomical(n-1, k-1);
  }
  ```

  - recursion : base가 있고 결국 해당 base에 도착하는 조건

→ n & k에 의해 정해지므로 배열은 2D 형태

```c
int binomial(int n, int k)
{
  if (n==k || k==0)
    return 1;
  else if (binom[n][k]>-1)  // 배열 binom이 -1로 초기화 되어 있다고 가정
    return binom[n][k]
  else
  {
    binom[n][k] = binmocial(n-1,k) + binomial(n-1, k-1);
    return binom[n][k];
  }
}

// bottom-up 
int binomial(int n, int k)
{
  for(int i=0;i<=n;i++)
  {
    for (int j=0; j<=k && j<=i; j++)
    {
      if(k==0 || n==k)
        binom[i][j] = 1;
      else
        binom[i][j] = binom[i-1][j-1] + binom[i-1][j];
    }
  }
  return binom[n][k];
}
```



## 예시 : 행렬 경로 문제

---

- 정수들이 저장된 nxn행렬의 좌상단에서 우하단까지 이동한다. 단, 오른쪽이나 아래쪽 방향으로만 이동할 수 있다.
- 방문한 칸에 있는 정수들의 합이 최소화 되도록 하라

- 동적 계획법 : **순환식을 세우고** 계산하는 것, general case와 exception case를 완전하게 구하라

- Key observation : (i,j)에 도달하기 위해서는 (i,j-1) 혹은 (i-1, j)를 거쳐야 한다. 또한 (i,j-1) 혹은 (i-1, j)까지는 최선의 방법으로 이동해야 한다.
  $$
  L[i,j]:(1,1)에서\ (i,j)까지 이르는 최소 합\\
  L[i,j]=\begin{cases} m_{ij} & if\ i=1\ \&j=1;\\L[i-1,j]+m_{ij}\ & if\ j=1; \\ L[i,j-1]+m_{ij} & if\ i=1; \\ min(L[i-1, j],L[i,j-1])+m_{ij} & otherwise.\end{cases}
  $$

  - i=1 : 맨 아래 줄일 경우, 그냥 왼쪽을 구하기
  - j=1 : 맨 오른쪽 일 경우, 그냥 위쪽을 구하기

  ```c
  int mat(int i, int j)  // recursive algorithm, 모든 경우 계산하므로 계산량이 너무 많음
  {
  	if(i==1 && j==1)
  		return m[i][j];
  	else if (i==1)
  		return mat(1,j-1) + m[i][j];
  	else if (j==1)
  		return mat(i-1, 1) + m[i][j];
  	else
  		return Math.min(mat(i-1,j), mat(i,j-1))+m[i,j]
  }
  
  int mat(int i, int j)  // Memoization
  {
    if (L[i][j] != -1) return L[i][j];
  	if(i==1 && j==1)
  		return L[i][j] = m[i][j];
  	else if (i==1)
  		return L[i][j] = mat(1,j-1) + m[i][j];
  	else if (j==1)
  		return L[i][j] = mat(i-1, 1) + m[i][j];
  	else
  		return L[i][j] = Math.min(mat(i-1,j), mat(i,j-1))+m[i,j]
     return L[i][j];
  }
  ```

- 해당 지점의 위나 왼쪽먼저 계산하도록 순서가 되면 필요한 값이 항상 먼저 계산됨

  ```c
  int mat() // bottom-up
  {
    for (int i=1; i<=n; i++)
    {
      for (int j=1; j<=n; j++)
      {
        if (i==1 && j==1)
          L[i][j] = m[1][1];
        else if (i==1)
          L[i][j] = m[i][j] + L[i][j-1];
        else if (j==1)
          L[i][j] = m[i][j] + L[i][j-1];
        else
          L[i][j] = m[i][j] + Math.min(L[i-1][j], L[i][j-1]);
      }
    }
    return L[n][n];
  }
  
  // initialize L with L[0][j]=L[i][0]=inf. for all i & j
  int mat()
  {
    for (int i=1; i<=n; i++)
    {
      for (int j=1; j<=n; j++)
      {
        if (i==1 && j==1)
          L[i][j] = m[1][1];
        else
          L[i][j] = m[i][j] + Math.min(L[i-1][j], L[i][j-1]);
      }
    }
    return L[n][n];
  }
  ```

- 경로 구하기 : 경로 배열 저장, 왼쪽이나 위쪽의 것 중에 작은 것에 대한 방향 저장

  ```c
  // initialize L with L[0][j]=L[i][0]=inf. for all i & j
  int mat()
  {
    for (int i=1; i<=n; i++)
    {
      for (int j=1; j<=n; j++)
      {
        if (i==1 && j==1)
          L[i][j] = m[1][1];
        	P[i][j] = '-';
        else
          if (L[i-1][j] < L[i][j-1])
          {
          	L[i][j] = m[i][j] + (L[i-1][j]);  
            P[i][j] = '->';
          }
        	else
          {
            L[i][j] = m[i][j] + L[i][j-1];
            P[i][j] = '<-'
          }
      }
    }
    return L[n][n];
  }
  
  // 경로를 출력
  void printPathRecursive(int i, int j)
  {
  	if(P[i][j] == '-')
      print(i + ' ' + j);
    else 
    {
      if (P[i][j]=='<-')
        printPathRecursive(i,j-1);
      else
        printPathRecursive(i-1, j);
      print(i+' '+j);
    }
  }
  ```

  

