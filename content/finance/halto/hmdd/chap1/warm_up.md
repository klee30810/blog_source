---
title: "Warm up"
description: "Warm up Before Quant Investment"
menuTitle : "Warm up"
weight: 1
date: 2021-09-26T09:23:13+09:00
draft: false
katex: true
markup: mmark
tags: ["finance"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

# 1. Book Structure

---

- 투자의 3 기술 : 자산 배분, 마켓 타이밍, 종목 선정
  - 자산 배분 : 기대 수익률과 위험 수준이 다른 자산군(주식, 채권, 금, 원자재, 부동산, 암호화폐 등)에 투자 자금을 배분 ⇒ 높은 수익이 아니라 적절한 수익을 유지하면서 리스크를 최소화
  - 마켓 타이밍 : 단기적으로 높은 수익이 기대되는 자산군의 비중을 늘리고 단기적으로 낮은 수익이 기대되는 자산군의 비중을 낮춰서, 리스크는 자산 배분과 같은 수준으로 유지하되 수익을 높이려는 전략 (가격, 계절성, 경제지표, 밸류에이션 ...)
  - 종목 선정 : 자산군 내에서 가장 유망하다고 판단하는 종목에 자금을 투자해서 수익 극대화를 추구하는 행위 (밸류, 모멘텀, 퀄리티, 저변동성, 계절성)





# 2. Main Terms

---

- 연복리수익률(Compound annual growth rate, CAGR) : 원금이 2배가 되는데 걸리는 시간

  - 72의 법칙 : 72를 연복리수익률로 나누면 원금이 2배 걸리는 시간을 어림잡을 수 있다.

- 최대 낙폭 (MDD, maximum drawdown) : 특정 투자 기간 중 겪을 수 있는 가장 큰 손실

  $MDD\ =\ (최저점/전고점) - 1$

  - MDD가 커지면 원금 복구가 어렵고 투자자의 심리적 충격이 너무 커져서 정상적인 투자를 지속하기 어려움

- 샤프지수 (Sharpe Ratio) : 변동성을 고려한 수익률

  $샤프 지수\ =\ \frac{(연복리수익률\ -\ 현금수익)}{변동성}$

- 초과수익 : 리스크 없는 안전수익 대비 초과수익 VS 벤치마크(비교 대상) 대비 초과수익

- 리밸런싱 : 주기적으로 실제 투자 비중을 목표 비중과 맞추는 작업

  - 중장기 자산배분은 6~12개월에 한번, 동적자산배분은 1달 1회 리밸런싱
  - 어느 날에 하느냐에 따라 차이가 엄청 남 : 기존 전략 평균적으로 **월말 3일, 월초 2일 수익이 가장 유리하다. 월 4일차부터는 피해라!** ⇒ 월말월초 효과가 아닌가? 직장인 월급이 월말월초에 집중되고 이때 투자량이 늘어나지 않을까?
  - 장점 : 실제 투자 비중이 목표 비중과 같아지는 것.  /.   단점 : 이때 수수료 등 거래비용이 발생
  - 20개 종목에 500 만원씩 투자하고 6개월에 한번 리밸런싱 & 몇 종목이 오르고 하락함. 전체는 1.2억으로 증가해서 수익률 20% 달성한 경우
    - 각 주식 금액을 12,000/20 = 600만원으로 맞추는 것이 목표 ⇒ 금액을 정확히 맞추는 것이 쉽지 않은데 595만원, 605만원 등으로 맞춰도 대세에 지장이 없음

- 투자 팩터 : 장기적으로 주가지수 대비 초과수익을 낼 수 있게 하는 요인

  1. 밸류(가치) : 기업의 가격(시가총액 또는 주가)을 펀더멘탈 지표(이익, 순자산, 현금흐름, 매출 등)와 비교하는 팩터 → 저평가, 고평가
  2. 퀄리티(우량주) : 기업의 수익성, 안전성, 변동성, 자본 활용 능력 등을 계량화 → 우량주, 비우량주
  3. 모멘텀 팩터 : 세계 주식시장 대부분에서는 최근 가격이 오른 주식이 계속 오르는 모멘텀 경향 존재
     - 한국 시장은 이 경향이 약한 편이고, 특히 영업이익과 순이익이 증가하는 기업의 주식 수익률이 매우 높은 편



# 3. 퀀트 투자란 무엇인가?

---

- 가치투자 종교 : 가끔 시장이 비이성적인 판단을 내려서 특정 주식이 저평가 또는 고평가 되는 경우가 있다고 믿음. 가치 투자자는 일시적으로 저평가된 주식을 사서 정상 가격 또는 고평가도니 가격으로 상승하면 매도
- 기술적 투자 종교 : 투자 자산을 사고파는 투자자는 인간이고, 인간의 심리는 일정한 패턴을 따르며 그 패턴이 자산의 차트 또는 가격과 거래량에 반영된다고 믿는다. 인간 심리 패턴은 몇 천년이 지나도 크게 변하지 않는다고 생각.
  - 추세를 믿는 경향이 강하며, 기업의 가치는 이미 가격에 반영되어 있으므로 과거 가격, 거래량 및 차트 패턴을 중시한다
- 매크로 투자 종교 : 투자 자산의 수익은 경제와 밀접한 관련이 있다고 생각하며, 경제 지표 또는 흐름과 동향을 분석해서 어떤 자산이 잘나가고 못나갈지 예측하고 다음 투자처를 찾아냄.
  - 대부분 경제의 큰 그림을 보기 때문에 개별 기업의 재무제표를 살피지 않음
- 정보 투자 종교 : 뉴스나 전문가들에게서 어떤 주식이 오를지 팁을 얻음
- 패시브 투자 종교 : 이런저런 노력을 해서 고수익을 내려고 시도하기보다는 ETF나 인덱스펀드 등을 매수해 저렴한 수수료로 시장 수익 자체를 추종
- 어떤 투자 종교도 수익률이 변하는 구간이 있고, 고수들은 손실 발생 구간에 손실을 최소화하는 동시에 전략을 포기하지 않고 꾸준히 버텨서, 전략이 다시 높은 수익률을 벌어주는 구간에 인내심을 보상받는다.
- **퀀트 투자 : 구체적인 매수와 매도, 보유 규칙을 따르는 "규칙 기반(rule-based) 투자다. 퀀트 투자의 규칙은 기업의 재무 데이터, 자산의 가격 등 계량화가 가능한 수치만 사용"**
  - 퀀트 투자자는 다양한 투자 종교가 주장하는 내용을 열심히 분석해서 최대한 계량화가 가능한 전략으로 바꾸고, 이 전략을 과거에 적용했으면 어느 정도의 수익을 벌었는지, 잘 안 풀리는 구간에는 어느 정도 잃었는지 등을 분석하는 백테스트를 진행한다.



# 4. 왜 퀀트투자인가?

---

- 퀀트 투자는 구체적인 규칙을 따르기 때문에, 데이터만 있으면 어제 투자를 시작한 초보자도 당장 따라 할 수 있다. 
- 객관화가 가능하다
- 매수와 매도 전략이 명확하므로 백테스트를 통해 과거 수익, MDD, 손실 만회 기간, 보유종목을 확인할 수 있다.
- 투자는 경험=실력 이라는 원칙이 통하지 않는다.
- 투자를 망치는 심리에서 최대한 벗어난다.

















 
































