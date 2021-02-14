---
title: "Chap9. 변수와 가독성"
menuTitle : "9. Readable Var"
weight: 9
date: 2021-02-15T07:54:56+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- 변수를 잘못 사용하면 다음과 같은 문제가 있다.

1. 변수의 수가 많을수록 기억하고 다루기 더 어려워진다.

2. 변수의 범위가 넓어질수록 기억하고 다루는 시간이 더 길어진다.

3. 변수값이 자주 바뀔수록 현재값을 기억하고 다루기가 더 어려워진다.

### __01. 변수 제거하기

- 가독성에 도움되지 않는 변수는 제거하는 것이 좋다.
  - It isn’t breaking down a complex expression.
  - It doesn’t add clarification—the expression datetime.datetime.now() is clear enough.
  - It’s used only once, so it doesn’t compress any redundant code.

- 불필요한 임시 변수 : 복잡한 표현을 나누는 것도 아니고, 명확성에 도움이 되는 것도 아니고, 중복된 코드를 압축하지도 않는 임시 변수.
- 중간 결과 삭제하기

```jsx
// BAD
var remove_one = function (array, value_to_remove) {
		var index_to_remove = null;
		for (var i = 0; i < array.length; i += 1) {
				if (array[i] === value_to_remove) {
						index_to_remove = i;
						break;
				}
		}
		if (index_to_remove !== null) {
				array.splice(index_to_remove, 1);
		}
};
```

```jsx
var remove_one = function (array, value_to_remove) {
		for (var i = 0; i < array.length; i += 1) {
				if (array[i] === value_to_remove) {
						array.splice(i, 1);
						return;
				}
		}
};

// it’s a good strategy to complete the task as quickly as possible.
```

- 흐름 제어 변수 제거하기 : 순수하게 프로그램의 실행 방향을 설정하는데 사용하는 변수는 구조를 변경하는 것으로 제거할 수 있다.

### __02. 변수의 범위를 좁혀라

**★ 모든 변수에 대해서 변수가 적용되는 범위를 최대한 좁게 만들어라.**  

- 변수의 범위를 좁히면 코드를 읽는 사람이 한꺼번에 생각해야 하는 변수의 수를 줄여주기 때문이다.
- 클래스 멤버 변수도 클래스 안의 전역 변수라 할 수 있다. 이런 변수도 지역변수로 강등시키는 것이 좋다.

```cpp
// For this case, it may make sense to “demote” str_ to be a local variable:
class LargeClass {
		void Method1() {
				string str = ...;
				Method2(str);
		}
		void Method2(string str) {
				// Uses str
		}

		// Now other methods can't see str.
};
```

- 메소드를 정적 static으로 만들어 클래스 멤버 접근을 제한하면 읽는 사람에게 이 메소드가 변수들로부터 독립적이라는 사실을 알려줄 수 있다.
- 커다란 클래스를 독립적인 작은 클래스로 나눌 수 있다면 나누는 것이 좋다.  - 변수는 사용하기 직전에 선언해서 사용 범위가 끝나면 해제되게 하는 것이 좋다.

- C++에서는 조건문 안에서 변수를 선언할 수 있다. 이렇게 하면 그 변수는 조건문 안에서만 사용되는 것이 명확해진다.
- 자바스크립트에서는 클로저(closure) 내부에 집어넣어 그 함수에서만 사용되게 할 수 있다.
- 자바스크립트에서 변수를 정의할 때 var를 생략하면 해당 변수는 전역 변수가 된다.
- 파이썬과 자바스크립트에는 중첩된 범위가 없으므로 '가장 인접한 공통 조상(closer common ancestor)'에 정의하여 코드의 가독성을 높일 수 있다.

### __03. 값을 한 번만 할당하는 변수를 선호하라

- 값이 고정된 변수는 읽는 사람에게 별다른 추가적인 생각을 요구하지 않는다. 그러므로 C++에서는 const 사용을, 자바에서는 final 사용을 권장한다.

**★ 변수값이 달라지는 곳이 많을수록 현재값을 추측하기 더 어려워진다. 그러므로 최대한 적은 횟수로 변하도록 설계하라.**

### __04. 마지막 예

- 아래와 같은 HTML이 있고,

```html
<input type="text" id="input1" value="Dustin">
<input type="text" id="input2" value="Trevor">
<input type="text" id="input3" value="">
<input type="text" id="input4" value="Melissa">
```

- string을 하나 받고 첫번째 empty \<input>에 넣는 setFirstEmptyInput() 함수를 작성하려 한다.

```jsx
var setFirstEmptyInput = function (new_value) {
		var found = false;
		var i = 1;
		var elem = document.getElementById('input' + i);
		while (elem !== null) {
				if (elem.value === '') {
						found = true;
						break;
				}
				i++;
				elem = document.getElementById('input' + i);
		}
		
		if (found) elem.value = new_value;
		return elem;
};
```

- 변수 관점 : var found, i, elem

  - found 같은 intermediate variables는 미리 return

  ```jsx
  var setFirstEmptyInput = function (new_value) {
  		var i = 1;
  		var elem = document.getElementById('input' + i);
  		while (elem !== null) {
  				if (elem.value === '') {
  						elem.value = new_value;
  						return elem;
  				}
  				i++;
  				elem = document.getElementById('input' + i);
  		}
  		
  		return null;
  };
  ```

- elem은 loopy하게 쓰이는데 1부터 올라가는 순서로 바꿔써본다

```jsx
var setFirstEmptyInput = function (new_value) {
		for (var i = 1; true; i++) {
				var elem = document.getElementById('input' + i);
				if (elem === null)
						return null; // Search Failed. No empty input found.
				if (elem.value === '') {
						elem.value = new_value;
						return elem;
				}
		}
};
```

### __요약

- You can make your code easier to read by having fewer variables and
  making them as “lightweight” as possible. Specifically:
    - **Eliminate variables that just get in the way**. In particular, we showed a few examples
      of how to eliminate “intermediate result” variables by handling the result immediately.
    - **Reduce the scope of each variable to be as small as possible.** Move each variable to a place where the fewest lines of code can see it. Out of sight is out of mind.
    - **Prefer write-once variables.** Variables that are set only once (or const, final, or
      otherwise immutable) make code easier to understand.