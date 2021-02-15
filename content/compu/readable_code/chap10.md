---
title: "Chap10. 상관없는 하위문제 추출"
menuTitle : "10. Extract Under"
weight: 10
date: 2021-02-15T07:55:00+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]
hidden: "true"


LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- 큰 흐름과 관계가 적은 하위 문제를 적극적으로 발견해서 추출하라.

1. 주어진 함수나 코드 블록을 보고, 스스로에게 질문하라. "상위 수준에서 본 이 코드의 목적은 무엇인가?"

2. 코드의 모든 줄에 질문을 던져라. "이 코드는 직접적으로 코드가 해결하기 위한 목적을 위해서 존재하는가?

   혹은 그 목적을 위해서 필요하긴 하지만 그 자체와 직접적으로 상관없는 하위 문제를 해결하는가?"

3. 만약 본래 목적과 직접적으로 관련되지 않은 하위 문제를 해결하는 코드의 분량이 상당히 많으면, 이를 추출해서 별도의 함수로 만들어라.

- 이렇게 추출된 코드는 자신이 호출되는 이유를 알 필요가 없어야 한다.

### __01. 소개를 위한 예: findClosestLocation()

```jsx
// Return which element of 'array' is closest to the given latitude/longitude.
// Models the Earth as a perfect sphere.
var findClosestLocation = function (lat, lng, array) {
		var closest;
		var closest_dist = Number.MAX_VALUE;
		for (var i = 0; i < array.length; i += 1) {
				// Convert both points to radians.
				var lat_rad = radians(lat);
				var lng_rad = radians(lng);
				var lat2_rad = radians(array[i].latitude);
				var lng2_rad = radians(array[i].longitude);

				// Use the "Spherical Law of Cosines" formula.
				var dist = Math.acos(Math.sin(lat_rad) * Math.sin(lat2_rad) +
									 Math.cos(lat_rad) * Math.cos(lat2_rad) *
									 Math.cos(lng2_rad - lng_rad));
				if (dist < closest_dist) {
						closest = array[i];
						closest_dist = dist;
				}
		}
		return closest;
};
```

- 예제 코드의 주요 목적은 주어진 점과 가장 가까운 장소를 찾는 것이다. 하지만 예제 안에 있는 대부분의 코드들이 '구 위에 있는 두 개의 위도/경도 점 사이의 거리를 계산한다'는 상위 문제와 상관없고 복잡하기만 한 하위 문제를 다룬다.
- 이런 코드는 별도의 함수로 따로 추출하는 편이 좋다.

```jsx
var spherical_distance = function (lat1, lng1, lat2, lng2) {
		var lat1_rad = radians(lat1);
		var lng1_rad = radians(lng1);
		var lat2_rad = radians(lat2);
		var lng2_rad = radians(lng2);
		// Use the "Spherical Law of Cosines" formula.
		return Math.acos(Math.sin(lat1_rad) * Math.sin(lat2_rad) +
										 Math.cos(lat1_rad) * Math.cos(lat2_rad) *
										 Math.cos(lng2_rad - lng1_rad));
};

var findClosestLocation = function (lat, lng, array) {
		var closest;
		var closest_dist = Number.MAX_VALUE;
		for (var i = 0; i < array.length; i += 1) {
				var dist = spherical_distance(lat, lng, array[i].latitude, array[i].longitude);
				if (dist < closest_dist) {
				closest = array[i];
				closest_dist = dist;
				}
		}
		return closest;
};
```

- 별도의 함수로 추출하면 복잡한 하위 문제에 방해 받지 않고 상위 수준의 목적에 집중할 수 있으니 전반적인 코드의 가독성이 좋아진다.
- 또한 별도로 추출한 함수는 독립적인 테스트를 수행하는데도 유용하다.

### __02. 순수한 유틸리티 코드

- 문자열 변경, 해시테이블 사용, 파일 읽기/쓰기와 같이 프로그램이 수행하는 일에는 매우 기본적인 작업을 포괄하는 핵심적인 집합이 있다.
- 이러한 '기본적인 유틸리티'는 대부분 해당 언어의 내장 라이브러리 안에 있다.
- 하지만 이런 작업을 수행하는 코드가 없어서 "라이브러리에 이런 함수가 있었으면 좋겠어!"라는 생각이 든다면 이를 함수로 작성하라.
- 이를 잘 정리해두면 다른 프로젝트에서도 사용할 수 있는 그럴듯한 유틸리티 코드 모음을 만들수 있다.

### __03. 일반적인 목적의 코드

```jsx
// dictionary example
ajax_post({
		url: 'http://example.com/submit',
		data: data,
		on_success: function (response_data) {
				var str = "{\n";
				for (var key in response_data) {
				str += " " + key + " = " + response_data[key] + "\n";
				}
				alert(str + "}");
				// Continue handling 'response_data' ...
		}
});

// extracted function
var format_pretty = function (obj) {
		var str = "{\n";
		for (var key in obj) {
				str += " " + key + " = " + obj[key] + "\n";
		}
		return str + "}";
};
```

- 예제 코드는 디버깅할 때 필요한 정보를 메시지 상자로 표시하는 목적을 수행하는데, 대부분의 코드가 이를 '예쁘게' 출력하는데 사용되고 있다.
- 이럴 때는 '예쁘게' 출력하는 부분만 따로 추출해서 함수로 만들 수 있다.
- 이렇게 하면 함수를 호출하는 코드를 간단하게 만들고, 이 함수를 다른 곳에서도 간편하게 사용할 수 있다는 장점이 있다.
- 또한 필요할 이 함수를 훨씬 손쉽게 개선할 수 있다는 장점도 있다.
- 별도로 분리된 작은 함수를 다룰 때는 기능을 더하고, 가독성을 개선하고, 코너케이스를 다루는 일이 상대적으로 쉽게 느껴지기 때문이다.

```jsx
// Updated Version
var format_pretty = function (obj, indent) {
		// Handle null, undefined, strings, and non-objects.
		if (obj === null) return "null";
		if (obj === undefined) return "undefined";
		if (typeof obj === "string") return '"' + obj + '"';
		if (typeof obj !== "object") return String(obj);

		if (indent === undefined) indent = "";

		// Handle (non-null) objects.
		var str = "{\n";
		for (var key in obj) {
				str += indent + " " + key + " = ";
				str += format_pretty(obj[key], indent + " ") + "\n";
		}
		return str + indent + "}";
};
```

### __04. 일반적인 목적을 가진 코드를 많이 만들어라

- 일반적인 코드는 프로젝트의 나머지 부분에서 완전히 분리되므로 개발하고, 테스트하고, 이해하기에 좋다.
- 프로젝트에서 사용하는 코드의 많은 부분이 이렇게 별도의 라이브러리로 만들어질 수 있다면 그렇게 하는 것이 좋다.
- 그렇게 하면 코드의 나머지가 차지하는 크기가 그만큼 줄어들어 프로그래머가 생각해야할 내용도 줄어들기 때문이다.

### __05. 특정한 프로젝트를 위한 기능

```python
# Original
business = Business()
business.name = request.POST["name"]
url_path_name = business.name.lower()
url_path_name = re.sub(r"['\.]", "", url_path_name)
url_path_name = re.sub(r"[^a-z0-9]+", "-", url_path_name)
url_path_name = url_path_name.strip("-")
business.url = "/biz/" + url_path_name
business.date_created = datetime.datetime.utcnow()
business.save_to_database()

# Extracted Version
CHARS_TO_REMOVE = re.compile(r"['\.]+")
CHARS_TO_DASH = re.compile(r"[^a-z0-9]+")
def make_url_friendly(text):
		text = text.lower()
		text = CHARS_TO_REMOVE.sub('', text)
		text = CHARS_TO_DASH.sub('-', text)
		return text.strip("-")

business = Business()
business.name = request.POST["name"]
business.url = "/biz/" + make_url_friendly(business.name)
business.date_created = datetime.datetime.utcnow()
business.save_to_database()
```

- 추출된 하위 문제는 사용하는 프로젝트를 전혀 모르는 것이 가장 이상적이다. 하지만 그렇지 않아도 큰 상관은 없다.
- 특정한 프로젝트 내에서만 사용되는 하위 문제라도 이를 분리하는 것 만으로 큰 도움이 되기 때문이다.

### __06. 기존의 인터페이스를 단순화하기

- 라이브러리가 깔끔한 인터페이스를 제공하면 누구나 좋아한다.
- 적은 수의 인수를 받고, 별다른 설정을 요구하지 않으며, 사용하기 간편한 인터페이스가 좋다.
- 이러한 인터페이스는 코드를 우아하게 만들고, 동시에 간단하고 강력하게 만든다.
- 만약 자신이 사용하는 인터페이스가 이렇게 깔끔하지 않다면 깔끔한 '덮개(wrapper)'를 씌워서 보완할 수 있다.

```jsx
var max_results;
var cookies = document.cookie.split(';');
for (var i = 0; i < cookies.length; i++) {
		var c = cookies[i];
		c = c.replace(/^[ ]+/, ''); // remove leading spaces
		if (c.indexOf("max_results=") === 0)
				max_results = Number(c.substring(12, c.length));
}

// TRANSFORMED
var max_results = Number(get_cookie("max_results"));
set_cookie(name, value, days_to_expire);
delete_cookie(name);
```

- 우리는 이상적이지 않은 인터페이스를 그냥 받아들일 이유가 없다.

### __07. 자신의 필요에 맞춰서 인터페이스의 형태를 바꾸기

```jsx
user_info = { "username": "...", "password": "..." }
user_str = json.dumps(user_info)
cipher = Cipher("aes_128_cbc", key=PRIVATE_KEY, init_vector=INIT_VECTOR, op=ENCODE)
encrypted_bytes = cipher.update(user_str)
encrypted_bytes += cipher.final() # flush out the current 128 bit block
url = "http://example.com/?user_info=" + base64.urlsafe_b64encode(encrypted_bytes)
...
```

- 프로그램 안의 많은 코드들은 다른 코드를 지원하려고 존재한다.

```jsx
// Changing Interface
def url_safe_encrypt(obj):
		obj_str = json.dumps(obj)
		cipher = Cipher("aes_128_cbc", key=PRIVATE_KEY, init_vector=INIT_VECTOR, op=ENCODE)
		encrypted_bytes = cipher.update(obj_str)
		encrypted_bytes += cipher.final() # flush out the current 128 bit block
		return base64.urlsafe_b64encode(encrypted_bytes)

//Then, the resulting code to execute the real logic of the program is simple:
user_info = { "username": "...", "password": "..." }
url = "http://example.com/?user_info=" + url_safe_encrypt(user_info)
```

- 이와 같은 '접착(glue)'코드는 프로그램의 실제 논리와 별로 직접적인 상관이 없으므로 따로 분리하여 독자적인 함수로 만드는 것이 좋다.
- 이렇게 하면 프로그램이 수행하는 본래의 논리를 쉽게 파악할 수 있다.

### __08. 지나치게 추출하기

- 코드에 새로운 함수를 더하는 일에는 **분명히 가독성 비용**이 든다.
- 만약 함수를 새로 만들면서 더해지는 가독성 비용이 이를 통해 얻는 이득보다 적다면 이를 수행할 이유가 없다.
- **새롭게 만든 작은 함수들이 다른 프로젝트에서도 사용된다면 추출하는 것이 이득**이 되겠지만 그런 순간이 오기 전까지는 그럴 필요가 없다.

### __요약

- A simple way to think about this chapter is to **separate the generic code from the project-specific code**. As it turns out, most code is generic. By building a large set of libraries and helper functions to solve the general problems, what’s left will be a small core of what makes your program unique.
- The main reason this technique helps is that it lets the programmer focus on smaller, well-defined problems that are detached from the rest of your project. As a result, the solutions to those subproblems tend to be more thorough and correct. You might also be able to reuse them later.

**+추가적인 읽기**

『리팩토링: 존재하는 코드의 설계를 개선하기(Refactoring: Improving the Design of Existing Code)』 - 마틴 파울러

⇒ 여기서는 '메소드 추출' 기법과 다양한 코드 리팩토링 방법을 설명한다.

『스몰토크 최선의 실전 패턴(Smalltalk Best Practice Patterns)』 - 켄트 벡

⇒ 여기서는 코드를 잘개 쪼개서 수많은 함수로 구성하는 방법을 의미하는 '구성된 메소드 패턴(Composed Method Pattern)' 원리를 설명한다.

⇒ 여기서 원리란 '모든 연산을 동일한 추상 수준의 메소드 안에 담기'를 말한다.
