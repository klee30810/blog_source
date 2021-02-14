---
title: "Chap12. 생각을 코드로 만들기"
menuTitle : "12. Think Coding"
weight: 12
date: 2021-02-15T07:55:05+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- 복잡한 생각을 다른 사람에게 설명할 때 중요하지 않은 자세한 내용 때문에 듣는 사람을 혼동시키는 일이 종종 있다.

- '쉬운 말'로 자신의 생각을 지식이 부족한 사람에게 전달하는 기술은 매우 중요하다.

- 여기에는 설명할 내용을 걸러서 요지만 뽑아내는 능력이 요구된다.

**- 코드도 마찬가지로 '쉬운 말'로 작성되어야 한다.**

- 이 장에서 설명할 코드를 더 명확하게 만드는 간단한 과정은 다음과 같다.

1. 코드가 할 일을 다른 사람에게 설명하듯이 '쉬운 말'로 묘사하라.

2. 이 설명에 들어가는 핵심적인 단어와 문구를 포착하라.

3. 설명과 부합하는 코드를 작성하라.

### __01. 논리를 명확하게 설명하기

```jsx
$is_admin = is_admin_request();
if ($document) {
		if (!$is_admin && ($document['username'] != $_SESSION['username'])) {
				return not_authorized();
		}
} else {
		if (!$is_admin) {
				return not_authorized();
		}
}
// continue rendering the page ...
```

- 예제 코드는 '사용자가 페이지를 볼 수 있는 허가가 있는지를 확인하고, 만약 허가되어 있지 않으면 이를 설명하는 페이지를 반환한다.'
- 예제 코드를 쉬운 말로 묘사하면 다음과 같다.

사용이 허가되는 경우는 두가지다.
1) 관리자다.
2) 관리자는 아니지만 소유하고 있는 문서이다.
그렇지 않으면 허가되지 않는다.

```jsx
if (is_admin_request()) {
		// authorized
} elseif ($document && ($document['username'] == $_SESSION['username'])) {
		// authorized
} else {
		return not_authorized();
}
// continue rendering the page ...
```

- 이를 이용해 수정한 예제 코드는 비어있는 본문 두개를 포함해서 조금 이상해보인다.
- 하지만 코드의 분량이 더 적고, 부정문이 없어서 논리도 더 간단해졌다.
- 즉, 더 이해하기 쉬워졌다.

### __02. 라이브러리를 알면 도움이 된다

```jsx
<div id="tip-1" class="tip">Tip: Log in to see your past queries.</div>
<div id="tip-2" class="tip">Tip: Click on a picture to see it close up.</div>
...
var show_next_tip = function () {
		var num_tips = $('.tip').size();
		var shown_tip = $('.tip:visible');

		var shown_tip_num = Number(shown_tip.attr('id').slice(4));
		if (shown_tip_num === num_tips) {
				$('#tip-1').show();
		} else {
				$('#tip-' + (shown_tip_num + 1)).show();
		}
		shown_tip.hide();
};
```

- **간결한 코드를 작성하는 기술 중 하나는 라이브러리가 제공하는 기능을 잘 활용하는 것이다.**

Describing the code : 
Find the currently visible tip and hide it.
Then find the next tip after it and show that.
If we've run out of tips, cycle back to the first tip.

```jsx
// SOLUTION
var show_next_tip = function () {
		var cur_tip = $('.tip:visible').hide(); // find the currently visible tip and hide it
		var next_tip = cur_tip.next('.tip'); // find the next tip after it
		if (next_tip.size() === 0) { // if we've run out of tips,
				next_tip = $('.tip:first'); // cycle back to the first tip
		}
		next_tip.show(); // show the new tip
};
```

### __03. 논리를 쉬운 말로 표현하는 방법을 더 큰 문제에 적용하기

![image](/images/compu/readable_code/chap12/Untitled.png)

```python
def PrintStockTransactions():
		stock_iter = db_read("SELECT time, ticker_symbol FROM ...")
		price_iter = ...
		num_shares_iter = ...

		# Iterate through all the rows of the 3 tables in parallel.
		while stock_iter and price_iter and num_shares_iter:
				stock_time = stock_iter.time
				price_time = price_iter.time
				num_shares_time = num_shares_iter.time

				# If all 3 rows don't have the same time, skip over the oldest row
				# Note: the "<=" below can't just be "<" in case there are 2 tied-oldest.
				if stock_time != price_time or stock_time != num_shares_time:
						if stock_time <= price_time and stock_time <= num_shares_time:
								stock_iter.NextRow()
						elif price_time <= stock_time and price_time <= num_shares_time:
								price_iter.NextRow()
						elif num_shares_time <= stock_time and num_shares_time <= price_time:
								num_shares_iter.NextRow()
						else:
								assert False # impossible
				continue

		assert stock_time == price_time == num_shares_time

		# Print the aligned rows.
		print "@", stock_time,
		print stock_iter.ticker_symbol,
		print price_iter.price,
		print num_shares_iter.number_of_shares

		stock_iter.NextRow()
		price_iter.NextRow()
		num_shares_iter.NextRow()
```

- 예제 코드는 주식 구매현황을 기록하는 시스템이다.
- 각 거래는 다음의 네 가지 데이터 조각으로 포함한다.

time(구매의 정확한 날짜와 시간), ticker_symbol(예, GOOG), price(예, $600), number_of_share(예, 100)

- 문제는 이 값들이 시간과 세개의 값이 하나씩 포함된 세개의 서로 다른 테이블에 따로 존재하며 이를 합쳐야 한다는 것이다.
- 더 큰 문제는 어떤 행은 아예 존재하지 않으며, 존재하지 않는 행을 무시하면서 세 개의 테이블에서 time값이 동일한 행을 연결해야한다는 것이다.
- 이를 해결하기 위해 하려는 일을 쉬운 말로 묘사하면 다음과 같다.

세 개 반복자를 병렬적으로 동시에 읽는다.
어느 행의 time이 일치하지 않으면, 일치할 때 까지 앞으로 한칸씩 나아간다.
일치 된 행을 출력하고, 다시 앞으로 나아간다.
일치하는 행이 더 이상 없을 때 까지 이를 반복한다.

- 이 중 가장 지저분하게 작성되는 곳은 두 번째 줄이며 이를 함수로 분리할 수 있다.

```python
def PrintStockTransactions():
		stock_iter = ...
		price_iter = ...
		num_shares_iter = ...

		while True:
				time = AdvanceToMatchingTime(stock_iter, price_iter, num_shares_iter)
				if time is None:
						return

				# Print the aligned rows.
				print "@", time,
				print stock_iter.ticker_symbol,
				print price_iter.price,
				print num_shares_iter.number_of_shares

				stock_iter.NextRow()
				price_iter.NextRow()
				num_shares_iter.NextRow()
```

```python
def AdvanceToMatchingTime(stock_iter, price_iter, num_shares_iter):
		# Iterate through all the rows of the 3 tables in parallel.
		while stock_iter and price_iter and num_shares_iter:
				stock_time = stock_iter.time
				price_time = price_iter.time
				num_shares_time = num_shares_iter.time

				# If all 3 rows don't have the same time, skip over the oldest row
				if stock_time != price_time or stock_time != num_shares_time:
						if stock_time <= price_time and stock_time <= num_shares_time:
								stock_iter.NextRow()
						elif price_time <= stock_time and price_time <= num_shares_time:
								price_iter.NextRow()
						elif num_shares_time <= stock_time and num_shares_time <= price_time:
								num_shares_iter.NextRow()
						else:
								assert False # impossible
						continue
						assert stock_time == price_time == num_shares_time
				return stock_time
```

- 이 함수가 하는 작업을 다시 한번 쉬운 말로 묘사하면 다음과 같다.

각 테이블의 현재 행에서 time을 확인한다. 값이 모두 같으면 작업이 완료된 것이다.  그렇지 않으면 값이 '뒤쳐진' 행을 한 칸 전진시킨다.
행이 모두 동일한 time을 가질 때까지 혹은 반복자 중의 하나가 끝에 이를 때까지 작업을 반복한다

- 여기에서 주목할 부분은 이 설명이 우리가 해결하려는 상위 문제와 관련된 어떠한 사항도 언급하지 않는다는 점이다.

- 따라서 변수명을 더 간단하고 일반적이게 바꿀 수도 있다.

```python
def AdvanceToMatchingTime(row_iter1, row_iter2, row_iter3):
		while row_iter1 and row_iter2 and row_iter3:
				t1 = row_iter1.time
				t2 = row_iter2.time
				t3 = row_iter3.time

				if t1 == t2 == t3:
						return t1

				tmax = max(t1, t2, t3)

				# If any row is "behind," advance it.
				# Eventually, this while loop will align them all.
				if t1 < tmax: row_iter1.NextRow()
				if t2 < tmax: row_iter2.NextRow()
				if t3 < tmax: row_iter3.NextRow()

		return None # no alignment could be found
```

### __요약

- **자신의 문제를 쉬운 말로 설명할 수 없으면, 해당 문제는 무언가 빠져 있거나 아니면 제대로 정의되지 않은 것이다.**
- This chapter discussed the simple technique of describing your program in plain English and using that description to help you write more natural code. This technique is deceptively simple, but very powerful. Looking at the words and phrases used in your description can also help you identify which subproblems to break off.
- But this process of “saying things in plain English” is applicable outside of just writing code. For example, one college computer lab policy states that when a student needs help debugging his program, he first has to explain the problem to a dedicated teddy bear in the corner of the room. Surprisingly, just describing the problem aloud can often help the student figure out a solution. This technique is called “rubber ducking.”
- Another way to look at it is this: if you can't describe the problem or your design in words, something is probably missing or undefined. Getting a program (or any idea) into words can really force it into shape.