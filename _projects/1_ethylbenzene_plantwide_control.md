---
layout: page
title: Plantwide Control of an Ethylbenzene Plant
description: Control design for a plant with two reactors, two columns and two recycles, built in AVEVA Process Simulation and checked with my own Python model.
img: assets/img/projects/ethylbenzene_snowball.png
importance: 1
category: university
---

**Master project (10 ECTS), TH Köln · March to June 2026**
**Tools:** AVEVA Process Simulation, Visio (DIN EN 62424), Python (NumPy, SciPy)

## Introduction

Ethylbenzene is made from benzene and ethylene. A side reaction also forms di-ethylbenzene (DEB). The DEB is separated and sent back to a second reactor, where it reacts back to ethylbenzene. So the plant has two recycle loops.

Recycles make a plant hard to control. A small change in the fresh feed can make a recycle stream grow far more than expected. This is called the snowball effect. In the worst case the columns overload, or the plant has no steady operating point at all.

My task was to design a control system for the whole plant, not just for each unit, and to show that it works. The base case is the plant published by Luyben (2011).

## Methodology

I worked in four steps:

1. **Control design.** I followed the Buckley/Luyben plantwide control procedure. The degree-of-freedom analysis gave 16 control valves and 9 steady-state specifications. I drew the P&ID one control loop at a time, in 16 sheets, following DIN EN 62424.
2. **Simulation.** I built the full plant in AVEVA Process Simulation. It has custom reaction kinetics, both distillation columns with condensers and reboilers, both recycles closed, and 16 PID controllers.
3. **Audit.** AVEVA saves its models as XML files. I parsed them with Python and read out every solved value. Then I compared each value with the published design.
4. **Independent check.** I wrote my own steady-state plant model in Python and validated it against the published stream table. I used it to test throughput and feed-impurity changes with and without a DEB recycle controller.

## Results

The Python model matches the published design within 1.3 % for the main streams and within 4 % for the DEB recycle. Only one parameter was fitted.

| Test                           | Fixed ratio control | With DEB recycle controller |
| ------------------------------ | ------------------- | --------------------------- |
| +20 % ethylene feed            | DEB recycle +91 %   | DEB recycle +29 %           |
| 5 % DEB in the fresh benzene   | no steady state     | DEB recycle +40 %           |
| Rate constants varied by ±20 % | –                   | benefit holds in every case |

For the 5 % DEB case, Luyben reports +34 %, so my result is close.

<div class="row justify-content-sm-center">
  <div class="col-sm-12 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/projects/ethylbenzene_snowball.png" title="DEB recycle vs. disturbance" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  DEB recycle flow as fresh ethylene feed (left) and DEB impurity in the benzene feed (right) increase. Grey: fixed ratio. Blue: with the DEB recycle controller. Without the controller the recycle grows steeply and, for the impurity case, the steady state disappears.
</div>

The audit also explained a problem in my AVEVA model. Its DEB recycle was 667 kmol/h instead of 282 kmol/h. The reason was 12.85 % ethylbenzene slipping over the top of the first column. The product was still on spec, so this was easy to miss.

**What I took away:**

- A product on spec does not prove that a recycle plant is right. Check the recycle compositions too.
- Mole and element balances catch errors fast. They found a typo in the source paper and a 16 % carbon imbalance in my own block diagram.
- An independent model is the best way to test a commercial simulation. It also tells you _why_ the simulation is off.

The natural next steps are to fix the column specification and the reactor heat removal in AVEVA, tune the loops, and rerun the disturbance tests against the Python results.
