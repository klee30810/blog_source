---
title: "Chap9. Center of Gravity & Centroid"
description: "Center of Gravity & Centroid"
menuTitle : "Centroid"
weight: 9
date: 2021-02-15T22:03:03+09:00
draft: false
katex: true
markup: mmark
tags: ["statics"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

### Objectives

- To discuss the concept of center of gravity, center of mass, and the centroid
- To show how to determine the location of the center of gravity and centroid for a system of discrete particles and a body of arbitray shape.
- To use the theorems of Pappus and Guldinus for finding the surface area and volume for a body having axial symmetry.
- To present a method for finding the resultant of a general distributed loading and show how it applies to finding the resultant force of a pressure loading caused by a fluid

### 9.1 Center of Gravity, Center of Mass, and the Centroid of a Body

### (1) Center of Gravity

- A body is composed of infinite particles, and is located within a gravitational field. Each particle's weight forms an approximately parallel force => total weight of the body wich passes through the center of gravity G.

$$W = \int dW, \quad \overline{x} = \frac{\int \widetilde{x}dW}{\int dW}, \quad \overline{y} = \frac{\int \widetilde{y}dW}{\int dW}, \quad \overline{z} = \frac{\int \widetilde{z} dW}{\int dW} \\ \widetilde{x} : the\ coordinates\ of\ each\ particle\ in\ the\ body$$

### (2) Center of Mass

 $$\overline{x} = \frac{\int \widetilde{x}dm}{\int dm}, \quad \overline{y} = \frac{\int \widetilde{y}dm}{\int dm}, \quad \overline{z} = \frac{\int \widetilde{z} dm}{\int dm}$$ 

### (3) Centroid of a Volume

 $$\overline{x} = \frac{\int \widetilde{x}dV}{\int dV}, \quad \overline{y} = \frac{\int \widetilde{y}dV}{\int dV}, \quad \overline{z} = \frac{\int \widetilde{z} dV}{\int dV}$$ 

### (4) Centroid of an Area

$$\overline{x} = \frac{\int \widetilde{x}dA}{\int dA}, \quad \overline{y} = \frac{\int \widetilde{y}dA}{\int dA}$$

### (5) Centroid of a Line

 $$\overline{x} = \frac{\int \widetilde{x}dL}{\int dL}, \quad \overline{y} = \frac{\int \widetilde{y}dL}{\int dL}, \quad dL = \sqrt{(dx)^2 + (dy)^2}$$ 

## 9.3 Theorems of Pappus and Guldinus

### (1) Surface Area

 $$dA = 2\pi rdL,\ A = 2\pi \int rdL=w\pi \overline{r}L=\theta \overline{r}L$$ 

- The area of a surface of revolution equals the product of the length of the generating curve and the distance traveled by the centroid of the curve in generating the surface area

### (2) Volume

$$dV = 2\pi rdA,\ V=2\pi \int rdA = 2\pi \overline{r}A=\theta \overline{r}A$$ 

- The volume of a body of revolution equals the product of the generating area and the distance traveled by the centroid of the area in generating the colume

### (3) Composite Shapes

 $$A=\theta \sum(\overline{r}L), \quad V=\theta \sum(\overline{r}A)$$ 

## 9.4 Resultant of a General Distributed Loading

### (1) Magnitude of Resultant Force

 $$F_R=\int_A p(x,y)dA = \int_V dV = V$$ 

### (2) Location of Resultant Force

 $$\overline{x} = \frac{\int_A xp(x,y)dA}{\int_A p(x,y)dA} = \frac{\int_V xdV}{\int_V dV}, \quad \overline{y} = \frac{\int_A yp(x,y)dA}{\int_A p(x,y)dA} = \frac{\int_V ydV}{\int_V dV}$$ 

## 9.5 Fluid Pressure

- \\(p=\gamma z = \rho gz​\\)  : valid only for incompressible fluid, Pascal's law
- If the surface is horizontal, the loading will be uniform
- The resultant for tilted loading can be determined by finding the volume under the loading curve or  \\(F_R = \gamma \overline{z} A​\\), where $\\(\overline{z}​\\) is the depth to the centroid of the plate area. The line of action passes through the centroid of the volume.