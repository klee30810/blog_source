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

