---
title: "Chap6. Structural Analysis"
description: "Disecting member of Truss"
menuTitle : "Structure Analy."
weight: 6
date: 2021-02-15T22:02:56+09:00
draft: false
katex: true
markup: mmark
tags: ["statics"]
hidden: true

LastModifierDisplayName : "Kevin Lee"
LastModifierEmail : "klee30810@gmail.com"
---

Objectives

- To show how to determine the forces in the members of a truss using the method of joints and the method od sections
- To analyze the forces acting on the members of frames and machines composed of pin-connected members

### 6.1 Simple Trusses

- truss : a structure composed of slender members joined together at their end points

(1) Assumptions for Design => Two-force member

a. All loadings are applied at the joints

- frequently the weight of the members is neglected bacause the force supported by each member is usually much larger than its weight

b. The members are joined together by smooth pin : provided the center lines of the joining members are concurrent

(2) Simple Truss : if a truss can be constructed by expanding the basic triangular truss

### 6.2 The Method of Joints

- Based on the fact that if the entire truss is in equilibrium, then each of its joints is also in equilibrium. => force equilibrium equations can be used to obtain the member forces acting on each joint.
- Always satrt at a joint having at least one known force and at most two unkonwn forces
- Always assume the unknown member forces acting on the joint's free=body diagram to be in tension => positive scalars for tension members and negative scalars for compression members.

### 6.3 Zero-Force Members

- used to increase the stability of the truss during construction and to provide added support if the loading is changed.
- if only two members form a truss joint and no external load or support reaction is applied to the joint, the two members must be zero-force members
- if three members form a truss joint for which two of the members are collinear, the third member is a zero-force member provided no external force or support reaction is applied to the joint.

### 6.4 The Method of Sections

- If the truss is in equilibrium then any segment of the truss is also in equilibrium.
- the line of action of each member force is specified from the geometry of the truss, since the force in a member is along its axis.
- the member forces acting on one part of the truss are equal but opposite to those acting on the other part (Newton's 3rd Law)
- Always assume that the unknown member forces at the cut section are tensile forces on the member. => positive scalars in tension and negative scalars in compression

### 6.6 Frames and Machines

- frame : support loads / machines : contain moving parts
- provided a frame or machine contains no more supports or members than are necessary to prevent its collapse, the forces acting at the joints and supports can be determined by applying the equations of equilibrium to each of its members.

(1) Free-Body Diagrams

a. Isolate each part by drawing its outlined shape, then show all the forces and couple moments that act on the part.

b. Identify all the two-force members in the structure and represent their free-body diagrams as having two equal but opposite collinear forces acting on their points.

c. Forces common to any two contacting members act with equal magnitudes but opposite sense on the respective members