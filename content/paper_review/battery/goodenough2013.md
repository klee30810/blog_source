---
title: "The Li-Ion Rechargeable Battery: A Perspective"
date: 2021-02-11T05:35:32+09:00
draft: false
katex: true
markup: mmark
tags: ["battery"]
---

- Author : Kyungmin Lee
- Thesis : Li-Ion Battery is hot trend of technology with its cathode, anode, electrolyte, and separation membrane.
- Citation : Goodenough, J. B., & Park, K. S. (2013). The Li-ion rechargeable battery: a perspective. *Journal of the American Chemical Society*, *135*(4), 1167-1176.

#### 

### Abstract

---

- Each cell of a battery stores electrical energy as chemical energy in two electrodes, a **reductant (anode)** and an **oxidant (cathode)**
  - separated by an **electrolyte** that transfers the ionic component of the chemical reaction inside the cell and forces the electronic component outside the battery
- The chemical reaction of a rechargeable battery must be **reversible** on the application of a charging I and V.
- Critical parameters of a rechargeable battery are **safety**, **density of energy** that can be stored at a specific power input and retrieved at a specific power output, **cycle and shelf life, storage efficiency, and cost of fabrication**
- Conventional rechargeable battery have **solid electrode & liquid electrolyte**
- The positive electrode (cathode) consists of a host framework into **which the mobile (working) cation is inserted reversibly over a finite solid−solution range** which limits the amount of charge per electrode formula unit and is reduced at higher current by the rate of transfer of the workding ion across the interfaces.
- the difference between energies of the LUMO and the HOMO of the electrolyte, i.e., **electrolyte window**, determines the maximum voltage for a long shelf and cycle life.
- Li-ion rechargeable battery uses an **organic electrolyte** with a larger window, which increase the density of stored energy
- Anode or cathode electrochemical potentials outside the electrolyte window can increase V, but they **require formation of a passivating surface layer** that must be permeable to Li+ and capable of adapting rapidly to the changing electrode surface area as the electrode changes volume during cycling. 
- A passivating surface layer adds to the **impedance of the Li+** transfer across the electrode/electrolyte interface and **lowers the cycle life** of a battery cell.
- Moreover, passivation layer on the anode robs Li from the cathode irreversibly.
- Improvement can be doen through by investigating and controlling electrode **passivation layers**, improving the rate of Li+ transfer **across electrode/electrolyte interfaces**, identifying electrolytes **with larger windows while retaining a Li+ conductivity** \\(σ_{Li} > 10^{-3}S cm^{-1}\\), synthesizing electrode morphologies that **reduce the size of the active particles** while pinning them on current collectors of **large surface area** accessible by the electrolyte, **lowering the cost** of cell fabrication, , **designing displacement-reaction anodes** of higher capacity that allow a safe, fast charge, and designing **alternative cathode** hosts
- Beyond approach : using electrode hosts with **two-electron redox centers**; replacing the cathode hosts by **materials that undergo displacement reactions (e.g. sulfur)** by liquid cathodes that may contain **flow-through redox molecules**, or by catalysts for air cathodes; developing a **Li+ solid electrolyte separator membrane** that allows an organic and aqueous liquid electrolyte on the anode and cathode sides, respectively, and **imaginative morphologies**



### Introduction

---

- Li-Ion rechaeable battery (LIB) : for how long a shelf and cycle life, with how rapid a response to a power outage or fluctuation in the grid, and with how large a capacity at a competitive cost?
- The output current **I** and/or time Δt to depletion of the stored energy in a battery can be **increased** **by enlarging the area of the electrodes or connecting cells in parallel**; the voltage **V** for a desired power P = IV by **connecting cells in series**.
- the management of the individual cells of a battery becomes more complex, as does the cost, the larger the number of cells needed for a given battery application.

### Electrochemical Cells

---

- An electrochemical cell consists of two electrodes, **the anode and the cathode**, separated by an **electrolyte**, having a reversible chemical reaction at two electrodes.
- The electrolyte may be **a liquid or a solid**. Solid electrolytes are used with gaseous or liquid electrodes; they may be used with solid electrodes, but solid−solid interfaces are problematic **unless the solid electrolyte is a polymer or the solid electrodes are thin.**
- Solid electrodes separated by a liquid electrolyte are kept apart by an **electrolyte-permeable separator**.

![figure1](/images/paper_review/battery/goodenough2013/figure1.png)

- The electrolyte **conducts the ionic component of the chemical reaction between the anode and the cathode**, but it **forces the electronic component to traverse an external circuit** where it does work.

- Ionic mobility in the electrolyte is smaller than the electronic conductivity in a metal, so a cell has **large-area electrodes separated by a thin electrolyte;**

- metallic current collectors **deliver electronic current** from/to the redox centers of the electrodes to/from posts that connect to the external circuit.

- During discharge and charge, an **internal battery resistance \\(R_b\\)** to the ionic current \\(I_i = I\\) **reduces the output voltage \\(V_{dis}\\)** from the open-circuit voltage \\(V_{oc}\\) by a **polarization** \\(η = I_{dis}R_b\\) and **increases the voltage** \\(V_{ch}\\) required to reverse the chemical reaction on charge by an **overvoltage** \\(η = I_{ch}R_b\\):

  $$ V_{dis}=V_{oc}-η(q, I_{dis}) \quad V_{dis}=V_{oc}+η(q,I_{ch})  100 \times \frac{\int^{Q_{ids}}_0V_{dis}(q)dq}{\int^{Q_{ch}}_0V_{ch}(q)dq}\quad Q=\int^{\triangle t}_0 Idt=\int^Q_0dq$$

  * where q represents the state of charge, Q is the total charge per unit weight (Ah\\(kg^{-1}\\)) or per volume (Ah\\(L^{-1}\\)) \\(I=dq/dt\\) and Q(I) is the **cell capacity** for given I

- Q depends on I because the rate of transfer of ions across **electrode/electrolyte interfaces becomes diffusion-limited at high currents**. A diffusion-limited loss of the Li inserted into an electrode particle at a high rate of charge or discharge **represents a reversible loss of capacity.**

- However, **changes in electrode volume, electrode−electrolyte chemical reactions, and/or electrode decomposition** can cause an irreversible loss of capacity

- Electrode−electrolyte chemical reactions that result in the **irreversible formation of a passivating solid−electrolyte interphase (SEI) layer** on an electrode **during an initial charge of a cell** fabricated in a discharged state are distinguished from the irreversible capacity fade that may occur with cycling.

- The **cycle life** of a battery is **the number of cycles until the capacity fades to 80%** of its initial reversible value. Aside from cost and safety, are **its density (specific and volumetric) of stored energy**, its output power \\(P(q) = V(q)I_{dis}\\)  for a given discharge current, and its calendar (shelf) life.

- The available energy stored in a fully charged cell **depends on the discharge current**\\(I_{dis}=dq/dt\\). The stored energy is the product of the average voltage and the capacity.

  $$ energy = \int^{\triangle}_0 IV(t)dt = \int^Q_0 V(q)dq $$

- The volumetric energy density is of particular interest for portable batteries, especially those that power hand-held or laptop devices. The tap density is a measure of the volume fraction of active particles in a cylinder after “tapping”,

  

### The Challenge

---

- these developments continue to request more operating cycles in electronic devices with thinner and lighter LIBs of higher stored energy
- After 1st LIB of Yoshino, The energy stored in a hand-held LIB has since been successfully increased to >3.0 Ah in the 18 650 cell now available. However, the volumetric energy density has been increased mainly by cell engineering; and sophisticated cell engineering, including the ability of the synthetic chemist **to control the size and morphology of the active particles** as well as t**he architecture of the current collectors**, has almost reached its limits.
- Present LIB uses **reversible Li extraction** from an oxide host as the rechargeable cathode and into carbon or buffered spongy silicon or tin as the anode host.
- The capacity of an oxide host is limited to the **reversible solid−solution range of Li** in the cathode; and where a **passivating layer** forms on the anode during the first charge, the capacity is further reduced by **an irreversible loss of Li from the cathode in the Li+-permeable SEI layer** formed on the anode.
- The open-circuit voltage of a cell is the difference between the electrochemical potentials \\(μ_A\\) and \\(μ_C\\) of the anode and cathode: \\(V_{OC}=(μ_A-μ_C)/e\\)

![figure2](/images/paper_review/battery/goodenough2013/figure2.png)

- This voltage is limited by either the **“window”** of the electrolyte or **the top of the anion-p bands of the cathode**. The window of the electrolyte is t**he energy gap between its lowest unoccupied and highest occupied molecular orbitals (LUMO and HOMO) of a liquid electrolyte** or **the bottom of the conduction band and top of the valence band of a solid electrolyte.**

- **\\(μ_A \\) above the electrolyte LUMO** **reduces the electrolyte** unless the anode−electrolyte reaction becomes blocked by the formation of a passivating SEI layer; similarly, **a \\(μ_C\\) located below the HOMO oxidizes the electrolyte** unless the reaction is blocked by an SEI layer. 

- \\(μ_C\\) cannot be lowered below the top of the cathode anion-p bands, which may have an energy above the electrolyte HOMO.

- Since the practical HOMO of the organic liquid carbonate electrolytes used in LIBs is at 4.3 eV below \\(μ_A\\)(Li), the voltage of the simple \\(LiMO_2\\) layered oxides is also self-limited by the energy of the top of the O-2p bands, Figure 2c. As a result, the original \\(Li_{1-x}CoO_2\\)  cathode evolves oxygen or inserts protons on removing Li+ beyond x = 0.55

- In the interval 0.5<x<0.9, there is a **crossover** from polaronic to itinerant holes in states of d-orbital symmetry. This crossover is first-order, which means that a phase segregation into Li-poor and Li-rich regions occurs to give a constant V(q). However, at x > 0.55, the added itinerant holes become trapped in O-2p molecular orbitals of peroxide \\O_2^{2−}\\) ions at the particle surface, which is followed by \\(O_2\\) evolution. **Not only is μC pinned at the top of the O-2p bands, it also becomes impossible to oxidize the Co beyond a Co(IV)/Co ratio of 0.55.**

- investigation of these high-voltage cathodes has been limited because the **organic liquid carbonate electrolytes used in the LIBs decompose at a voltage V > 5 V**. Moreover, **the counter cation used to lower the top of the O-2p bands reduces the capacity** unless the active redox center can accommodate two electrons without a voltage step between them.

  

### Rechargeable Li-Ion Batteries: Their Evolution

---

- Reversible chemical reactions at solid electrodes are of two types: **displacement** and **insertion reactions**. Solid cathodes undergo insertion reactions, and solid anodes commonly undergo displacement reactions, but insertion reactions are also used

- **insertion reactions** consist of an electronically conducting host structure **into/from which the working cation, e.g., H+ or Li+, can be inserted/extracted reversibly over a finite solid−solution range**; e.g., the cathode reaction:

  $$ xH^+ xe^-+NiOOH = NiO_{1-x}(OH)_{1+x} $$

- The anode of an aqueous-electrolyte battery may be either an insertion host, e.g., a metal hydride MHx, or an elemental metal that undergoes a **displacement reaction**

  $$ Cd+2xH_2O-2xe^- = Cd(OH)_2 + 2xH^+$$

- The window of an aqueous electrolyte restricts the voltage of a battery with a stable shelf life to V ≤ 1.5V

-  The Li-ion battery was motivated by the need for a rechargeable battery with a larger energy density, i.e., a larger voltage, which requires a **nonaqueous electrolyte**. **Since H+ is only mobile in an aqueous medium**, Li+ was chosen as the working ion in a nonaqueous electrolyte.

- Li with liquid carbonate offer a lower practical HOMO at ~ 4.3cV below Li., and primary

  (unrechargeable) Li batteries with these electrolytes were known to support the anode displacement reaction, provided the the electrolyte included an ethylene carbonate (EC) additive to passivate the Li anode

  $$ Li = Li_{1-x}+xLi^+xe^-$$

- Unfortunately, although the SEI layer that forms with EC to passivate the Li anode is

  permeable to Li+, it **prevents uniform plating of Li during charge** in a rechargeable cell. Consequently, dendrites form and, on repeated charging, can grow across the separator to give an **internal short-circuit with incendiary, even explosive, consequences.**

- However, the concept of a LIB was not abandoned. Goodenough recognized that increasing the voltage of the anode would require a cathode providing a larger voltage vs Li.

- Rachid Yazami was exploring Li intercalation into graphite and noted that **reversible Li insertion into carbon avoids the problem of dendrite formation,**

- The first Li-ion battery was assembled by Yoshino with a dischared carbon anode and a discharged LiCoO2

  $$ C+xe^-+xLi^+=Li_xC(anode) \\ LiCoO_2 -xe^- -xLi^+ = Li_{1-x}CoO_2(cathode)$$

- Insertion of a guest Li+ into a layered host like TiS2, CoO2, or graphite was originally referred to as intercalation.

- Nevertheless, O2 evolution inside a cell creates safety problems unless the cell voltage is carefully managed. T**he addition of ∼10% Al3+ stabilizes the layered** oxides against electrode/electrolyte interface reactions on Li extraction, but at **the expense of capacity**

- Another safety issue is **internal short-circuits by Li penetration across the separator or by direct cathode and anode contact through a pinhole or thermal shrinkage**. To eliminate this problem, a slurry of Al2O3 and a polymeric binder is coated on the separator to block Li-dendrite penetration.

- If the Fermi energy μA of the charged anode is above the LUMO of the electrolyte, which is the case with a carbon anode at 0.2 V vs Li, a fraction of the Li from the cathode is consumed irreversibly on the initial charge in the passivating Li+-permeable SEI layer that forms on the anode surfaces.

- Moreover, the SEI layer increases the impedance of Li+ transfer across the anode/electrolyte interface, and the SEI layer changes with successive cycling to contribute to a capacity fade.

- Since carbon has a limited capacity and Li is plated onto its surface in a fast charge, the carbon anode is being replaced by **insertion into Si or alloys of Sn or Sb.**

- However, a huge lattice expansion (∼300% for Si) on Li insertion r**equires that the active anode be assembled as small particles or a sponge-like array** within a Li+ and electron conductive medium that is **sufficiently elastic to absorb the volume changes.**

- a μA ≤ 0.2 V vs Li does not allow safe fast charge that is desired for an electric vehicle battery. The buffering medium may be carbon or a conductive polymer. However, a μA(Li) − μA(alloy) <1.1 eV still requires formation of an SEI layer with the organic liquid-carbonate electrolyte

- In an attempt to improve the stability and lower the impedance of the SEI layer, replacement of **EC by other additives to the electrolyte**, e.g. fluoroethylene carbonate,22 is being explored.

- Kummer & Weber discovered fast 2D Na+ transport in Na2O·11Al2O3, which offered the possibility of a rechargeable battery with molten electrodes and a solid electrolyte, attempt to develop Na+ and Li+ electrolytes.

- The larger Na+ ion requires a relatively large 3D interstitial space. The Li+ ion, on the other hand, is small enough to be mobile at room temperature in a close-packed oxide-ion array, However, these 2D Li+ conductors have **a degree of freedom along the c axis**

- cooperative Jahn-Teller distortion : The e-orbital degeneracy on a localized Mn(III):t3e1 high-spin configuration in sufficient concentration gives rise to a cooperative orbital ordering to lower the Mn(III) site symmetry above room temperature.

- The **Ni couples** remain close enough to the top of the O-2p bands that the holes introduced into the σ-bonding 3d orbitals of Ni(II) occupy itinerant states of d-orbital symmetry

- Not only high Li+ mobility in the close-packed oxide-ion array of the spinel framework, but also **that elimination of the anode SEI layer** improves greatly the cycle life of the electrode.

- The FePO4 framework is **inexpensive and environmentally friendly**, but the cost of quality control of the LiFePO4 electrodes is presently too **expensive**.

  → exposing the surface to S or N anions has been shown to lower the surface charge-transfer impedance, thereby increasing the capacity at higher rates of charge/discharge

- two-phase reaction between LiFePO4 and FePO4 gives a flat output voltage, but **it prevents sufficient mixed valence on the iron in either phase to give an adequate** **polaronic conductivity.**

- Therefore, it has proved necessary to **either coat small particles of shorter charge-carrier path length with an electronically conductive surface layer and/or reduce the particle size to the nanoscale** where a larger range of single-phase reaction occurs.

  

  ##### More detailed challenges go on the paper....

  

### Strategies with Solid Electrolytes

---

- Kummer and Weber23 proposed in 1967 the sodium−sulfur battery, which uses cells containing a **solid electrolyte separating a molten Na anode and a cathode of molten sulfur impregnated by carbon felt**

  $$ 2Na + FeCl_2 = 2NaCl + Fe $$

- Sodium is much less expensive than lithium and widely available, but operation at higher temperatures of corrosive materials presents a challenge.

- On the other hand, solid electrolytes allow consideration of liquid and gaseous reactants.

- An all solid-state Li battery would, in principle, use an **inorganic solid or a polymer Li+ electrolyte.** An inorganic solid Li+ electrolyte has been used with thin solid electrodes in an all solid-state Li cell, but **the volume changes** in the electrodes on charge/discharge have not allowed retention of good electrode/electrolyte contact in a rechargeable storage battery

- Polymer Li+ electrolytes with a sufficiently large window and a Li+ conductivity \\(σ_{Li}\\) > 10−4 S cm−1 that retains a good contact with solid electrodes have **yet** to be demonstrated.

- a solid Li+ electrolyte membrane separating different liquid electrolytes contacting the anode and cathode would offer the possibility, if it blocked Li+ dendrites, of a **lithium anode and a liquid or gaseous cathode reactant.**

- However, a solid Li+ electrolyte separator that blocks dendrites from a Li anode would need to have a major ceramic component, but a sufficiently thin ceramic membrane would be too fragile.

- A practical solid-electrolyte separator membrane would need: 

  (a) a \\(σ_{Li}\\) > 10−4 S cm−1; 

  (b) the capability to block Li dendrites without being reduced; 

  (c) to be chemically stable in the liquid electrolytes;

  (d) to be easily fabricated into a mechanically robust, flexible thin membrane.

- These requirements would appear to **require a ceramic-polymer composite**

- Moreover, coating the surface of the electrolyte particles with a hydrophobic skin, e.g., polydopamine, allows Li+ transport while stabilizing the electrolyte particles in an aqueous electrolyte.

- The air cathode offers a high capacity, but the reversible reaction: requires **inexpensive catalysts** for the oxygen-reduction reaction (ORR) and the oxygen-evolution reaction (OER) at a high rate with a voltage difference Vch − Vdis ≤ 0.3 for storage efficiency

- $$ 4Li^+ + 4e^- + O_2 = 2Li_2O $$

- $$ 4Li^+ + 4e^- +O_2=2Li_2O$$

- With a **solid Li+ electrolyte separator of a nonaqueous electrolyte** at a Li anode and an aqueous electrolyte at the air cathode, the voltage can be increased to ∼3.5 V.
- The **lithium−sulfur battery** also provides a multielectron redox couple at the sulfur cathode and, therefore, a large increase in capacity
- A **composite polymer gel** containing a large volume fraction of an inorganic oxide and an organic liquid electrolyte immobilized in a polymer can give a flexible, thin membrane, but it needs yet to be tests for block dendrite from Li anode



#### Summary

---

- Electrical energy can be stored efficiently in a rechargeable battery, and the shift from an aqueous to the organic liquid-carbonate electrolyte in a LIB increased the energy density of a rechargeable battery sufficiently to enable battery-powered hand-held electronic devices and power tools
- Realization of this vision has led to consideration of multiple-electron redox couples and/or multivalent working ions such as Mg2+ in place of Li+, the electrolyte, catalysts, and organic multiple-electron redox centers, potential of a Li+ electrolyte membrane separating two different liquid electrolytes, a material that will require a composite of a polymer and an inorganic Li+ electrolyte, 
- 3D current collectors that enable thicker electrodes and flow-through liquid cathodes, electrochemical capacitors of higher energy density, Na-ion battery to eliminate vulnerability to sources of lithium and to lower material costs; however, the larger Na+ ion is adequately mobile only in framework oxides with a larger interstitial volume than is available in a close-packed oxide-ion array