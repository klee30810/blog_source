---
title: "Chap6. Stack"
description: "Stack"
menuTitle : "Stack"
weight: 6
date: 2021-03-07T14:35:11+09:00
draft: false
katex: true
markup: mmark
tags: ["Data Structure"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

## 1. 스택의 이해와 ADT정의

---

- 스택은 ‘먼저 들어간 것이 나중에 나오는 자료구조’로써 초코볼이 담겨있는 통에 비유할 수 있다. ⇒ **LIFO(Last-in, First-out)**
  - Push : 초코볼 통에 초코볼을 넣는다
  - Pop : 초코볼 통에서 초코볼을 꺼낸다. (데이터 추출 & 삭제)
  - Peek : 이번에 꺼낼 초코볼의 색이 무엇인지 통 안을 들여다 본다 (데이터 추출만)

### ADT 정의 : 배열 혹은 연결 리스트 기반

```cpp
void StackInit(Stack * pstack);
// 스택의 초기화 진행, 스택 생성 후 제일 먼저 호출되어야 함

int SIsEmpty(Stack * pstack);
// 스택인 빈 경우 TRUE, 그렇지 않으면 FALSE 

void SPush(Stack * pstack, Data data);
// 스택에 데이터를 저장, 매개변수 data로 전달된 값을 저장

Data SPop(Stack * pstack);
// 마지막에 저장된 요소를 반환하고 삭제
// 본 함수 호출을 위해 데이터가 하나 이상 존재함이 보장되어야 함 => SIsEmpty 사용

Data SPeek(Stack * pstack);
// 마지막 저장요소를 반환하되 삭제하지 않음
// 본 함수 호출을 위해 데이터가 하나 이상 존재함이 보장되어야 함 => SIsEmpty 사용
```

## 2. 스택의 배열 기반 구현

---

### 구현의 논리

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap6/1.png)

- index는 0이 되지 않고 -1이 되어야 참조가 안되어 있음을 확인 가능
- 인덱스가 0인 위치를 스택의 바닥으로 정의해야 배열 길이에 상관없이 바닥의 인덱스 값이 동일해진다.
- push : Top을 위로 한 칸 올리고, Top이 가리키는 위치에 데이터 저장
- pop : Top이 가리키는 데이터를 반환하고, Top을 아래로 한 칸 내림

### 배열 기반 스택 구현

```cpp
void StackInit(Stack * pstack)
{
	pstack->topIndex = -1; // 01은 빈 스택 의미
}

int SIsEmpty(Stack * pstack)
{
	if(pstack->topIndex == -1)  // 빈 경우 TRUE
		return TRUE;
	else
		return FALSE;
}

void SPush(Stack * pstack, Data data)
{
	pstack->topIndex += 1;  // -1이 초기값이므로 먼저 증가해야 함
	pstack->stackArr[pstack->topIndex] = data;
}

Data SPop(Stack * pstack)
// 배열에서 삭제 의미 : 값이 있어도 유효한 값으로 인정받지 못하면 삭제 된 것
// 어차피 Push되면 새 데이터로 초기화 되므로 미리 초기화 필요 X
{
	int rIdx;

	if(SIsEmpty(pstack))
	{
		printf("Stack Memory Error!");
		exit(-1);
	}

	rIdx = pstack->topIndex;    // 삭제 될 인덱스 저장
	pstack->topIndex -= 1;

	return pstack->stackArr[rIdx];
}

Data SPeek(Stack * pstack)
{
	if(SIsEmpty(pstack))
	{
		printf("Stack Memory Error!");
		exit(-1);
	}

	return pstack->stackArr[pstack->topIndex];
}
```

## 3. 스택의 연결 리스트 기반 구현

---

### 연결 리스트 기반 스택의  논리

![3%20Linked%20List%201%202391bffe280542fe83f0c81d9e03d829/Untitled.png](/images/compu/DS/chap6/2.png)

- **저장된 순서의 역순으로 데이터(노드)를 참조(삭제)**하는 연결 리스트가 바로 연결 기반의 스택이다!

```cpp
// 새 노드를 머리에 추가하고 삭제시 머리부터 삭제하는 단순 연결 리스트와 비슷
void StackInit(Stack * pstack)
{
	pstack->head = NULL;  
}

int SIsEmpty(Stack * pstack)
{
	if(pstack->head == NULL)
		return TRUE;
	else
		return FALSE;
}

void SPush(Stack * pstack, Data data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));

	newNode->data = data;
	newNode->next = pstack->head;

	pstack->head = newNode;
}

Data SPop(Stack * pstack)
{
	Data rdata;
	Node * rnode;

	if(SIsEmpty(pstack)) {
		printf("Stack Memory Error!");
		exit(-1);
	}

	rdata = pstack->head->data;   // 데이터 백업
	rnode = pstack->head;

	pstack->head = pstack->head->next;   // 데이터 헤드 앞당김
	free(rnode);     // 삭제

	return rdata;
}

Data SPeek(Stack * pstack)
{
	if(SIsEmpty(pstack)) {
		printf("Stack Memory Error!");
		exit(-1);
	}

	return pstack->head->data;
}
```

## 4. 계산기 프로그램 구현

---

- **소괄호**를 파악하여 그 부분을 먼저 연산한다.
- **연산자의 우선순위**를 근거로 연산의 순위를 결정한다

### 수식 표기법

- 중위 표기법(infix notation) 예) 5 + 2 / 7
  - 수식 내에 연산의 순서에 대한 정보가 담겨 있지 않다. 그래서 **소괄호**와 **연산자의 우선순위**라는 것을 정의하여 이를 기반으로 연산의 순서를 명시한다.
- 전위 표기법(prefix notation) 예) + 5 / 2 7
  - 수식 내에 연산의 순서에 대한 정보가 담겨 있지 않다. 그래서 소괄호와 연산자의 우선순위라는 것을 정의하여 이를 기반으로 연산의 순서를 명시한다.
- 후위 표기법(postfix notation) 예) 5 2 7 / +
  - 전위 표기법과 마찬가지로 **수식 내에 연산의 순서에 대한 정보가 담겨 있다.** 그래서 소괄호가 필요 없고 연산의 우선순위를 결정할 필요도 없다.

⇒ 우리는 (1) 중위 표기법을 후위 표기법으로 바꾸고 (2) 후위 표기법을 계산하는 계산기를 만들 것이다.

### 중위 표기법의 후위 표기법 변환

1. 피 연산자는 그냥 옮긴다.

2. 연산자는 쟁반으로 옮긴다.

3. 연산자가 쟁반에 있다면 우선순위를 비교하여 처리방법을 결정한다.

   - 쟁반에 위치한 연산자의 우선순위가 높다면
     ∙ 쟁반에 위치한 연산자를 꺼내서 변환된 수식이 위치할 자리로 옮긴다.
     ∙ 그리고 새 연산자는 쟁반으로 옮긴다.
   - 쟁반에 위치한 연산자의 우선순위가 낮다면
     ∙ 쟁반에 위치한 연산자의 위에 새 연산자를 쌓는다

   ⇒ 우선순위가 높은 연산자는 우선순위가 낮은 연산자 위에 올라서서, 우선순위가 낮은 연산자가 먼저 자리를 잡지 못하게 하려는 목적!

4. 마지막까지 쟁반에 남아있는 연산자들은 하나씩 꺼내서 옮긴다

- 주의 :
  - 동일 연산자가 올 시 먼저 있던 것이 일을 진행해야 하므로 이미 있던 것을 옮기고 쌓음
  - 둘 이상의 연산자가 쌓인 경우 쌓인 것 모두 순서대로 비교 해야 함

- 소괄호 고려
  - 후위 표기법의 수식에서는 먼저 연산이 이뤄져야 하는 연산자가 뒤에 연산이 이뤄지는 연산자보다 앞에 위치해야 한다. 따라서 소괄호 앞에 있는 연산자들이 후위 표기법의 수식에서 앞부분에 위치해야 한다.
  - '(' 연산자의 우선순위는 그 어떤 사칙 연산자들보다 낮다고 간주! **(새로운 바닥)**그래서 ')' 연산자가 등장할 때까지 쟁반에 남아 소괄호의 경계 역할을 해야 함!
  - ')' 연산자는 변환된 수식을 옮김

### 후위 표기법 변환 구현

```cpp
// 하나의 함수가 너무 많은 일을 할때 이를 나누는 함수도 큰 의미
// Helper 1
int GetOpPrec(char op)   // 연산 우선순위 반환
{
	switch(op)    // 값이 클 수록 우선순위 높음
	{
	case '*':
	case '/':
		return 5;
	case '+':
	case '-':
		return 3;
	case '(':       // 연산자가 등장 할 때 까지 남아있어야 하므로 가장 낮은 우선순위
		return 1;
	}
// ) 연산자는 소괄호의 끝을 알이므로 쟁반으로 가지 않아서 정의 X

	return -1;   // 등록되지 않은 연산자
}

// Helper 2
int WhoPrecOp(char op1, char op2)   // 두 연산자 우선순위 비교 결과 반환
{
	int op1Prec = GetOpPrec(op1);
	int op2Prec = GetOpPrec(op2);

	if(op1Prec > op2Prec)
		return 1;
	else if(op1Prec < op2Prec)
		return -1;
	else
		return 0;
}

void ConvToRPNExp(char exp[])
{
	Stack stack;
	int expLen = strlen(exp);
	char * convExp = (char*)malloc(expLen+1);  // 변환된 수식을 담을 공간 마련

	int i, idx=0;
	char tok, popOp;
	
	memset(convExp, 0, sizeof(char)*expLen+1);   // 공간 0으로 초기화
	StackInit(&stack);

	for(i=0; i<expLen; i++)   // 한 문자씩 처리
	{
		tok = exp[i];
		if(isdigit(tok))    // 피연산자라면 자리잡기
		{
			convExp[idx++] = tok;
		}
		else                // 연산자라면 stack
		{
			switch(tok)       // stack 순위처리
			{
			case '(':          // 새로운 바닥이므로 무조건 push
				SPush(&stack, tok);
				break;

			case ')':         // 닫는 소괄호는 과정 진행
				while(1)
				{   
					popOp = SPop(&stack);    // 스택에서 연산자를 꺼내서
					if(popOp == '(')         // ( 을 만날 때까지
						break;
					convExp[idx++] = popOp;      // 배열에 저장
				}
				break;

			case '+': case '-': 
			case '*': case '/':    // tok에 저장된 연산자를 스택에 저장
				while(!SIsEmpty(&stack) && WhoPrecOp(SPeek(&stack), tok) >= 0)
					convExp[idx++] = SPop(&stack);

				SPush(&stack, tok);
				break;
			}
		}
	}

	while(!SIsEmpty(&stack))
		convExp[idx++] = SPop(&stack);  // 남은 연산자 모두 이동

	strcpy(exp, convExp);    // 연환된 수식 반환
	free(convExp);
}
```

## 5. 후위 표기법의 계산

---

- 피연산자 두 개가 연산자 앞에 항상 위치하는 구조
- 계산 규칙

- 피연산자는 무조건 스택으로 옮긴다.
- 연산자를 만나면 스택에서 두 개의 피연산자를 꺼내서 계산을 한다.
- 계산 결과는 다시 스택에 넣는다

$(1*2+3)/4 \rightarrow 12*3+4/ \rightarrow 23+4/ \rightarrow 54/$ 

### 후기 표기법 수식 계산 구현

```cpp
int EvalRPNExp(char exp[])
{
	Stack stack;
	int expLen = strlen(exp);    // 문자의 수
	int i;
	char tok, op1, op2;

	StackInit(&stack);

	for(i=0; i<expLen; i++)
	{
		tok = exp[i];

		if(isdigit(tok))           // 피연산자라면
		{
			SPush(&stack, tok - '0');     // char를 숫자로 변환하여 PUSH!
		}
		else           // 연산자라면
		{
			op2 = SPop(&stack);     // 먼저 꺼낸 값이 두 번째 피연산자!
			op1 = SPop(&stack);

			switch(tok)
			{
			case '+':
				SPush(&stack, op1+op2);
				break;
			case '-':
				SPush(&stack, op1-op2);
				break;
			case '*':
				SPush(&stack, op1*op2);
				break;
			case '/':
				SPush(&stack, op1/op2);
				break;
			}
		}
	}   // 최종 결과는 stack에 있음
	return SPop(&stack);
}
```

### 계산기 프로그램의 완성

중위 표기법 수식 → ConvToRPNExp → EvalRPNExp → 연산 결과

- 스택의 활용 : ListBaseStack.c
- 후위 표기법의 수식으로 변환 : InfixToPostfix.c
- 후위 표기법의 수식을 계산 : PostCalculator.c
- 중위 표기법의 수식을 계산 (위의 과정 이어 줌) : InfixCalculator.c
- InfixCalculatorMain.c

```cpp
int EvalInfixExp(char exp[])
{
	int len = strlen(exp);
	int ret;
	char * expcpy = (char*)malloc(len+1);     // 문자열 저장 공간 마련
	strcpy(expcpy, exp);                 // 원본 유지를 위한 복사본 생성

	ConvToRPNExp(expcpy);       // 후위 표기법으로 수식 변환
	ret = EvalRPNExp(expcpy);   // 후위 표기법 계산

	free(expcpy);          // 복사한 저장공간 해제
	return ret;
}
```