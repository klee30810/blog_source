---
title: "Chap3. 오해할 수 없는 이름들"
menuTitle : "3. Clear Names"
weight: 3
date: 2021-02-15T07:54:41+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

**★ 본인이 지은 이름을 "다른 사람들이 다른 의미로 해석할 수 있을까?"라는 질문을 던져보며 철저하게 확인해야 한다.**

### __01. 예: Filter()

- filter라는 변수명은 의미가 모호하다. 대상을 '고르는' 기능을 원한다면 select가 '제거하는' 기능을 원한다면 exclude가 더 낫다.

### __02. 예: Clip(text, length)

- 어떤 문단의 내용을 오려내는 함수 Clip(text,length)라면 두 가지 방식으로 이해할 수 있다. 이 함수의 기능은 문단을 처음부터 최대 length만큼 잘라내는 것이므로 truncate(text,max_chars)가 더 좋다.
- max_length 은 몇몇 애매모호함을 가지고 있다.
  • A number of bytes
  • A number of characters
  • A number of words

### __03. 경계를 포함하는 한계값을 다룰 때는 min과 max를 사용하라

- CART_TOO_BIG_LIMIT라는 이름은 '그 수까지(up to)'를 의미하는지, '그 수까지(up to including)'를 의미하는지 분명하지 않다

**★ 한계를 설정하는 이름을 가장 명확하게 만드는 방법은 제한 받는 대상의 이름 앞에 max_나 min_을 붙이는 것이다.** (max나 min은 그 값을 포함한다는 의미를 갖는다.)

### __04. 경계를 포함하는 범위에는 first와 last를 사용하라

![image](/images/compu/readable_code/chap3/Untitled.png)

- 경계의 양 끝점을 포함하는 범위에는 first와 last가 적절하다(inclusive). 이는 시작점과 끝점이라는 정보를 명확하게 전달한다.

### __05. 경계를 포함하고/배제하는 범위에는 begin과 end를 사용하라

![image](/images/compu/readable_code/chap3/untitled1.png)

- 시작점에서는 경계를 포함하고, 끝점에는 포함하지 않는 범위에는 begin과 end가 관행적으로 가장 적절하다.

### __06. 불리언 변수에 이름 붙이기

- 불리언 변수 혹은 불리언 값을 반환하는 함수에 이름을 붙일 때는 true와 false가 각각 무엇을 의미하는지 명확해야 한다.

ex) read_password → need_password or user_is_authenticated

- 일반적으로 is, has, can, should와 같은 단어를 더하면 불리언 값의 의미가 더 명확해진다.
- 또한 이름에서는 의미를 부정하는 단어를 피하는 것이 좋다.

### __07. 사용자의 기대에 부응하기

- 프로그래머들이 어떤 이름을 이미 특정한 방식으로 이해해서 실제와 다른 의미로 관행적으로 사용하는 것들이 있다. 이런 경우에는 그것을 따르는 것이 좋다.
- get*() : get으로 시작되는 이름의 메소드는 '가벼운 접근자(lightweight accessos'로서 단순히 내부 멤버를 반환한다고 관행적으로 생각한다.
- list:size() : list.size()는 C++표준 라이브러리에 있던 메소드인데 이는 O(n)연산을 했다. 하지만 프로그래머들은 size() 메소드가 일정한 시간을 소비한다고 관행적으로 생각한다. c++에 존재하는 다른 모든 size()메소드는 일정한 시간을 소비하기 때문이다. 이럴때는 메소드명을 size()가 아니라 countSize()나 countElements()라 했다면 오해를 피할 수 있었을 것이다. 다행히도 최근에 C++ 표준 라이브러리에서 "size()는 O(1)이어야 한다."는 규칙을 정했다.

### __08. 예: 이름을 짓기 위해서 복수의 후보를 평가하기

- 좋은 이름을 정하기 위해서는 여러개의 후보를 놓고 이것이 명확한 의미를 전달하는지를 따져보는 습관을 가질 필요가 있다.

### __요약

- The best names are ones that can’t be misconstrued—the person reading your code will
  understand it the way you meant it, and no other way. Unfortunately, a lot of English words are ambiguous when it comes to programming, such as filter, length, and limit.
- Before you decide on a name, play devil’s advocate and imagine how your name might be misunderstood. The best names are resistant to misinterpretation.
- When it comes to defining an upper or lower limit for a value, max_ and min_ are good prefixes to use. For inclusive ranges, first and last are good. For inclusive/exclusive ranges, begin and end are best because they’re the most idiomatic.
- When naming a boolean, use words like is and has to make it clear that it’s a boolean. Avoid negated terms (e.g., disable_ssl).
- Beware of users’ expectations about certain words. For example, users may expect get() or
  size() to be lightweight methods.