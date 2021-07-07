---
title: "Basic 1 - Players of Financial Market"
description: "Basic1 of Ajae - Players of Financial Market"
menuTitle : "Basic 1"
weight: 2
date: 2021-07-06T21:04:01+09:00
draft: false
katex: true
markup: mmark
tags: ["finance"]
hidden: false

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

## Players of Financial Market - 1

- 셀 사이드 : 고객에게 금융 상품과 서비스를 판매 (투자은행)
  - 상업 은행 : 예금 대출
  - 투자 은행 : 주식, 채권 발행 및 매매, 인수합병, 리서치, 자산관리
- 바이 사이드 : 상품과 서비스를 구매 (뮤추얼펀드, 사모펀드, 헤지펀드, 연기금 등)

### Investment Banking

- Primary Market 업무 : 주식/채권 발행, 기업공개(IPO), 인수합병(M&A), 기업자문 등
  - 구조 : 주로 산업군으로 팀이 분류, 특정 종류의 딜로 분류
  - 업무 : 회사 가치 평가, 산업 분석, 재무제표 분석, 금융 모델링 (퀀트 X, 엑셀), 프레젠테이션 준비

- Research

  - $0 commission 브로커 출현 등으로 수익 구조 악화
  - MiFID 2 규제 : 거래 수수료와 리서치 비용을 별도 구분하도록 강제
  - 데이터 과학 등의 대두로 인한 고도화, buy-side 자체 인력화

  |                | 뮤추얼 펀드           | 헤지 펀드                    | 프랍 트레이딩        |
  | -------------- | --------------------- | ---------------------------- | -------------------- |
  | 운용 자금 출처 | 다수의 개인           | 소수 부유층 (규제요건)       | 파트너 자신          |
  | 투자 상품      | 주식/채권만           | 주식/채권/외환/원자재        | 제한 없음            |
  | 전략 제한      | 제한적                | 제한 없음                    | 제한 없음            |
  | 정보 공개      | 투명 공개             | 제한적 공개                  | 비공개               |
  | 기본 수수료    | 운용액 1~2%           | 운용액의 1~2%                | -                    |
  | 성과 수수료    | 없음                  | 운용액의 15~20%              | -                    |
  | 광고           | 공개 광고 자유        | 공개 광고 금지               | 필요 없음            |
  | 대표적 기관    | Vanguard, Fidelity 등 | Bridgewater, AQR, Elliott 등 | Jump Trading, DRW 등 |

- Hedge Fund의 종류 : Long/short Equity, Market Neutral, Merger Arbitrage, Convertible Arbitrage, Event-Driven, Credit, Fixed-Income Arbitrage, Global Macro, Short-Only, Quantitative
- 사모 펀드 (Private Equity) : 헤지 펀드와 많은 면에서 유사, 금융 시장에서 매매하는 것이 아닌 회사에 직접 투자

-------

## Players of Financial Market - 2

- ETF Exchange Traded Fund : 주식의 형태로 거래되는 펀드
  - 일반인들이 모든 회사를 직접 사서 분산투자 하기 힘드므로 그 배스킷을 만들고 소유권인 주식 판매
  - ETN은 중개사의 파산 위험에 노출되나, ETF는 기초자산이 담보로 있어서 안전
  - 예) S&P500을 추종하는 펀드들이 테슬라의 편입에 맞춰 테슬라를 구매해야함, 테슬라 주가 상승
- 연기금 (Pension Fund) / 국부 펀드(Sovereign Wealth Fund) / 기부 펀드 (Endowment Fund)
- 브로커 : 매수자와 매도자를 연결시키고 대신 주문 체결해서 수수료를 받는 중재자, 본인 포지션 없음
- 딜러 : 매매하려는 사람의 반대 포지션을 취해줌, 마켓 메이커(Liquidity Provider)
- 마켓메이커 : 매수/매도 호가를 지속적으로 시장에서 제시해줌
- 보험사
- 중앙은행
- 주식회사 : 배당, share buyback, insider transaction 등

---

## Players of Financial Market - 3 Hedge Funds

- Long short Equity : 저평가 된 주식을 매수하고 고평가된 주식을 공매도 하는 전략 → 시장과 상관없이 펀드매니저의 실력
  - long 선호 : 오르는 시장에서 숏 해서 잃으면 자금 회수를 많이 하므로
  - Market Neutral : 롱포지션과 숏포지션이 정확히 1대1 (Relative Value로 분류되기도 함)
  - 130-30 : 롱 130%, 숏 30%로 100% 주식 long
- Event-driven Fund : 인수, 합병, 파산, 채무불이행 등의 이벤트를 중심으로 매매하는펀드
  - Distressed securities : 파산, 구조조정 등의 사건을 겪는 회사의 저평가된 채권을 매수
  - Merger Arbitrage : 인수 합병시의 불확실성을 이용한 투자 (인수 당하는 쪽 상승)
    - 일반적으로 인수를 위해 인수기업 측에서 타겟기업에 시장가격 이상을 줘야 함
    - 인수 의향 발표부터 완료까지 1) 시간이 오래 걸리고 2) 불확실성 존재 → 인수 완료가격보다 저평가되어 거래됨
    - 매매 방식 : 인수 합병이 성공적 일 것 같으며 타겟기업 매수, 불발일 것 같으면 타겟기업 공매도, 주식 교환 인수(stock-for-stock)일 경우 타겟기업 매수, 인수기업 공매도
    - 필요한 전문성: 인수 합병이 전략적으로, 재무적으로 합리적인가? 타겟기업에 더 높은 가격을 제시할 경쟁자는 없는가?, 인수합병 시도가 성공할 확률은 얼마나 되는가? 인수합병에 걸리는 시간은 얼마나 되는가? 인수합병시도가 무산되었을 때 시나리오는 어떠한가? 반독점범 및 각종 규제, 정치적 고려로 인해 불발에 그칠 확률은 있는가? 주주 총회에서 반대로 무산될 확률은 있는가? 경영진이 반대할 확률은 있는가?
  - Activist Fund : 타겟 기업의 지분을 취득하여 영향력을 행사함으로서 수익을 올리는 전략
    - 기업의 지배구조 변화 : CEO 혹은 이사진 해임, 교체 등
    - 보유한 현금 배당, 자사주 매입
    - 기업 내의 특정 부서나 비즈니스 매각
    - 다른 기업 인수 합병
    - 일반적으로 지분 취득 후 기업 경영진에 접근하여 요구 → 받아들이지 않을 시 뉴스 및 미디어에서 PR 싸움 → 주주 총회에서 표결 싸운
- Relative Value Fund : 차익거래
  - Convertible Bond Arbitrage
    - Convertible Bond : 전환사채, 일반 회사채처럼 정기적으로 이자를 주는데, 주식으로 변환할 수 있는 권리 가짐 → **채권 + 콜옵션**
    - 매매 방법
      1. Cash-flow Arbitrage : 전환사채 롱, 주식 숏
         - 예) 현금 10, 주식 공매도 100 > 전환 사채 110 매수 / 공매도 이자 2, 배당 2%, 전환사채 이자 5% 가정시 현금 10으로 1(10%)이익
      2. 옵션 매매 : 콜옵션 비슷하게 매매 됨
      3. Credit Arbitrage : Credit Default Swap → 회사 부도 방지
  - Fixed Income Arbitrage : 채권이나 이자율 등을 이용한 차익거래
  - MBS Arbitrage (Mortgage-Backed Securities) : 모기지 담보 증권, 사람들의 주택담보대출증서를 모아서 증권화, Prepayment Option 때문에 이자율에 대한 옵션 상품처럼 가능 → 일찍 갚을 수도 있어서 더 짧은 기간 이자를 받을 risk 존재
  - Index Arbitrage : 지수에 편입/퇴출되는 기업을 매수/공매도 하는 방식
    - 인덱스ETF가 테슬라를 언제 사는가?
      - 덩치가 크므로 2번에 걸쳐 편입하기도 하고 이를 발표함, 각자의 성문화된 규정에 따라 매수
  - Statistical Arbitrage : 페어 트레이딩을 다원화, 수백개의 주식 종목, 베타와 리스크 팩터를 제거
  - Volatility Arbitrage
    - implied volatility : 내재 변동성, 실제 변동성이 아니라 앞으로 30일 동안 얼마나 변할 것 인가?에 대하여 시장 참여자들이 어떻게 생각하는가?
    - future realized volatility : 미래의 실제 변동성
    - 가장 기본 거래 : 트레이더가 예상하는 미래 변동성 예측치보다 옵션의 내재 변동성이 낮으면 옵션 매수, 반대면 옵션 매도

- Global Macro Fund : 세계 각 국의 경제, 정치 상황을 토대로 주식, 채권, 외환, 원자재 등의 시장에 투자
  - 이자율, 정치 상황, 국내외 정책 변화, 국제 무역, 환율

---

## Players of Financial Market -4 Endowment Fund

하버드 대학처럼 자산 분배 (Brazenor, R. (2008) Investing Like the Harvard and Yale Endowment Fund)

- 대안 투자 상품 : 원자재, 외환, 부동산, 천연자원, 벤처 캐피탈, 사모펀드, 미술품, 와인, 골동품, 코인  → 40% 이상 투자

- 수익의 3요소 - 수익률 = α + β x 주식시장 수익률 + 무작위성

  - β : 포트폴리오가 주식시장에 얼마나 연동되어 있는지 → 1이면 주식시장과 같이 움직임
    - 기울기 : 높을 수록 주식시장 흐름을 많이 탐 → 레버리지
  - α : 종목 선정하는 실력
    - 절편 : 절대 실력 → 실력
  - 주식을 순매수하는 펀드나, 지수추종하는 인덱스펀트/ETF일수록 자산배분이 중요해지고, 롱숏 에퀴티, 마켓 뉴트럴 전략 등에서는 종목선정이 중요해진다.
  - 개인 레벨 : 주식을 순매수만 한다면(공매도 안하거나, 코스피 숏해서 헤지 안하면) 자산 분배 중요시 필요
  - 스프레드 매매(항공주 매수, 기술주 공매도)를 한다면 해당 아이디어(가설매매)만 집중 가능 - 주식시장 영향을 덜 받으므로

- 실력 쌓기 (알파) + 변동성 감수 (베타) + 손실확률 감수 (파산위험의 고금리 채권) + 유동성 포기(쉽게 현금화 할수 없는 자산은 유동성 프리미엄 존재)

- 사모 펀드 투자 : PEX, PSP 사모펀드 ETF

  |          Asset Class          |                    Benchmark                     | ETF  |
  | :---------------------------: | :----------------------------------------------: | :--: |
  |           US Equity           |                 S&P 500 TR Index                 |      |
  |   Non US-Developed Equities   |                MSCI EAFE TR Index                |      |
  |   Private Equity - Illiquid   | Cambridge Associates Private Equity Buyout Index |      |
  |    Private Equity - liquid    |                  LPX50 TU Index                  |      |
  |       Emerging Equities       |          MSCI Emerging Markets TR Index          |      |
  |         Global Bonds          |       Barclays Global Aggregate Bond Index       |      |
  |          Real Estate          |          DJ Global Select REIT TR Index          |      |
  |       Natural Resources       |      S&P Global Natural Resources TR Index       | GNR  |
  | Hedge Funds & Managed Futures |          Credit Suisse Hedge Fund Index          |      |
  |        Global Equities        |               MSCI World TR Index                |      |

  - 기관이므로 매년 자산배분 공개 : https://www.harvard.edu/about-harvard/harvard-glance/endowment

    













