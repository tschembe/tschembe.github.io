---
layout: page
title: Polystyrene Reactor Design
description: Design of an auto-refrigerated CSTR for thermal bulk polymerisation of styrene, then an independent re-check of the whole design.
img: assets/img/projects/styrene_runaway.png
importance: 4
category: university
---

**Module Polymer Engineering and Recycling, TH Köln · April to September 2025**
**Tools:** MATLAB, Excel, Python (NumPy, SciPy, pandas), Visio

## Introduction

The task was to design the reactor section of a 45,000 t/a polystyrene plant, which is about 5.6 t of polymer per hour. Styrene polymerises on its own when heated. The reaction releases a lot of heat, and the melt becomes very viscous. If cooling fails, the reaction speeds itself up.

The chosen reactor is a stirred tank that cools itself by boiling. Part of the liquid evaporates, the vapour is condensed and flows back. The design questions were: how large must the reactor be, at what pressure must it run, how much heat must be removed, and how much time do operators have if cooling is lost?

## Methodology

1. **Balances and kinetics.** I set up the mass and energy balances of a continuous stirred-tank reactor with published kinetics for thermal polymerisation, including the gel effect.
2. **First design.** I built a dynamic model in MATLAB and did the sizing in Excel. I sized the reactor and heat exchangers and designed a two-stage cooling system (jacket plus boiling cooling with a reflux condenser). I also drew the BFD and PFD and made a pre-HAZOP.
3. **Independent re-check.** I later rebuilt the model in Python. Before using it, I validated it against published design tables (Chen 1994).
4. **Redesign and safety.** With the validated model I recalculated the volume, energy balance, boiling pressure, cooling duty and agitation. I also simulated a full loss of cooling.

## Results

The validated model matched 23 published data points within 0.22 %-points. It showed that the first design had a real error: the reactor would only make half the target output.

| Quantity                          | First design  | Checked design                |
| --------------------------------- | ------------- | ----------------------------- |
| Liquid volume (m³)                | 14            | 62                            |
| Residence time (h)                | 3.05          | 6.9                           |
| Output of the 14 m³ reactor (t/h) | 5.6 (assumed) | 2.8                           |
| Reactor pressure (bar abs.)       | 1.9           | 0.81 (boiling point at 415 K) |
| Heat to remove (GJ/t)             | 1.58          | 0.68                          |
| Time to 200 °C after cooling loss | not assessed  | 43–65 min                     |

<div class="row justify-content-sm-center">
  <div class="col-sm-12 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/projects/styrene_runaway.png" title="Loss of cooling" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Reactor temperature after a total loss of cooling, for three cases. The reactor reaches 200 °C within 43 to 65 minutes.
</div>

**What went wrong in the first design, and what I learned:**

- **Check the basis of borrowed numbers.** The first volume came from a literature table, but that table used product rate where feed rate belongs.
- **Units matter.** A mix-up between molar and mass units made one reaction rate 30× too high, with nothing obviously wrong in the output.
- **Thermodynamics sets the pressure.** Boiling cooling only works at the boiling pressure of the mixture, here 0.81 bar. It cannot be chosen freely.
- **Close the energy balance.** The cooling duties must add up to the heat the reaction releases.

Natural next steps are to confirm agitator power and wall heat transfer at the high melt viscosity with vendor data, and to turn the pre-HAZOP into a full HAZOP with relief sizing.
