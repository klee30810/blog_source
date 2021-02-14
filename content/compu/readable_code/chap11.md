---
title: "Chap11. 한 번에 하나씩"
menuTitle : "11. One for One"
weight: 11
date: 2021-02-15T07:55:03+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- 한 번에 여러 가지 일을 수행하는 코드는 이해하기 어렵다.

**★ 한 번에 하나의 작업만 수행하게 코드를 구성해야 한다. (코드를 '탈파편화(Defragmenting)'하라.)**

- 하나의 함수는 오직 한 가지 작업만 하게 하는 것이 좋지만, 큰 함수를 독자적인 논리적 영역들로 재구성하는 것 만으로 가독성에 도움이 된다.

- 코드가 한 번에 한가지 일만 수행하게 하는 절차는 다음과 같다.

1. 코드가 수행하는 모든 '작업'을 나열한다. 이때 작업은 아주 간단한 일일수도 있고, 아주 모호한 일일수도 있다.

2. 이러한 작업을 분리하여 서로 다른 함수로 혹은 적어도 논리적으로 구분되는 영역에 놓을 수 있는 코드로 만들어라.

### __01. 작업은 작을 수 있다

```jsx
var vote_changed = function (old_vote, new_vote) {
		var score = get_score();

		if (new_vote !== old_vote) {
				if (new_vote === 'Up') {
						score += (old_vote === 'Down' ? 2 : 1);
				} else if (new_vote === 'Down') {
						score -= (old_vote === 'Up' ? 2 : 1);
				} else if (new_vote === '') {
						score += (old_vote === 'Up' ? -1 : 1);
				}
		}
		set_score(score);
};
```

- 예제 코드는 투표 도구의 추천수를 바꾸는 작업을 하는 코드이다. 이때 한명의 사용자는 한 표만 행사할 수 있고 이를 수정할 수 있다.

- 이는 '사용자의 이전 투표값과 현재 투표값을 비교하는 작업'과 '점수를 변경하는 작업'으로 분리될 수 있다.

```jsx
var vote_value = function (vote) {
		if (vote === 'Up') {
				return +1;
		}
		if (vote === 'Down') {
				return -1;
		}
		return 0;
};

var vote_changed = function (old_vote, new_vote) {
		var score = get_score();

		score -= vote_value(old_vote); // remove the old vote
		score += vote_value(new_vote); // add the new vote

		set_score(score);
};
```

- 이렇게 각각의 작업을 분리하고 나면 코드를 더 읽기 편하게 만들 수 있다.

**- 꼭 함수로 만들어서 분리하지 않더라도 두개의 논리적 영역으로 분류하는 것만으로도 가독성이 향상된다.**

### __02. 객체에서 값 추출하기

- 예제 코드는 "도시,나라" 포맷으로 정보를 저장하는 코드이다.

  - 이 코드에 입력되는 주소 값은 아래의 네가지이다.

    'localityName(도시/마을)','SubAdministrativeAreaName(도시권/자치주)', 'AdministrativeAreaName(주/영역)', 'CountryName(나라명)'

  - 이 예제의 문제점은 값들 중 일부 혹은 전체가 없을 수도 있다는 것이다.

```jsx
var place = location_info["LocalityName"]; // e.g. "Santa Monica"
if (!place) {
		place = location_info["SubAdministrativeAreaName"]; // e.g. "Los Angeles"
}
if (!place) {
		place = location_info["AdministrativeAreaName"]; // e.g. "California"
}
if (!place) {
		place = "Middle-of-Nowhere";
}
if (location_info["CountryName"]) {
		place += ", " + location_info["CountryName"]; // e.g. "USA"
} else {
		place += ", Planet Earth";
}
return place;
```

- 예제 코드에서는 비어있는 값을 다루기 위해서 기본값을 주는 방법을 사용했다.

1.  '도시'를 선택할 때 값이 있으면 '도시/마을'을 사용한다. 만약 이 값이 없으면 '도시권/자치주'를 사용하고, 이도 없으면 '주/영역'을 사용한다.
2.  세 값이 모두 없으면 '도시/마을'에 "Middle-of-Nowhere(아무 곳도 아닌 곳)'이라는 기본값을 준다.
3.  만약 '나라'값이 없으면, 기본값으로 "Planet Earth(지구)"라는 값을 준다.

- 무작정 기본값을 주는 코드를 추가한 예제 코드는 지저분해졌고, 기능을 더 추가하면 더 지저분해질 상황이 되었다.
- 그래서 책에서는 '한 번에 한 가지 일' 원리를 적용하기 위해서 코드가 하는 일을 모두 정리했다.

1. location_info 딕셔너리에서 값들을 읽는다.

```jsx
var town = location_info["LocalityName"]; // e.g. "Santa Monica"
var city = location_info["SubAdministrativeAreaName"]; // e.g. "Los Angeles"
var state = location_info["AdministrativeAreaName"]; // e.g. "CA"
var country = location_info["CountryName"]; // e.g. "USA"
```

2. '도시'의 값을 설정하기 위해 정해진 선호도 순으로 값을 읽는다. 아무런 값도 찾을 수 없으면 "아무 곳도 아닌 곳"이라는 기본값을 설정한다.

```jsx
// Start with the default, and keep overwriting with the most specific value.
var second_half = "Planet Earth";
if (country) {
		second_half = country;
}
if (state && country === "USA") {
		second_half = state;
}
```

3. '나라' 값을 설정한다. 값이 없으면 기본값인 "지구"로 설정한다.

```jsx
var first_half = "Middle-of-Nowhere";
if (state && country !== "USA") {
		first_half = state;
}
if (city) {
		first_half = city;
}
if (town) {
		first_half = town;
}
```

4. place를 변경한다.

```jsx
return first_half + ", " + second_half;
```

![images](/images/compu/readable_code/chap11/Untitled.png)

**- 코드를 리팩토링할 때 보통 여러 가지 접근 방법이 있다.**

**- 일단 몇 가지 작업을 분리하면, 코드는 더 생각하기 쉬워지므로 이를 리팩토링 할 수 있는 더 나은 방법이 떠오르게 될 수도 있다.**

```jsx
var first_half, second_half;
if (country === "USA") {
		first_half = town || city || "Middle-of-Nowhere";
		second_half = state || "USA";
} else {
		first_half = town || city || state || "Middle-of-Nowhere";
		second_half = country || "Planet Earth";
}
return first_half + ", " + second_half;
```

### __03. 더 큰 예제

```jsx
// WARNING: DO NOT STARE DIRECTLY AT THIS CODE FOR EXTENDED PERIODS OF TIME.
void UpdateCounts(HttpDownload hd) {
		// Figure out the Exit State, if available.
		if (!hd.has_event_log() || !hd.event_log().has_exit_state()) {
				counts["Exit State"]["unknown"]++;
		} else {
				string state_str = ExitStateTypeName(hd.event_log().exit_state());
				counts["Exit State"][state_str]++;
		}

		// If there are no HTTP headers at all, use "unknown" for the remaining elements.
		if (!hd.has_http_headers()) {
				counts["Http Response"]["unknown"]++;
				counts["Content-Type"]["unknown"]++;
				return;
		}

		HttpHeaders headers = hd.http_headers();

		// Log the HTTP response, if known, otherwise log "unknown"
		if (!headers.has_response_code()) {
				counts["Http Response"]["unknown"]++;
		} else {
				string code = StringPrintf("%d", headers.response_code());
				counts["Http Response"][code]++;
		}

		// Log the Content-Type if known, otherwise log "unknown"
		if (!headers.has_content_type()) {
				counts["Content-Type"]["unknown"]++;
		
		} else {
				string content_type = ContentTypeMime(headers.content_type());
				counts["Content-Type"][content_type]++;
		}
}
```

- 이번 예제는 더 길고 복잡한 코드이다.
- 이 예제는 길고 복잡한만큼 실제로 많은 논리를 포함하며, 심지어 반복되는 코드도 있다. 이런 코드를 읽는 것은 결코 즐겁지 않다.
- 이 코드 전반에 걸쳐서 뒤섞여 있는 별도의 작업들은 다음과 같다.

1. 각 키를 위한 기본값으로 "unknown" 사용하기.

2. HttpDownload의 멤버 중 존재하지 않는 값이 있는지 확인하기.

3. 값을 읽어서 문자열로 변환하기.

4. counts[] 갱신하기.

```jsx
void UpdateCounts(HttpDownload hd) {
		// Task: define default values for each of the values we want to extract
		string exit_state = "unknown";
		string http_response = "unknown";
		string content_type = "unknown";

		// Task: try to extract each value from HttpDownload, one by one
		if (hd.has_event_log() && hd.event_log().has_exit_state()) {
				exit_state = ExitStateTypeName(hd.event_log().exit_state());
		}
		if (hd.has_http_headers() && hd.http_headers().has_response_code()) {
				http_response = StringPrintf("%d", hd.http_headers().response_code());
		}
		if (hd.has_http_headers() && hd.http_headers().has_content_type()) {
				content_type = ContentTypeMime(hd.http_headers().content_type());
		}

		// Task: update counts[]
		counts["Exit State"][exit_state]++;
		counts["Http Response"][http_response]++;
		counts["Content-Type"][content_type]++;
}
```

- 이렇게 작업을 정리하고 다시 작성한 코드는 다음과 같은 목적을 가진 세 개의 영역으로 분리되었다.

1. 읽고자 하는 각각의 값을 위한 기본값을 정의한다. (우리가 관심을 가지고 있는 세 개의 키에 기본값을 설정한다.)

2. HttpDownload에서 각각의 값을 하나씩 읽는다. (각 키에 대한 값이 존재하면 그 값을 읽어서 문자열로 변환한다.)

3. count[]를 갱신한다. (각각의 키/값 짝에 counts[]를 갱신한다.)

- 처음에 나열한 작업이 네 개인데 실제 코드에는 세 개의 영역만 존재하는데 문제 될 것은 전혀 없다.
- **처음에 나열한 작업은 그저 출발점에 해당할 뿐이다. 나열된 대상 중에서 일부만 분리되어도 큰 도움이 된다.**
- 이러한 개선은 별도로 함수를 만들지 않더라도 코드 정리를 도와준다. 예제에서는 처음에 별도의 함수를 만들지 않고 코드를 정리했다.
- **개선은 여러가지 방법으로 행할 수 있으며, 예제에서는 함수를 사용하는 방법도 따로 보여준다.**

```jsx
void UpdateCounts(HttpDownload hd) {
		counts["Exit State"][ExitState(hd)]++;
		counts["Http Response"][HttpResponse(hd)]++;
		counts["Content-Type"][ContentType(hd)]++;
}

string ExitState(HttpDownload hd) {
		if (hd.has_event_log() && hd.event_log().has_exit_state()) {
				return ExitStateTypeName(hd.event_log().exit_state());
		} else {
				return "unknown";
		}
		}
```

- 함수를 따로 정리한 예제 코드에는 중간 결과값을 저장하는 변수가 제거되었으며, 이는 9장과 관련이 있다.
- 두 해결책 모두 코드를 읽는 사람이 한 번에 하나의 일만 생각하게 하므로 가독성을 높여준다.

### __요약

- This chapter illustrates a simple technique for organizing your code: do only one task at a
  time.
- If you have code that’s difficult to read, try to list all of the tasks it’s doing. Some of these tasks might easily become separate functions (or classes). Others might just become logical “paragraphs” within a single function. The exact details of how you separate these tasks isn’t as important as the fact that they’re separated. The hard part is accurately describing all the little things your program is doing.