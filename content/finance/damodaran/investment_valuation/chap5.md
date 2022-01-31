---
title: "Chap5. Option Pricing Theory & Models"
description: "Option Pricing Theory & Models"
menuTitle : "Option Pricing"
weight: 5
date: 2021-12-05T14:41:48+09:00
draft: false
katex: true
markup: mmark
tags: ["finance"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

# Option Pricing Theory & Models

- option
  - derive their value from the values of other assets.
  - cash flows on the assets are contingent on the occurrence of specific events



## Basics of Option Pricing

---

- option : provides the holder with the right to buy or sell a specified quantity of an underlying asset at a fixed price (called a strike price or an exercise price)
- the holder can choose not to exercise the right and allow the option to expire



## Call & Put Options : Description & Payoff Diagrams

---

- Call option

  - the buyer of the option the right to buy the underlying asset at a fixed price, called the strike or the exercise price, at any time prior to the expiration date of the option. 
  - The net profit on the investment is the difference between the gross profit and the price paid for the call initially

  ![image](/images/finance/damodaran/investment_valuation/chap5/1.png)

- Put option

  - the buyer of the option the right to sell the underlying asset at a fixed price, again called the strike or exercise price, at any time prior to the expiration date of the option.
  - negative net payoff if the value of the underlying asset exceeds the strike price, and has a gross payoff equal to the difference between the strike price and the value of the underlying asset if the asset value is less than the strike price

  ![image](/images/finance/damodaran/investment_valuation/chap5/2.png)



## Determinants of Option Value

---

1. Current Value of the Underlying Asset : an increase in the value of the asset will increase the value of the Calls. Puts, on the other hand, become less valuable as the value of the asset increase.
2. Variance in Value of the Underlying Asset : The higher the variance in the value of the underlying asset, the greater will the value of the option be
   - options are different from other securities since buyers of options can never lose more than the price they pay for them; in fact, they have the potential to earn significant returns from large price movements
3. Dividends Paid on the Underlying Asset : the value of a call on the asset is a decreasing function of the size of expected dividend payments, and the value of a put is an increasing function of expected dividend payments
4. Strike Price of Option : the value of the call will decline as the strike price increases, In the case of puts, where the holder has the right to sell at a fixed price, the value will increase as the strike price increases.
5. Time To Expiration On Option : Both calls and puts become more valuable as the time to expiration increases.
   - This is because the longer time to expiration provides more time for the value of the underlying asset to move, increasing the value of both types of options.
6. Riskless Interest Rate Corresponding To Life Of Option : Increases in the interest rate will increase the value of calls and reduce the value of puts.

![image](/images/finance/damodaran/investment_valuation/chap5/3.png)



## American VS European Options : Variables Relating To Early Exercise

---

- A primary distinction between American and European options is that American options can be exercised at any time prior to its expiration, while European options can be exercised only at expiration



## Option Pricing Models

---

### A. The Binomial Model

- to use a combination of risk-free borrowing/lending and the underlying asset to create a portfolio that has the same cash flows as the option being valued.

  $$\triangle=Number\ of\ units\ of\ the\ underlying\ asset =\frac{C_u-C_d}{Su-Sd} \\ C_u:value\ of\ call\ @ S_u$$

$$Value\ of\ call = value\ of\ replicating\ position=current\ value\ of\ underlying\ asset\ *\ option\ delta-borrowing\ needed\ to\ replicate\ the\ option $$

### B. Black-Scholes Model

- The Model

  S = Current value of the underlying asset

  K = Strike price of the option

  t = Life to expiration of the option

  r = Riskless interest rate corresponding to the life of the option

  $$ σ^2 $$ = variance in the ln(value) of the underlying asset

  $$ value\ of \ call=SN(d_1)-Ke^{-rt}N(d_2)\\d_1=\frac{ln(\frac{S}{K}+(r+\frac{σ^2}{2})t)}{σ\sqrt{t}},\quad d_2=d_1-σ\sqrt{t}$$
  - $$e^{-rt}$$ : present value factor, reflects the fact that the exercise price on the call option does not have to be paid until expiration
  - N(d) : cumulative standardized normal distribution, the likelihood that an option will generate positive cash flows for its owner at exercise, i.e., when S>K in the case of a call option and when K>S in the case of a put option
    - replicates the call option is created by buying N(d1) units of the underlying asset, and borrowing Ke-rtN(d2)
  - $$ N(d_1) $$ : the number of units of the underlying asset that are needed to create the replicating portfolio ⇒ **option delta**

- Black-Scholes model requires inputs that are consistent on time measurement

  1. the model works in continuous time, rather than discrete time ⇒ $$exp^{-rt}$$
  2. The variance that is entered into the model also has to be an annualized variance.

- Implied Volatility : variance at which the estimated value will be equal to the market price

  - The only input on which there can be significant disagreement among investors is the variance. While the variance is often estimated by looking at historical data, the values for options that emerge from using the historical variance can be different from the market prices

- Model Limitations & Fixes

  1. Dividends : on the ex-dividend day, the stock price generally declines. Consequently, call options will become less valuable and put options more valuable as expected dividend payments increase

  



















