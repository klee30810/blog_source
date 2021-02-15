---
title: "Chap5. 주석에 담아야 하는 대상"
menuTitle : "5. Comment Contents"
weight: 5
date: 2021-02-15T07:54:46+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]
hidden: "true"


LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

**★ 주석의 목적은 코드를 읽는 사람이 코드를 작성한 사람만큼 코드를 잘 이해하게 돕는 데 있다.**

### __01. 설명하지 말아야 하는 것

**★ 코드에서 빠르게 유추할 수 있는 내용은 주석으로 달지 말라.**

- 설명 자체를 위한 설명을 달지 말라. 주석을 달아야하겠다면 파악하기 어려운 세부사항을 적는것이 좋다.

- 나쁜 이름에 주석을 달지 마라. 대신 이름을 고쳐라. 좋은 코드 > 나쁜 코드 + 좋은 주석

### __02. 생각을 기록하라

- 코드를 짜면서 의도한 바나 깨달은 바를 주석으로 기록하라.
- 상수가 무엇을 하는지, 그것이 왜 특정한 값을 갖게 되었는지에 대한 내용을 기록해둬라.
- 코드에 있는 결함을 설명하라. 코드에 부족한 점이 있는건 당연하고, 당장 그것을 고칠 시간이 없을때도 많다.
- 코드의 어떠한 내용을 훗날 수정할 것이라는 생각이 들면 이를 주석으로 작성하는 걸 당연하게 받아들여라.
- 코드에 개선해야 할 점이 있다고 생각된다면 이를 주석으로 기록하라. 다음은 이를 위해 자주 쓰이는 표시들이다.

![image](/images/compu/readable_code/chap5/Untitled.png)

### __03. 코드를 읽는 사람의 입장이 되어라

- 코드를 처음으로 읽는 외부인의 입장이 되어보라.
- 나올 것 같은 질문 예측하기 : 누군가가 코드를 읽었을 때 할법한 질문의 답변을 주석으로 달아라.
- 사람들이 쉽게 빠질 것 같은 함정을 경고하라 : 코드를 쓰다가 생길수 있는 문제점을 미리 예측해서 주석으로 달아라.
- '큰 그림'에 대한 주석 : 특정 코드가 상위 수준의 문제와 어떻게 연결되어 있는지를 주석으로 달아라.
- 이는 함수 내부에서 코드의 목적을 설명하는 식으로도 적용될 수 있다.
- 사람들이 코드를 쉽게 읽을 수 있게 도와주는 내용이라면 그것이 무엇이든지 적어두는 것이 좋다.

### __04. 마지막 고찰 - 글 쓰는 두려움을 떨쳐내라

- 그냥 떠오르는 생각이라도 써있는 것이 아무것도 안 써있는 것보다는 좋다.
- 떠오른 생각을 적은 주석의 표현들을 좀 더 구체적인 표현으로 바꾸다보면 사람들이 읽기 편한 주석을 작성할 수 있다.

### __요약

- What not to comment:
  - Facts that can be quickly derived from the code itself.
  - “Crutch comments” that make up for bad code (such as a bad function name)—fix the
    code instead.

- Thoughts you should be recording include:
  - Insights about why code is one way and not another (“director commentary”).
  - Flaws in your code, by using markers like TODO: or XXX:.\
  - The “story” for how a constant got its value.
- Put yourself in the reader’s shoes:
  - Anticipate which parts of your code will make readers say “Huh?” and comment those.
  - Document any surprising behavior an average reader wouldn’t expect.
  - Use “big picture” comments at the file/class level to explain how all the pieces fit together.
  - Summarize blocks of code with comments so that the reader doesn’t get lost in the details.
