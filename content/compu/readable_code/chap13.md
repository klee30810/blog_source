---
title: "Chap13. 코드 분량 줄이기"
menuTitle : "13. Code Diet"
weight: 13
date: 2021-02-15T07:55:09+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]
hidden: "true"


LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- 프로그래머가 배워야 하는 가장 중요한 기술은 언제 코딩을 해야 하는지 아는 것이다.

**★ 가장 읽기 쉬운 코드는 아무것도 없는 코드다.**

### __01. 그 기능을 구현하려고 애쓰지 마라 - 그럴 필요가 없다

- 프로그래머는 대개 프로젝트에 정말로 필요한 기능이 얼마나 있는지 과대평가하는 경향이 있다.
- 또한 어떤 기능을 구현하는 데 필요한 노력을 과소평가하는 경향도 있다.
- 이는 조잡한 프로토타입을 구현하는 시간을 지나치게 낙관적으로 에측하고, 그 코드를 장차 유지보수하고, 문서를 만들고, 코드 베이스에 새로운 '무게'를 더하는데 얼마나 많은 시간이 필요한지를 완전히 잊어버리게 한다.

### __02. 요구사항에 질문을 던지고 질문을 잘게 나누어 분석하라

- 주어진 요구사항을 정말로 잘 분석하면, 적은 코드로 구현할 수 있는 간단한 문제를 정의할 수 있다.
- "요구사항을 제거하기"와 "더 간단한 문제를 해결하기"가 제공하는 이점은 아무리 강조해도 지나치지 않다.

### __03. 코드베이스를 작게 유지하기

- 프로젝트를 처음 시작할 때는 한 두개의 소스 파일만 있으므로 관리하기 용이하나, 프로젝트가 커지면 이를 관리하는게 갈수록 어려워지기 마련이다.
- 이에 대처할 최선의 방법은 프로젝트가 성장하더라도 '코드베이스를 최대한 작고 가볍게 유지하는 것'이다.

1. 일반적인 '유틸리티'를 많이 생성하여 중복된 코드를 제거하라.

2. 사용하지 않는 코드 혹은 필요 없는 기능을 제거하라.

3. 프로젝트가 서로 분절된 하위 프로젝트로 구성되게 하라.

4. 코드베이스의 '무게'를 항상 의식하여 가볍고 날렵하게 유지시켜라.

### __04. 자기 주변에 있는 라이브러리에 친숙해져라

- 프로그래머는 이미 존재하는 라이브러리로 자신의 문제를 풀 수 있는 상황이 많다는 걸 모르고 있다.
- **매일 15분씩 자신의 표준 라이브러리에 있는 모든 함수/모듈/형들의 이름을 읽어라. :** These include the C++ Standard Template Library (STL), the Java API, the built-in Python modules, and others.
- 이는 라이브러리 전체를 암기하라는 것이 아니라 그 안에 무엇이 있는지 감을 잡아놓고 이후에 떠올릴 수 있게 하라는 뜻이다.

```python
def unique(elements):
		temp = {}
		for element in elements:
				temp[element] = None # The value doesn't matter.
		return temp.keys()

unique_elements = unique([2,1,2])

/*-------------------------------*/
unique_elements = list(set([2,1,2])) # Remove duplicates
```

- 통계에 따르면 평균적인 수준의 소프트웨어 엔지니어는 출시할 수 있는 수준의 코드를 하루 평균 10줄 정도 작성한다고 한다.
- 여기에서 중요한 단어는 '출시할 수 있는 있는(shippable)'이라는 표현이다.
- 완숙한 라이브러리 안에 있는 모든 코드는 상당한 분량의 설계, 디버깅, 재작성, 문서화, 최적화, 테스트를 거쳤다.

### __05. 예: 코딩 대신 유닉스 도구를 활용하기

```html
1.2.3.4 example.com [24/Aug/2010:01:08:34] "GET /index.html HTTP/1.1" 200 ...
2.3.4.5 example.com [24/Aug/2010:01:14:27] "GET /help?topic=8 HTTP/1.1" 500 ...
3.4.5.6 example.com [24/Aug/2010:01:15:54] "GET /favicon.ico HTTP/1.1" 404 ...
...
// browser-IP host [date] "GET /url-path HTTP/1.1" HTTP-response-code ...
```

- Unix

```jsx
// UNIX
cat access.log | awk '{ print $5 " " $7 }' | egrep "[45]..$" \
| sort | uniq -c | sort -nr

// Produces : \
// 95 /favicon.ico 404
// 13 /help?topic=8 500
// 11 /login 403
// ...
// <count> <path> <http response code>
```

- \we’ve avoided writing any “real” code or checking anything into source control.

### __요약

- 다음과 같은 방법으로 새로운 코드를 작성하는 일을 피할 수 있다.

1. 제품에 꼭 필요하지 않은 기능을 제거하고, '과도한 작업(overengineering)'을 피한다.

2. 요구사항을 다시 생각해서, 가장 단순한 형태의 문제를 찾아본다.

3. 주기적으로 라이브러리 전체 API를 훑어봄으로써 표준 라이브러리에 친숙해진다.
