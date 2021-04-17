---
title: "Chap3. Mechanical Properties of Materials"
description: "Mechanical Properties of Materials"
menuTitle : "Mechanical Properties"
weight: 2
date: 2021-04-04T20:07:02+09:00
draft: false
katex: true
markup: mmark
tags: ["Material Mechanics"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

**모든 방향에 힘이 같다는 등방성 가정!!**

##### Force - Displacement Curve

![image](/images/mech/material/lect/chap3/1.png)

- fracture : 재료의 붕괴 시점 ⇒ 예측 불가능
- UTS를 지나면 약한 부분이 necking 
- K : stiffness(강성) → 변형 구간의 변형정도
- strength(강도) → 힘을 얼마나 줄 수 있느냐
- Area = F x d : 흡수 가능한 (견딜 수 있는) 에너지 (toughness) 

![image](/images/mech/material/lect/chap3/2.png)

- work(strain) hardening : 소성영역에서 더 강해짐 / 에너지 흡수 등력이 줄어들음

![image](/images/mech/material/lect/chap3/3.png)

- ductile : large plastic deformation, metal, energy absorption (소성변형 시)

- brittle : almost no plastic deformation(\\(yield\ point \approx\ fracture\\) ), high yield stress, not goot to absorb energy, rock, plastic, carbon

- elastomer : very long elongation, energy absorption (potential), rubber

  → 재료의 temp 상승 : E, strength 감소 / 연신율 증가

- 소재에 저장되는 에너지 : 탄성변형까지!

- Hooke's Law : \\(σ = E\varepsilon\\) ⇒ 탄성 영역에서만!

- modulus of resilience : permanent damage 없이 E 흡수

- modulus of toughness : fracture 까지 E 흡수

- Poisson ratio : 줄어드는 폭의 역학적 비율, **탄성 영역까지만 적용됨**

  $$ \varepsilon_{zz}=\frac{L-L_0}{L_0}, \varepsilon_{xx}=\frac{w-w_0}{w_0},\varepsilon_{yy}=\frac{D-D_0}{D_0}\\ \nu_{yy}=-\frac{\varepsilon_{yy}}{\varepsilon_{zz}},\quad \nu_{xx}=-\frac{\varepsilon_{xx}}{\varepsilon_{zz}} $$

  → 금속 : 0.3, 고무 : 0.5

- Shear stress