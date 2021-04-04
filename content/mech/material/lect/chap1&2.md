---
title: "Chap1, 2. Statics Review"
description: "Statics Review"
menuTitle : "Statics Review"
weight: 1
date: 2021-04-04T10:32:01+09:00
draft: false
katex: true
markup: mmark
tags: ["Material Mechanics"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

- 4대 힘 : Tension & Compression (Axial Loading) / Bending (중립축에 90도)  /  Shear (단변적 평행방향 힘, 각도 변화)  /  Twist
- 외력에 대한 deformation field 구하기 : 
  - Governing Equation \\(\triangledown σ + \overrightarrow{b} = \rho \frac{\partial v}{\partial t}\\),
  - Stress-strain equation \\(σ=D\varepsilon\\), 
  - Displacement - strain equation \\(\varepsilon = \frac{\partial u}{\partial x}\\)

### Statics Review → Equilibrium

---

##### Analysis of load acting on physical system

- Do not experience acceleration
- Newton's Law
  1. An object tends to either **remain at rest or continue to move at constant velocity**
  2. \\(\sum\overrightarrow{F}=m\overrightarrow{a}\\): resultant force로 표현가능
  3. 작용 & 반작용 : 같은 크기 반대방향 힘, *작용점이 같아야 함*
     * 반작용 : 움직이지 않는 부분은 같은 크기 반대방향 힘이 걸림 → 가장 안 움직이는 점을 반력 작용점으로.
- 6개의 힘 요소 : translation & moment ⇒ \\(F_x, F_y, F_z, M_x, M_y, M_z\\)
- Rigid body : An object which **does not go through deformation**(But internal force exists)
- Deformable body : An object which **can be deformed (Shape Change)**
- displacement : Even the same deformation does not have the same effect (different shape)
- strain
  - normal : **힘의 방향이 가해지는 면에 수직, element의 각도 변화 없음**, \\(\\varepsilon = \frac{δ u}{L_0}\\)
  - shear : **힘의 방향이 가해지는 면에 평행**, \\(γ = \frac{dx}{L}=\tanθ\approx\theta\\)
- 외력 : contact 필요     /     Body force : contact 필요 없음

![iamge](/images/mech/material/lect/1.png)

$$x:Σ F=(σ_{xx}+\triangle σ_{xx})\triangle y\triangle z -σ_{xx}\triangle y\triangle z\\+(σ_{yx}+\triangle σ_{yx})\triangle x\triangle z -σ_{yx}\triangle x\triangle z\\+(σ_{zx}+\triangle σ_{zx})\triangle x\triangle y -σ_{zx}\triangle x\triangle y+B_x=M\overrightarrow{a_x}\\⇒ \div dV\\ ⇒x:\frac{\triangle σ_{xx}}{\triangle x}+\frac{\triangle σ_{yx}}{\triangle y}+\frac{\triangle σ_{zx}}{\triangle z} + b_x = \rho \overrightarrow{a_x} \\⇒ y:\frac{\triangle σ_{xy}}{\triangle x}+\frac{\triangle σ_{yy}}{\triangle y}+\frac{\triangle σ_{zy}}{\triangle z} + b_y = \rho \overrightarrow{a_y}\\ ⇒ z:\frac{\triangle σ_{xz}}{\triangle x}+\frac{\triangle σ_{yz}}{\triangle y}+\frac{\triangle σ_{zz}}{\triangle z} + b_z = \rho \overrightarrow{a_z} \\ b=0 \because no\ weight \\ \rho\overrightarrow{a}=0\because static$$









