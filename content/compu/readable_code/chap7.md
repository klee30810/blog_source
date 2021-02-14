---
title: "Chap7. 읽기 쉽게 흐름제어 만들기"
menuTitle : "7. Readable Control"
weight: 7
date: 2021-02-15T07:54:50+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- 제어문은 사실 코드를 읽기 어렵게 만든다.

★ 흐름을 제어하는 조건과 루프 그리고 여타 요소를 **최대한 '자연스럽게'** 만들도록 노력하라.코드를 읽다가 다시 되돌아가서 코드를 읽지 않아도 되게끔 만들어야 한다.

### __01. 조건문에서 인수의 순서

- 왼쪽에 '질문을 받는' 유동적인 값을 쓰고, 오른쪽에 비교대상으로 사용되는 고정적인 값을 써라.

### __02. if/else 블록의 순서

- 조건은 부정문이 아닌 긍정문으로 쓰려고하라.
- 간단한 것을 먼저 처리하라. 이러면 한 화면으로 두개의 구문을 볼 수 있을 가능성이 높아진다.
- 더 흥미롭고, 확실한 것을 먼저 다루어라.
- 위의 세가지 규칙이 서로 충돌을 일으킬 수도 있다. 중요한 것은 자신이 작성한 if/else문이 이상한 순서로 작성되었는지 확인하라는 것이다.

### __03. (삼항 연산자로 알려진)?:를 이용하는 조건문 표현

★ 줄 수를 최소하는 일보다 다른 사람이 코드를 읽고 이해하는데 걸리는 시간을 최소화하는 일이 더 중요하다.

★ 기본적으로 if/else를 이용하라. ?:를 이용하는 삼항 연산은 매우 간단할 때만 사용해야 한다.

### __04. do/while 루프를 피하라

- do/while루프는 코드를 두번 읽도록 만든다. 반면에 while 루프는 반복 조건을 먼저 확인할 수 있으므로 읽기 좋다.

- 하지만 do/while을 제거하려고 중복된 코드를 사용하진 말라. 그렇게 하지 않아도 대부분의 do/while루프를 while루프로 변경할 수 있다.

- 특히 do/while루프 안에서 continue를 사용하면 혼란을 초래하므로 피하는 것이 좋다.

      (다만 이 조언이 do/while루프를 절대 쓰지 말라는 뜻은 아니다.)

★  "내 경험으로 에러와 혼동의 원인은 do문에 있다. 그래서 나는 **조건이 '눈에 띄는 곳에 미리' 나타나도록 만드는 것**을 선호한다.

- 결과적으로 나는 do문을 피하는 경향이 있다." - C++의 창시자 "반얀 스토라우스트럽"

### __05. 함수 중간에서 반환하기

- 반환 포인트를 하나만 두려는건 함수의 끝부분에서 실행되는 클린업(cleanup)코드의 호출을 보장하려는 의도이다.
- 하지만 현대의 언어들은 클린업 코드를 실행시키는 더 정교한 방법을 제공하고 있다.       그러므로 이제는 함수 중간에서 반환하는 것은 완전히 허용되어야 한다.

### __06. 악명 높은 goto

- goto를 쓰면 코드가 쉽게 엉망진창이 될 가능성이 높으므로 피하는 것이 좋다.
- 하지만 클린업 코드를 실행시키기 위해서 함수의 맨 밑으로 단 하나의 exit 포인트만 두는 방식이라면 goto의 괜찮은 사용법이라 할 수 있다.

(어지럽게 중첩된 코드에서 한번에 빠져나오기 위한 용도로도 goto는 유용하다. 그런 코드 자체를 지양해야겠지만 말이다.)

### __07. 중첩을 최소화하기

- 코드가 중첩된다는 것은 읽는 사람에게는 정신적 스택에 추가적인 조건이 입력된다는 뜻이다.
- 중첩이 추가되는 상황은 주로 코드를 수정하는 과정에서 새로운 코드를 추가할 때이다.

★ 코드를 수정해야 하는 상황이 오면 코드를 새로운 관점에서 혹은 코드 전체를 보고 어떻게 수정하면 좋을지 생각해보라.

- 함수 중간에서 반환하여 중첩을 제거하라 (if/return)
- 밖으로 빠져나가지 않고 루프 중간에서 반환해야 한다면 continue를 사용하면 된다. (if/continue)
  - 일반적으로 continue문은 goto처럼 논리의 흐름을 건너뛰게 하므로 읽는 이를 혼란스럽게 만들 수 있다.    하지만 루프에 대한 각각의 반복이 독립적이라면 continue문이 단지 이번 반복을 건너뛰어라라는 의미임을 쉽게 확인할 수 있다.

- 우리는 자신의 프로그램에 존재하는 '흐름'을 상위 수준에서 조망해볼 필요가 있다.
- 궁극적인 목표는 프로그램의 전체 실행 경로를 쉽게 따라갈 수 있게 만드는 것이기 때문이다.   다음은 흐름을 따라가기 어렵게 만드는 구조들이다. 이러한 구조들은 가능하면 사용을 지양하는 것이 좋다.

### __08. 실행 흐름을 따라올 수 있는가?

![image](/images/compu/readable_code/chap7/Untitled.png)

### __요약

- When writing a comparison (while (bytes_expected > bytes_received)), it’s better to put the changing value on the left and the more stable value on the right (while (bytes_received < bytes_expected)).
- You can also reorder the blocks of an if/else statement. Generally, try to handle the positive/easier/interesting case first. Sometimes these criteria conflict, but when they don’t, it’s a good rule of thumb to follow.
- Certain programming constructs, like the ternary operator (: ?), the do/while loop, and goto often result in unreadable code. It’s usually best not to use them, as clearer alternatives almost always exist.
- Nested code blocks require more concentration to follow along. Each new nesting requires more context to be “pushed onto the stack” of the reader. Instead, opt for more “linear” code to avoid deep nesting.
- Returning early can remove nesting and clean up code in general. “Guard statements”
  (handling simple cases at the top of the function) are especially useful.