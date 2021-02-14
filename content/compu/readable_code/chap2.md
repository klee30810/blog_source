---
title: "Chap2. 이름에 정보 담기"
menuTitle : "2. Naming with Info"
date: 2021-02-15T07:54:38+09:00
draft: false
weight: 2
katex: true
markup: mmark
tags: ["programming"]

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

**★ 이름(식별자)에 정보를 담아내라. 이름을 일종의 설명문으로 생각하라. 좋은 이름은 생각보다 많은 정보를 전달할 수 있다.**

### __01. 특정한 단어 고르기

- 구체적인 단어를 선택하라. 더 '화려한' 단어를 찾아보면 그 변수에 더 적합한 단어를 찾는데 도움이 된다.

**★ 재치 있는 이름보다 명확하고 간결한 이름이 더 좋다.**

![image](/images/compu/readable_code/chap2/Untitled.png)

### __02. tmp나 retval 같은 보편적인 이름 피하기

```jsx
var euclidean_norm = function (v) {
	var retval = 0.0;
	for (var i = 0; i < v.length; i += 1)
		retval += v[i] * v[i];
	return Math.sqrt(retval);
};
```

**★ retval이라는 이름은 정보를 제대로 담고 있지 않다. 대신 변수값을 설명하는 이름을 사용하라.**

- 하지만 보편적인 이름이 필요한 의미를 더 전달하는 경우도 있다.

**★ tmp라는 이름은 대상이 짧게 임시적으로만 존재하고, 임시적 존재 자체가 변수의 가장 중요한 용도일때에 한해서 사용해야한다.**

```jsx
if (right < left) {
	tmp = right;
	right = left;
	left = tmp;
		}   // Acceptable tmp
```

- 루프 반복자에 조건문의 이니셜 등을 첨자로 넣으면 더 좋은 이름이 된다.

**★ tmp, it, retval 같은 보편적인 이름을 사용하려면, 꼭 그렇게 해야 하는 이유가 있어야 한다.**

### __03. 추상적인 이름보다 구체적인 이름을 선호하라

- **이름을 지을 때, 추상적인 방식이 아니라 구체적인 방식으로 묘사하라.**
- 예1 : 서버가 어느 TCP/IP 포트를 사용할 수 있는지 검사하는 내부 메소드의 이름은 ServerCanStart()보다 CanListenOnPort()가 더 구체적이다.
- 예2 : DISALLOW_EVIL_CONSTRUCTOR라는 구글이 사용하던 매크로의 명칭은 사악한(evil)이라는 단어가 너무 강한 표현이고, 매크로가 금지하는(disallowing)대상이 명확하지 않기 때문에 좋은 이름이 아니다. 이 이름은 DISALLOW_COPY_AND_ASSIGN으로 대체되었다.
- 예3 : 저자들이 사용했던 --run_locally라는 명령행 플래그(command-lind flag) 옵션
  - 1. 새로운 사람이 무엇을 위한 플래그인지 알기 어렵고,
  - 2. 원격으로 실행되는 프로그램에 쓰이기에는 부적절한 이름이었고,
  - 3. 성능 저하의 가능성이 있다는 사실을 담고 있지 않다는 문제점이 있었다.
  - 핵심은 이 이름이 실제 내용보다 주로 사용되는 환경을 나타내는 방식으로 지어졌다는 점이다. 이 이름보다는 --extra_logging이라는 이름이 더 직접적이고 명확하다. 만약 이 플래그가 다른 작업도 수행하며 그 이름이 두 가지 모두를 포괄할 수 있다 하더라도 그럴때는 새로운 역할을 구체적으로 표현하는 새로운 플래그를 만드는 것이 더 좋다.

### __04. 추가적인 정보를 이름에 추가하기

- 이름 안에 들어간 추가 정보는 사용할 때마다 전달된다. 따라서 사용자가 반드시 알아야 하는 정보를 이름에 포함시키는 것이 좋다.
- 변수가 시간의 양이나 바이트의 수와 같은 측정치를 담고 있다면, 변수명에 단위를 포함시키는게 좋다.

![image](/images/compu/readable_code/chap2/untitled1.png)

- 변수의 의미를 제대로 이해하는 것이 중요하다면 그 의미를 드러내는 정보를 변수의 이름에 포함시켜야 한다.

![image](/images/compu/readable_code/chap2/untitled2.png)

### __05. 이름은 얼마나 길어야 하는가?

- 좁은 범위에서는 짧은 이름도 좋다. 하지만 어떤 이름이 넓은 범위에서 사용된다면, 의미를 분명하게 하는 것이 더 중요하다.
- 팀에 새로 합류한 사람이 이름이 의미하는 바를 이해할 수 있다면 괜찮은 이름이다.
- 제거해도 정보가 손실 되지 않는 단어는 제거하는 것이 좋다.

### __06. 이름 포메팅으로 의미를 전달하라

- 밑줄(underscores)과 대시(dashes) 그리고 대문자를 잘 이용하면 이름에 더 많은 정보를 담을 수 있다.

```cpp
static const int kMaxOpenFiles = 100;   // NOT MACRO_NAME to be easily distinguished
class LogReader {    // CamelCase for class names
	public:
		void OpenFile(string local_file);     // lower_separated for variable name

	private:
		int offset_;       // class member variable ends with an underscore 
		DISALLOW_COPY_AND_ASSIGN(LogReader);
	};
```

- 문법적 차이가 드러나게 서로 다른 개체의 이름에 다른 포맷팅 방식을 적용하는 것은 코드를 더 읽기 쉽게 해준다.

### __Summary

- **Use specific words**—for example, instead of Get, words like Fetch or Download might be
  better, depending on the context.
- Avoid generic names like tmp and retval, unless there’s a specific reason to use them.
- **Use concrete names** that describe things in more detail—the name ServerCanStart() is
  vague compared to CanListenOnPort().
- **Attach important details** to variable names—for example, append *ms to a variable
  whose value is in milliseconds or prepend raw* to an unprocessed variable that needs
  escaping.
- **Use longer names for larger scopes**—don’t use cryptic one- or two-letter names for
  variables that span multiple screens; shorter names are better for variables that span only
  a few lines.
- **Use capitalization, underscores, and so on in a meaningful way**—for example, you
  can append “_” to class members to distinguish them from local variables.