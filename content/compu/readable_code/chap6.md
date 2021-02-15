---
title: "Chap6. 명확하고 간결한 주석 달기"
menuTitle : "6. Clear Comments"
weight: 6
date: 2021-02-15T07:54:48+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]
hidden: "true"


LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- **주석은 높은 '정보 대 공간' 비율을 맞춰야 한다.**

### __01. 주석을 간결하게 하라

```cpp
// The int is the CategoryType.
// The first float in the inner pair is the 'score',
// the second is the 'weight'.
typedef hash_map<int, pair<float, float> > ScoreMap;

/* CONCISE ONE!!! */
// CategoryType -> (score, weight)
typedef hash_map<int, pair<float, float> > ScoreMap;
```

- 짧게 줄여도 정보를 충분히 전달할 수 있다면 주석을 짧게 줄여라.

### __02. 모호한 대명사는 피하라

- 모호한 대명사는 원래 명사로 고치거나, 의미가 명확해지도록 문장을 고쳐라.

### __03. 엉터리 문장을 다듬어라

- 주석을 명확하게 하는 작업과 간결하게 하는 작업은 대부분 한꺼번에 이루어진다.

  

### __04. 함수의 동작을 명확하게 설명하라

```cpp
// Count how many newline bytes ('\n') are in the file.
int CountLines(string filename) { ... }
```

- 애매한 단어를 사용하지 말고 구현한 방식을 명확하게 설명하는 것이 좋다.

### __05. 구체적인 용법을 설명해주는 입/출력 예를 사용하라

```cpp
// ...
// Example: Partition([8 5 9 8 2], 8) might result in [5 2 | 8 9 8] and return 1
int Partition(vector<int>* v, int pivot);
```

- 주석을 작성하는 데 신중하게 선택된 입/출력 예는 천 마디 말보다 위력적이다.
- 모든 동작을 표현할 수 있는 입/출력 예를 주석으로 사용하라.

### __06. 코드의 의도를 명시하라

- 이러한 주석은 코드가 의도와 다르게 작동하는지 확인하기 좋다.

### __07. 이름을 가진 함수 파라미터 주석

- 뜻이 잘 드러나는 이름을 지을 수 없거나 값을 전달할 때 그 값의 의미를 알기가 힘들다면 값의 앞에 주석을 달아라.

### __08. 정보 축약형 단어를 사용하라

```cpp
As another example, a comment such as:
// Remove excess whitespace from the street address, and do lots of other cleanup
// like turn "Avenue" into "Ave." This way, if there are two different street addresses
// that are typed in slightly differently, they will have the same cleaned-up version and
// we can detect that these are equal.

could instead be:
// Canonicalize the street address (remove extra spaces, "Avenue" -> "Ave.", etc.)
```

- 지속적으로 반복되는 문제에 대해서는 이를 표현하기 위한 관용적 표현이 만들어져 있는 경우가 많다. 이를 찾아보고 활용하라.

ex) '경험적인(heuristic)', '주먹구구식(brute_force)', '순진한 해법(native solution)' 등

### __요약

- Avoid pronouns like “it” and “this” when they can refer to multiple things.
- Describe a function’s behavior with as much precision as is practical.
- Illustrate your comments with carefully chosen input/output examples.
- State the high-level intent of your code, rather than the obvious details.
- Use inline comments (e.g., Function(/* arg = */ ... ) ) to explain mysterious function
  arguments.
- Keep your comments brief by using words that pack a lot of meaning.
