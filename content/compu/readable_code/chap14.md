---
title: "Chap14. 테스트와 가독성"
menuTitle : "14. Test & Readable"
weight: 14
date: 2021-02-15T07:55:12+09:00
draft: false
katex: true
markup: mmark
tags: ["programming"]
hidden: "true"


LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- We’re going to focus on the readability aspect of tests and not get into whether you should write test code before writing real code ("test-driven development”) or other philosophical aspects of test development.

### __01. 읽거나 유지보수하기 쉽게 테스트를 만들어라

- **Test code should be readable so that other coders are comfortable changing or
  adding tests.**

### __02. 이 테스트는 어떤 점이 잘못되었을까?

```cpp
// Sort 'docs' by score (highest first) and remove negative-scored documents.
void SortAndFilterDocs(vector<ScoredDocument>* docs);

void Test1() {
		vector<ScoredDocument> docs;
		docs.resize(5);
		docs[0].url = "http://example.com";
		docs[0].score = -5.0;
		docs[1].url = "http://example.com";
		docs[1].score = 1;
		150 C H A P T E R F O U R T E E N
		docs[2].url = "http://example.com";
		docs[2].score = 4;
		docs[3].url = "http://example.com";
		docs[3].score = -99998.7;
		docs[4].url = "http://example.com";
		docs[4].score = 3.0;

		SortAndFilterDocs(&docs);

		assert(docs.size() == 3);
		assert(docs[0].score == 4);
		assert(docs[1].score == 3.0);
		assert(docs[2].score == 1);
}
```

- There are at least eight different problems with this test code.

### __03. 이 테스트를 더 읽기 쉽게 만들기

- **You should hide less important details from the user, so that more important details are most prominent.**
- This test code clearly violates this rule. Every detail of the test is front and center, like the unimportant minutiae of setting up a vector<ScoredDocument>.
- url, score, docs[] are just details about how the underlying C++ objects are set up, not about what this test is doing at a high level.

```cpp
void MakeScoredDoc(ScoredDocument* sd, double score, string url) {
		sd->score = score;
		sd->url = url;
}
// Using this function, our test code becomes slightly more compact:

void Test1() {
		vector<ScoredDocument> docs;
		docs.resize(5);
		MakeScoredDoc(&docs[0], -5.0, "http://example.com");
		MakeScoredDoc(&docs[1], 1, "http://example.com");
		MakeScoredDoc(&docs[2], 4, "http://example.com");
		MakeScoredDoc(&docs[3], -99998.7, "http://example.com");
		...
}
```

- The parameter "[http://example.com](http://example.com/)" is just an eyesore. it’s just needed to fill out a valid ScoredDocument.
- Another unimportant detail we’re forced to see is docs.resize(5) and &docs[0], &docs[1], and so on.

```cpp
void AddScoredDoc(vector<ScoredDocument>& docs, double score) {
		ScoredDocument sd;
		sd.score = score;
		sd.url = "http://example.com";
		docs.push_back(sd);
}

// Using this function, our test code is even more compact:
void Test1() {
		vector<ScoredDocument> docs;
		AddScoredDoc(docs, -5.0);
		AddScoredDoc(docs, 1);
		AddScoredDoc(docs, 4);
		AddScoredDoc(docs, -99998.7);
		...
}
```

- Let’s describe what our test is trying to do in plain English (chapter 12) :

We have a list of documents whose scores are [-5, 1, 4, -99998.7, 3]. After
SortAndFilterDocs(), the remaining documents should have scores of [4, 3, 1], in that order.

- Nowhere in that description did we mention a vector<ScoredDocument>.

```cpp
⇒ CheckScoresBeforeAfter("-5, 1, 4, -99998.7, 3", "4, 3, 1");
```

- In general, defining a custom minilanguage can be a powerful way to express a lot of
  information in a small amount of space.
- In this case, writing some helper functions to parse a comma-separated list of numbers
  shouldn’t be too hard.

```cpp
void CheckScoresBeforeAfter(string input, string expected_output) {
		vector<ScoredDocument> docs = ScoredDocsFromString(input);
		SortAndFilterDocs(&docs);
		string output = ScoredDocsToString(docs);
		assert(output == expected_output);
}
```

```cpp
vector<ScoredDocument> ScoredDocsFromString(string scores) {
		vector<ScoredDocument> docs;

		replace(scores.begin(), scores.end(), ',', ' ');

		// Populate 'docs' from a string of space-separated scores.
		istringstream stream(scores);
		double score;
		while (stream >> score) {
				AddScoredDoc(docs, score);
		}

		return docs;
}

string ScoredDocsToString(vector<ScoredDocument> docs) {
		ostringstream stream;
		for (int i = 0; i < docs.size(); i++) {
				if (i > 0) stream << ", ";
				stream << docs[i].score;
		}
		return stream.str();
}
```

- 

### __04. 읽기 편한 에러 메시지 만들기

- what happens when that assert(output == expected_output) line fails? It produces an error message like this:

```cpp
Assertion failed: (output == expected_output),
function CheckScoresBeforeAfter, file [test.cc](http://test.cc/), line 37.
```

- Most languages and libraries have more sophisticated versions of assert()

```cpp
BOOST_REQUIRE_EQUAL(output, expected_output)
```

* BETTER ASSERT() IN OTHER LANGUAGES

- In Python, the built-in statement assert a == b produces a plain error message like:

```python
File "[file.py](http://file.py/)", line X, in <module>
		assert a == b
AssertionError
```

- Instead, you can use the **assertEqual()** method in the **unittest** module:

```python
import unittest

class MyTestCase(unittest.TestCase):
		def testFunction(self):
				a = 1
				b = 2
				self.assertEqual(a, b)

if **name** == '**main**':
unittest.main()

# which produces an error message like:

File "[MyTestCase.py](http://mytestcase.py/)", line 7, in testFunction
		self.assertEqual(a, b)
AssertionError: 1 != 2
```

- Whichever language you’re using, there’s probably a library/framework (e.g., XUnit) available to help you. It pays to know your libraries!

- Nicer Error Message

```cpp
CheckScoresBeforeAfter() failed,
		Input: "-5, 1, 4, -99998.7, 3"
		Expected Output: "4, 3, 1"
		Actual Output: "1, 3, 4"
```

```cpp
// Implementing!!
void CheckScoresBeforeAfter(...) {
		...
		if (output != expected_output) {
				cerr << "CheckScoresBeforeAfter() failed," << endl;
				cerr << "Input: \"" << input << "\"" << endl;
				cerr << "Expected Output: \"" << expected_output << "\"" << endl;
				cerr << "Actual Output: \"" << output << "\"" << endl;
				abort();
}
```

### __05. 좋은 테스트 입력값의 선택

- **In general, you should pick the simplest set of inputs that completely exercise the
  code.**
- A simpler value is just -1. (If -99998.7 was meant to be “a very negative number,” a better value would have been something crisp like -1e100.)
- **Prefer clean and simple test values that still get the job done.**
- Large inputs like these do a good job of exposing bugs such as buffer overruns or others you might not expect.
- But code like this is big and scary to look at and not completely effective in stress-testing the code. Instead, it’s more effective to construct large inputs programmatically, constructing a large input of (say) 100,000 values.
- Rather than construct a single “perfect” input to thoroughly exercise your code, it’s often  easier, more effective, and more readable to write multiple smaller tests.

```cpp
// four tests for SortAndFilterDocs():
CheckScoresBeforeAfter("2, 1, 3", "3, 2, 1"); // Basic sorting
CheckScoresBeforeAfter("0, -0.1, -10", "0"); // All values < 0 removed
CheckScoresBeforeAfter("1, -2, 1, -2", "1, 1"); // Duplicates not a problem
CheckScoresBeforeAfter("", ""); // Empty input OK
```

### __06. 테스트 함수에 이름 붙이기

- Picking a good name for a test function can seem tedious and irrelevant, but don’t resort to meaningless names like Test1(), Test2(), and the like.
- Instead, you should use the name to describe details about the test. In particular, it’s handy if the person reading the test code can quickly figure out:
  • The class being tested (if any)
  • The function being tested
  • The situation or bug being tested
- possibly with a “Test_” prefix
- The test function name is effectively acting like a comment. Also, if that test fails, most testing frameworks will print out the name of the function where the assertion failed, so a descriptive name is especially helpful.
- When it comes to naming helper functions in your test code, it’s useful to highlight whether the function does any assertions itself or is just an ordinary “test-unaware” helper

### __07. 이 테스트 코드는 무엇이 잘못되었는가?

```cpp
void Test1() {
		vector<ScoredDocument> docs;
		docs.resize(5);
		docs[0].url = "http://example.com";
		docs[0].score = -5.0;
		docs[1].url = "http://example.com";
		docs[1].score = 1;
		docs[2].url = "http://example.com";
		docs[2].score = 4;
		docs[3].url = "http://example.com";
		docs[3].score = -99998.7;
		docs[4].url = "http://example.com";
		docs[4].score = 3.0;

		SortAndFilterDocs(&docs);

		assert(docs.size() == 3);
		assert(docs[0].score == 4);
		assert(docs[1].score == 3.0);
		assert(docs[2].score == 1);
}
```

1. The test is very long and full of unimportant details. You can describe what this test is
   doing in one sentence, so the test statement shouldn’t be much longer.
2. Adding another test isn’t easy. You’d be tempted to copy/paste/modify, which would make
   the code even longer and full of duplication.
3. The test failure messages aren’t very useful. If this test fails, it will just say Assertion failed:
   docs.size() == 3, which doesn’t give you enough information to debug it further.
4. The test tries to test everything at once. It’s trying to test both the negative filtering and
   the sorting functionality. It would be more readable to break this into multiple tests.
5. The test inputs aren’t simple. In particular, the example score -99998.7 is “loud” and gets
   your attention even though there isn’t any significance to that specific value. A simpler
   negative value would suffice.
6. The test inputs don’t thoroughly exercise the code. For example, it doesn’t test when the
   score is 0. (Would that document be filtered or not?)
7. It doesn’t test other extreme inputs, such as an empty input vector, a very large vector, or
   one with duplicate scores.
8. The name Test1() is meaningless—the name should describe the function or situation
   being tested.

### __08. 테스트에 친숙한 개발

- Ideal code to test has a well-defined interface, doesn’t have much state or other “setup,” and doesn’t have much hidden data to inspect.
- **You start designing your code so that it’s easy to test!** Fortunately, coding this way also means that you create better code in general. Test-friendly designs often lead naturally to well-organized code, with separate parts to do separate things.

**TEST-DRIVEN DEVELOPMENT**
Test-driven development (TDD) is a programming style where you write the tests before you write the real code. TDD proponents believe this process profoundly improves the quality of the nontest code, much more so than if you write the tests after writing the code.

This is a hotly debated topic that we won’t get into. At the very least, we’ve found that just keeping testing in mind while writing code helps make the code better.

But regardless of whether you employ TDD, the end result is that you have code that tests other code. The goal of this chapter is to help you make your tests easier to read and write.

- Of all the ways to break up a program into classes and methods, the most decoupled ones are usually the easiest to test.

![image](/images/compu/readable_code/chap14/Untitled.png)

![image](/images/compu/readable_code/chap14/Untitled1.png)

### __09. 지나친 테스트

- **Sacrificing the readability of your real code, for the sake of enabling tests.**
  Designing your real code to be testable should be a win-win situation: your real code
  becomes simpler and more decoupled, and your tests are easy to write. But if you have to
  insert lots of ugly plumbing into your real code just so you can test it, something’s wrong.
- **Being obsessive about 100% test coverage.** Testing the first 90% of your code is often
  less work than testing that last 10%. That last 10% might involve user interface, or dumb
  error cases, where the cost of the bug isn’t really that high and the effort to test it just isn’t worth it.
  The truth is that you’ll never get 100% coverage anyhow. If it’s not a missed bug, it might
  be a missed feature or you might not realize that the spec should be changed.
  Depending on how costly your bugs are, there’s a sweet spot of how much development
  time it’s worth spending on test code. If you’re building a website prototype, it might not
  be worth writing any test code at all. On the other hand, if you’re writing a controller for
  a spaceship or medical device, testing is probably your main focus.
- **Letting testing get in the way of product development.** We’ve seen situations where
  testing, which should be just one aspect of a project, dominates the whole project. Testing becomes some sort of god to be appeased, and coders just go through the rituals and motions without realizing that their precious engineering time might be better spent
  elsewhere.

### __요약

- The top level of each test should be as concise as possible; ideally, each test input/output
  can be described in one line of code.
- If your test fails, it should emit an error message that makes the bug easy to track down
  and fix.
- Use the simplest test inputs that completely exercise your code.
- Give your test functions a fully descriptive name so it’s clear what each is testing. Instead
  of Test1(), use a name like Test_\<FunctionName>_\<Situation>.
