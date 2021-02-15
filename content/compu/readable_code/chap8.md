---
title: "Chap8. 거대한 표현을 잘게 쪼개기"
menuTitle : "8. Segments"
weight: 8
date: 2021-02-15T07:54:53+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]
hidden: "true"


LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

★ 거대한 표현을 더 소화하기 쉬운 여러 조각으로 나눈다.

### __01. 설명 변수

- 거대한 표현을 쪼개는 가장 쉬운 방법은 작은 하위 표현을 담을 '추가 변수(extra variable)'을 만드는 것이다.

```cpp
// Here is an example:
if line.split(':')[0].strip() == "root":
...
//Here is the same code, now with an explaining variable:
username = line.split(':')[0].strip()
if username == "root":
...
```

- 추가 변수는 하위 표현의 의미를 설명하므로 '설명 변수(explaning variable)'라고도 한다.

### __02. 요약 변수

```cpp
final boolean user_owns_document = (request.user.id == document.owner_id);
if (user_owns_document) {
	// user can edit this document...
}
	...
if (!user_owns_document) {
	// document is read-only...
}
```

- 커다란 코드의 덩어리를 짧은 이름으로 대체하여 더 쉽게 관리하고 파악하는 목적으로 만드는 변수를 '요약 변수(summary variable)'라고 한다.
- 요약 변수를 통해 읽는 사람이 코드의 주된 개념을 더 쉽게 파악할 수 있게 도와줄 수 있다.

### __03. 드모르간의 법칙 사용하기

1. not (a or b or c) <=> (not a) and (not b) and (not c)
2. not (a and b and c) <=> (not a) or (not b) or (not c)

### __04. 쇼트 서킷 논리 오용하기

- 쇼트 서킷 논리를 복잡한 연산에 사용하면 가독성을 해칠 수 있다.

★ '영리하게' 작성된 코드에 유의하라. 나중에 다른 사람이 읽으면 그런 코드가 종종 혼란을 초래한다.

- 파이썬, JS, 루비 같은 언어는 or 연산자가 인수 중 하나를 반환한다.

따라서 x = a || b|| c 라는 코드는 a,b,c 세 값 중에서 첫 번째 '참'값을 반환하는데 사용할 수 있다.

### __05. 예: 복잡한 논리와 씨름하기

```cpp
struct Range {
	int begin;
	int end;
	
	// For example, [0,5) overlaps with [3,8)
	bool OverlapsWith(Range other);
};

bool Range::OverlapsWith(Range other) {
	// Check if 'begin' or 'end' falls inside 'other'.
	return (begin >= other.begin && begin < other.end) ||
				 (end > other.begin && end <= other.end) ||
				 (begin <= other.begin && end >= other.end);
}
```

- One technique is to see if you can solve the problem the “opposite” way. Depending on the situation you’re in, this could mean iterating through arrays in reverse or filling in some data structure backward rather than forward.

```cpp
/*
1. The other range ends before this one begins.
2. The other range begins after this one ends.
*/
// We can turn this into code quite easily:

bool Range::OverlapsWith(Range other) {
if (other.end <= begin) return false; // They end before we begin
if (other.begin >= end) return false; // They begin after we end
return true; // Only possibility left: they overlap
}
```

### __06. 거대한 구문 나누기

```cpp
// BAD
var update_highlight = function (message_num) {
	if ($("#vote_value" + message_num).html() === "Up") {
			$("#thumbs_up" + message_num).addClass("highlighted");
			$("#thumbs_down" + message_num).removeClass("highlighted");
	} else if ($("#vote_value" + message_num).html() === "Down") {
		  $("#thumbs_up" + message_num).removeClass("highlighted");
		  $("#thumbs_down" + message_num).addClass("highlighted");
	} else {
			$("#thumbs_up" + message_num).removeClass("highighted");
			$("#thumbs_down" + message_num).removeClass("highlighted");
	}
};
```

- 거대한 구문에서도 동일한 부분을 요약 변수나 함수로 추출해서 구문의 앞부분에 놓아둘 수 있다.
- 이는 DRY-Don't Repeat Yourself의 원리이기도 하다.

```cpp
// GOOD
var update_highlight = function (message_num) {
		var thumbs_up = $("#thumbs_up" + message_num);
		var thumbs_down = $("#thumbs_down" + message_num);
		var vote_value = $("#vote_value" + message_num).html();
		var hi = "highlighted";

		if (vote_value === "Up") {
				thumbs_up.addClass(hi);
				thumbs_down.removeClass(hi);
		} else if (vote_value === "Down") {
				thumbs_up.removeClass(hi);
				thumbs_down.addClass(hi);
		} else {
				thumbs_up.removeClass(hi);
				thumbs_down.removeClass(hi);
		}
};
```

- 이렇게 하면 다음과 같은 장점이 있다.

1. 타이핑 실수를 피할 수 있다.

2. 코드를 한눈에 훑어보기 좋도록 코드의 길이를 조금이라도 줄여준다.

3. 클래스명을 변경해야 할 때 한 곳만 바꾸면 된다.

### __07. 표현을 단순화하는 다른 창의적인 방법들

```cpp
void AddStats(const Stats& add_from, Stats* add_to) {
		add_to->set_total_memory(add_from.total_memory() + add_to->total_memory());
		add_to->set_free_memory(add_from.free_memory() + add_to->free_memory());
		add_to->set_swap_memory(add_from.swap_memory() + add_to->swap_memory());
		add_to->set_status_string(add_from.status_string() + add_to->status_string());
		add_to->set_num_processes(add_from.num_processes() + add_to->num_processes());
		...
}
```

- C++에서는 비슷한 표현을 매크로 함수로 정의해서 줄일수도 있다.

```cpp
void AddStats(const Stats& add_from, Stats* add_to) {
		#define ADD_FIELD(field) add_to->set_##field(add_from.field() + add_to->field())
		ADD_FIELD(total_memory);
		ADD_FIELD(free_memory);
		ADD_FIELD(swap_memory);
		ADD_FIELD(status_string);
		ADD_FIELD(num_processes);
		...
		#undef ADD_FIELD
}
```

- 매크로의 사용은 권장할 것이 아니지만 적절히 사용하면 코드 가독성을 높일 수 있다.

### __요약

- One simple technique is to introduce “explaining variables” that capture the value of some large subexpression. This approach has three benefits:
  • It breaks down a giant expression into pieces.
  • It documents the code by describing the subexpression with a succinct name.
  • It helps the reader identify the main “concepts” in the code.
- Another technique is to manipulate your logic using De Morgan’s laws—this technique can sometimes rewrite a boolean expression in a cleaner way (e.g., if (!(a && !b)) turns into if (!a || b)).
- We showed an example where a complex logical condition was broken down into tiny
  statements like “if (a < b) ...”. In fact, all of the improved-code examples in this chapter had if statements with no more than two values inside them. This setup is ideal. It may not always seem possible to do this—sometimes it requires “negating” the problem or considering the opposite of your goal.
