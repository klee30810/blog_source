---
title: "Chap4. 미학"
menuTitle : "4. Aesthetics"
weight: 4
date: 2021-02-15T07:54:43+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

-좋은 소스코드는 '눈을 편하게' 해야 한다.  이 장에서는 다음과 같은 세 가지 원리가 사용된다.

**1. 코드를 읽는 사람이 이미 친숙한, 일관성 있는 레이아웃을 사용하라.**

**2. 비슷한 코드는 서로 비슷해 보이게 만들어라.**

**3. 서로 연관된 코드는 하나의 블록으로 묶어라.**

※ 여기서 말하는 미학은 가독성 만을 위한 것으로 새롭게 설계하는 리팩토링은 서로 독립된 아이디어에 해당한다. 두 가지 모두를 추구하는게 이상적이다.

### __01. 미학이 무슨 상관인가?

- 미학적으로 보기 좋은 코드가 사용하기 더 편리하다. 우리는 대부분의 시간을 코드를 그저 바라보는 데 쓴다는 것을 명심하라.

### __02. 일관성과 간결성을 위해서 줄 바꿈을 재정렬하기

```jsx
public class PerformanceTester {
	// TcpConnectionSimulator(throughput, latency, jitter, packet_loss)
   													// [Kbps]     [ms]    [ms]    [percent]
	public static final TcpConnectionSimulator wifi =
			new TcpConnectionSimulator(500,      80,     200,     1);
	public static final TcpConnectionSimulator t3_fiber =
			new TcpConnectionSimulator(45000,    10,     0,       0);
	public static final TcpConnectionSimulator cell =
			new TcpConnectionSimulator(100,      400,    250,     5);
}
```

- 줄이 길어져 줄바꿈을 해야한다면 다른 비슷한 코드도 같은 방식으로 줄바꿈을 하는게 좋다. (비슷한 코드는 서로 비슷해 보이게 만들어라!)

### __03. 메소드를 활용하여 불규칙성을 정리하라

```jsx
// BAD EXAMPLE
// Turn a partial_name like "Doug Adams" into "Mr. Douglas Adams".
// If not possible, 'error' is filled with an explanation.
string ExpandFullName(DatabaseConnection dc, string partial_name, string* error);

DatabaseConnection database_connection;
string error;
assert(ExpandFullName(database_connection, "Doug Adams", &error)
		== "Mr. Douglas Adams");
assert(error == "");
assert(ExpandFullName(database_connection, " Jake Brown ", &error)
		== "Mr. Jacob Brown III");
assert(error == "");
assert(ExpandFullName(database_connection, "No Such Guy", &error) == "");
assert(error == "no match found");
assert(ExpandFullName(database_connection, "John", &error) == "");
assert(error == "more than one result");
```

```jsx
// GOOD EXAMPLE
CheckFullName("Doug Adams", "Mr. Douglas Adams", "");
CheckFullName(" Jake Brown ", "Mr. Jake Brown III", "");
CheckFullName("No Such Guy", "", "no match found");
CheckFullName("John", "", "more than one result");

void CheckFullName(string partial_name,
									 string expected_full_name,
									 string expected_error) {
// database_connection is now a class member
	string error;
	string full_name = ExpandFullName(database_connection, partial_name, &error);
	assert(error == expected_error);
	assert(full_name == expected_full_name);
}
```

- 같은 내용의 코드가 반복되어야 한다면 이를 새로운 메소드로 작성하라. 이는 미학적 개선(가독성 향상)을 위한 한 일이지만 다음과 같은 장점들도 있다.

1. 중복된 코드를 없애서 코드를 더 간결하게 한다.

2. 이름이나 에러 문자열 같은 테스트의 중요 부분들이 한 눈에 보이게 모아졌다.

3. 새로운 테스트 추가가 훨씬 쉬워졌다.

### __04. 도움이 된다면 코드의 열을 맞춰라

```jsx
commands[] = {
	...
	{ "timeout",         NULL,                 cmd_spec_timeout },
	{ "timestamping",    &opt.timestamping,    cmd_boolean },
	{ "tries",           &opt.ntry,            cmd_number_inf },
	{ "useproxy",        &opt.use_proxy,       cmd_boolean },
	{ "useragent",       NULL,                 cmd_spec_useragent },
	...
};
```

- 비슷한 코드를 행과 열을 맞춰서 정렬하면 파라메터 값들을 더 쉽게 확인할 수 있고, 탈자로 인한 버그도 손쉽게 찾을 수 있다.

### __05. 의미 있는 순서를 선택하고 일관성 있게 사용하라

- 순서가 중요하지 않은 코드의 경우에는 임의의 순서가 아니라 의미있는 순서로 나열하는 것이 좋다.

ex1 ) 변수의 순서를 HTML 폼에 있는 input 필드의 순서대로 나열하기.

ex2 ) '가장 중요한 것'에서 시작해서 '덜 중요한 것' 순으로 나열하기.

ex3 ) 알파벳 순서대로 나열하기

### __06. 선언문을 블록으로 구성하라

```cpp
class FrontendServer {
	public:
		FrontendServer();
		~FrontendServer();

		// Handlers
		void ViewProfile(HttpRequest* request);
		void SaveProfile(HttpRequest* request);
		void FindFriends(HttpRequest* request);

		// Request/Reply Utilities
		string ExtractQueryParam(HttpRequest* request, string param);
		void ReplyOK(HttpRequest* request, string html);
		void ReplyNotFound(HttpRequest* request, string error);
		
		// Database Helpers
		void OpenDatabase(string location, string user);
		void CloseDatabase(string location);
};
```

- 코드들을 논리적 영역에 따라 그룹화하고 주석을 붙여라.

### __07. 코드를 ‘문단’으로 쪼개라

```python
def suggest_new_friends(user, email_password):
		# Get the user's friends' email addresses.
		friends = user.friends()
		friend_emails = set(f.email for f in friends)

		# Import all email addresses from this user's email account.
		contacts = import_contacts(user.email, email_password)
		contact_emails = set(c.email for c in contacts)

		# Find matching users that they aren't already friends with.
		non_friend_emails = contact_emails - friend_emails
		suggested_friends = User.objects.select(email__in=non_friend_emails)

		# Display these lists on the page.
		display['user'] = user
		display['friends'] = friends
		display['suggested_friends'] = suggested_friends

		return render("suggested_friends.html", display)
```

- 코드들이 하는 일을 논리적으로 설명하기 좋게 문단을 나눠라.

### __08. 개인적인 스타일 대 일관성

**★ 일관성 있는 스타일은 '올바른' 스타일보다 더 중요하다.**

### __요약

- If multiple blocks of code are doing similar things, try to give them the same silhouette.
- Aligning parts of the code into “columns” can make code easy to skim through.
- If code mentions A, B, and C in one place, don’t say B, C, and A in another. Pick a
  meaningful order and stick with it.
- Use empty lines to break apart large blocks into logical “paragraphs.”