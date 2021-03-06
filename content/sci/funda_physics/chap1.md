---
title: "Chap1. 힘과 운동"
description: "힘과 운동"
menuTitle : "힘과 운동"
weight: 1
date: 2021-02-17T02:02:07+09:00
draft: false
katex: true
markup: mmark
tags: ["physics"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

### 1. 속도와 가속도

---

#### (1) 위치의 변화와 변위

- 운동 : 물체의 위치가 시간에 따라 변하는 현상
  - 기준점을 먼저 정하고, 그 기준점으로부터의 거리와 방향을 함께 나타내야 함
- 이동거리 : 물체가 이동한 경로의 길
- 변위 : 물체의 이동경로는 생각하지 않고 출발점과 도착점을 최단거리로 연결하는 선분의 길이와 방향을 함께 나타낸 것
  - 스칼라 : 크기만으로 완전히 표현되는 물리량
  - 벡터 : 크기와 방향을 함께 나타내는 물리량



#### (2) 속력과 속도

1) 속력 (speed) : 물체가 단위 시간(1초)에 이동한 거리

$$ 속력=\frac{이동거리}{걸린시간},\quad v=\frac{x}{t} $$

- 평균 속력 : \\(평균속력 = \frac{이동거리}{걸린시간}\\)
  - 이동하는 동안 속력의 변화를 나타내지는 않음
- 순간 속력 : 속력이 변하면서 이동할 때 평균 속도로는 알 수가 없다

2) 속도 (velocity) : 빠르기와 방향을 함께 나타내는 물리량

$$ 속도=\frac{변위}{시간},\quad v=\frac{x}{t} $$

- 일정한 속도 : 일정한 속력과 방향으로 운동

  → 빠르기가 더 빨라지지도 더 느려지지도 않는 똑같은 속력 & 운동 경로가 곡선이 아니라 직선

- 변하는 속도 : 물체의 속력이 변하거나 운동 방향이 변하는 것

3) 상대 속도 : 움직이고 있는 관측자가 느끼는 상대속도

$$ v_{ab}=v_b-v_a $$

4) 등속 직선 운동 : 일직선상을 일정한 속력으로 달리는 물체의 운동

$$ v=\frac{x}{t} $$



#### (3) 가속도

- 단위 시간(1초)에 대한 속도의 변화량

$$ 가속도 = \frac{속도의 변화량}{걸린 시간},\quad a=\frac{v_2-v_1}{t_2-t_1}=\frac{\triangle v}{\triangle t}$$

- 가속도는 속도가 얼마나 빠른가를 나타내는 물리량이 아니라 **속도의 변화가 얼마나 빠르게 일어나는가**를 나타내는 물리량
- 운동 방향이 변화할 때도 적용된다.



### 2. 운동의 법칙

---

#### (1) 힘

- 힘 : 물체의 모양이나 운동 상태를 변화시키는 요인

  → 물체의 변형이나 운동 상태의 변화의 정도로 힘이 얼마나 크게 작용하였는지 알 수 있다.
  
- 힘의 3요소 : 크기, 방향, 작용점

- 힘의 표시 : 힘의 작용선 이용

- 힘의 합성과 분해 

  - 같은 작용선 상 : 사칙연산
  - 다른 작용선 상 : 평행사변형법, 삼각형법
  - 힘의 분해 : 각도에 따라 cos, sin가능

- 힘의 평형 : 한 물체에 크기가 같고 방향이 반대인 두 힘이 동시에 같은 작용선 상에서 작용하면 힘의 효과가 나타내지 않는다 → 힘의 합력이 0이므로 힘이 없는 것과 같다

#### (2) 운동의 제1법칙 (관성의 법칙)

- 관성 : 물체는 외부로부터 힘이 작용하지 않으면 원래의 운동 상ㅌ를 계속 유지하려는 성질
- **뉴턴의 운동의 제 1법칙 (관성의 법칙)** : 외부로부터 물체에 힘이 작용하지 않거나 작용하더라도 힘의 합력이 0이면 운동하고 있는 물체는 계속 등속도 운동을 하고 정지하는 물체는 계속 정지해 있다.

→ 관성의 법칙으로 물체의 운동상태를 변화시키는 원인이 힘 이라는 것과 정지 상태에 있는 물체와 등속도 운동하는 물체는 모두 가속도가 0인 운동 상태라는 것을 밝히게 됨

- 질량으로 그 물체의 관성의 크기를 나타낼 수 있다.

#### (3) 운동의 제2법칙 (가속도의 법칙)

- 운동하는 물체에 힘이 작용하면 물체의 속력이 변하거나 운동 방향이 바뀌게 됨 ⇒ 가속도 생성
- 힘과 가속도 : 물체에 작용하는 힘이 더 클수록 운동 상태의 변화가 크게 일어남 \\(F \propto \alpha\\)
- 질량과 가속도 : 물체에 작용하는 힘이 일정할 때 물체의 질량과 가속도는 반비례 \\(α \propto \frac{1}{m}\\)
- **뉴턴의 운동의 제 2법칙 (가속도의 법칙)**: 물체의 가속도는 물체에 작용하는 힘의 크기에 비례하여 물체의 질량에 반비례한다. 가속도의 방향은 물체에 작용하는 힘의 방향과 같다. \\(F=ma\\)
- 한 물체의 여러 힘이 동시에 작용할 경우 여러 힘을 합한 합력 하나만 작용하는 것으로 취급
- 관성의 법칙은 2법칙의 특수한 경우

$$ 1N=1kg\cdot m/s^2 $$

#### (4) 운동의 제3법칙 (작용 반작용의 법칙)

- 힘은 물체의 상호작용에 의해 나타나는 것이며, 단독으로 존재하는 것이 아니라 반드시 쌍으로 존재하는 것
- $$ F_{AB}=-F_{BA} $$
- **뉴턴의 운동의 제3법칙 (작용 반작용의 법칙)**: 한 물체의 어던 힘이 작용하면 반드시 상대편 물체에 크기가 같고 방향이 반대인 힘이 반작용으로 나타난다.
- 모든 상호작용에서 힘들은 항상 쌍으로 존재한다.

#### (5) 여러가지 힘과 운동

1) 중력에 의한 운동

- 만유인력의 법칙 : 질량이 \\(m_1, m_2\\)인 두 물체가 거리 r 만큼 떨어져 있을 떄, 이들 물체 사이에도 서로 잡아당기는 힘이 있다. 이 힘은 두 물체의 질량의 곱에 비례하고 두 물체 사이의 거리의 제곱에 반비례

$$F=G\frac{m_1m_2}{r^2},\quad G=6.67\times10^{-11}N\cdot m^2/kg^2 $$

- 중력 : 지구 표면에서 물체가 일정한 가속도로 낙하하는 것은 그 물체에 일정한 힘이 계속 작용한다는 것 의미 → 지구라는 거대한 물체와 지구 표면상에 있는 물체 사이에 작용하는 힘
- 자유 낙하운동 : 손에 들고 있던 물체를 가만히 놓아 낙하시킬 때처럼, 물체가 정지 상태로부터 중력만을 받으면서 낙하하는 운동 → 등가속도 운동

2) 마찰력이 작용할 때의 운동

- 물체를 밀거나 당길 때, 물체에 작용하는 힘의 크기가 어느 정도 되기까지는 물체가 움직이지 않는다.

- 마찰력 : 물체와 접촉면 사이에서 운동을 방해하는 힘, 물체의 운동 방향과 반대 방향으로 작용

- 정지 마찰력 : 물체를 잡아당겨도 물체가 움직이지 않을 때의 마찰력

  → 마찰력에는 한계가 있어서 물체에 작용하는 힘의 크기가 이 한계의 마찰력보다 커지면 물체가 움직이기 시작

  - 움직이는 순간의 마찰력 크기 : 최대 정지 마찰력 \\(f_s=μ_sN\\)

- 운동 마찰력 : 물체가 운동하고 있는 동안의 마찰력 \\(f_k=μ_kN\\)

- **마찰 계수는 접촉면의 종류나 상태에 따라 다르며, 접촉면의 넓이와 무관**

3) 탄성력

- 탄성 : 물체가 힘을 받아 변형될 때 본래의 모양으로 되돌아가려는 성질
- 후크의 법칙 \\(F=kx\\)
  - 탄성력은 후크의 법칙에서 구한 외부에서 작용한 힘과 크기가 같고 방향이 반대인 힘, \\(F=-kx\\)
  - 탄성력은 변형의 크기에 비례하며, (-)부호는 음수를 뜻하는 것이 아니라 탄성력이 외부에서 물체에 작용하는 힘과 반대로 작용한다는 것











