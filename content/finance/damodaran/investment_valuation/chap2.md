---
title: "2. Approaches to Valuation "
description: "Approaches to Valuation"
menuTitle : "Approaches"
weight: 2
date: 2021-12-05T14:41:41+09:00
draft: false
katex: true
markup: mmark
tags: ["finance"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

# Approaches to Valuation



## 1. Discounted Cashflow Valuation

---

- The the value of an asset to the present value of expected future cashflows on that asset
- the value of any aset is the present value of expected future cashflows that the asset generates

$$ value\ =\ Σ^{t=n}_{t=1}\frac{CF_t}{(1+r)^t}$$ r : reflecting the riskiness of estimated cashflows

- Categorizing DCF Models

  1. equity valuation - value just the equity stake in the business (residual cashflows after metting all expenses, reinvestment needs , tax obligations and net debt payments 

     $$value\ of\ equity = Σ^{t=n}_{t=1}\frac{CF\ to\ equity_t}{(1+k_e)^t}, k_e:\ cost\ of \ equity$$

  2. Firm valuation - value the entire firm, (residual cashflows after metting all operating expenses, reinvestment needs and taxes, but prior to any payments to either debt or equity holders

     $$value\ of\ firm = Σ^{t=n}_{t=1}\frac{CF\ to\ Firm_t}{(1+WACC)^t} $$

     - WACC : the cost of the different components of financing used by the firm, weighted by their market value proportions

  3. Adjusted Present Value(APV) Valuation - valuing each claim on the firm separately

     - value equity -> the value added by debt by considering the PV of the tax benefit
     - cost of debt = pre-tax rate (1-tax rate)
     - WACC = Cost of Equity(E/(E+D)) + Cost of Debt (D/(D+E))

- Applicability & Limitation of DCF Valuation

  - easiest for assets whose cashflows are currently positive and can be estimated with some reliability ofr future period, and where proxy for risk that can be used to obtain discount rate is avaliable

    1.  Firms in trouble : negative earnings and cashflows & expects to lose money for some time in the future => does not work very well

    2. Cyclical Firms : expected future cashflows are usually smoothed out, unless the analyst wants to undertake the onerous task of predicting the timing and duration of econimic recessions and recoveries

    3. Firms with unutilized assets : If a firm has assets that are unutilized, the value of these assets will not be reflected in the value obtained from discounting expected future cashflows

    4. Firms with patents or product option : the value will understate the true value of the firm

    5. Firms in the process of restructuring : often change their structure, using historic data for such firm can give a misleading picture of the firm's value

    6. Firms involved in acquisitions

    7. private firms : no historical prices to check risk

       

## 2. Relative Valuation

---

- we assume that the market is correct in the way it prices stocks, on average, but that it makes errors on the pricing of individual stocks

### Categorizing Relative Valuation Models

1. Fundamentals versus Comparables
   - Using Fundamentals - relates multiples to fundamentals about the firm being valued growth rates in earnings and cashflows, payout ratios and risk.
   - Using Comparables - compare how a firm is valued with how similar firms are priced by the market
     - we have to either explicitly or implicitly control for differences across firms on growth, risk and cash flow measures. In practice, controlling for these variables can range from the naive (using industry averages) to the sophisticated (multivariate regression models where the relevant variables are identified and we control for differences.)
2. Cross Sectional versus Time Series Comparisons
   - 





















