---
layout: page
title: Scale-up of a Fluidised-Bed Reactor for Vinyl Acetate
description: Scaling a vinyl acetate reactor 200× from 10 kg/h to 2 t/h, with cost estimate, dynamic model and an independent re-check.
img: assets/img/projects/vam_van_heerden.png
importance: 5
category: university
---

**Module Scale-up and Cost Estimation, TH Köln**
**Tools:** CHEMCAD, Excel, MATLAB, Python (NumPy, SciPy, pandas)

## Introduction

Vinyl acetate is a base chemical for glues, paints and paper coatings. In this process it is made from acetylene and acetic acid on a zinc acetate catalyst. The reaction releases heat, so the catalyst sits in a fluidised bed. Gas flows up through the catalyst grains until they float and mix like a boiling liquid. This keeps the temperature even.

My task was to take a miniplant reactor that makes 10 kg/h and design the production reactor for 2 t/h, which is 200× larger. The main difficulty: a bed 3.7 m wide does not behave like a tube 26 cm wide. I had to decide what to keep the same between the two scales, and show that the large reactor still works.

## Methodology

1. **Balances.** I collected property data and a published rate law, and balanced the whole sub-plant in CHEMCAD: evaporator, preheaters, blower and reactor.
2. **Design and scale-up.** I sized the miniplant bed from fluidisation physics and scaled it 200× with dimensionless numbers. Gas velocity, bed height and residence time stay constant, so the catalyst sees the same conditions.
3. **Cost estimate.** I priced the equipment, escalated the prices with the CEPCI, and built an AACE class 4 investment estimate with Hand factors.
4. **Dynamics.** I built a dynamic reactor model with temperature control in MATLAB.
5. **Independent re-check.** Later I recalculated every key number in Python. I compared the reactor with a real 50 t/d plant from the literature, modelled bubbles and gas bypass at both scales, redid the energy balance, added a production cost, and tested temperature stability.

## Results

| What               | Result                                                                           |
| ------------------ | -------------------------------------------------------------------------------- |
| Production reactor | 3.71 m diameter, 8.5 m bed, 51 t catalyst, 2 t/h at 32.5 % conversion            |
| Scale-up check     | Two-phase bubbling-bed model gives 32.0 % conversion at both scales              |
| Benchmark          | Catalyst inventory about 2.4× the industrial need, a margin for catalyst ageing  |
| Investment         | 3.0 M€ (class 4)                                                                 |
| Production cost    | about 1,940 €/t, versus a market price of about 990 €/t. Acetylene is 65 % of it |
| Stability          | The design point is unstable without control; PI control holds it within ±1.1 K  |

<div class="row justify-content-sm-center">
  <div class="col-sm-12 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/projects/vam_van_heerden.png" title="van Heerden stability check" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Heat generated vs. heat removed in the production bed. Above about 185 °C the reaction makes heat faster than the cooling can remove it, so the uncontrolled bed would drift towards about 220 °C.
</div>

The scale-up concept held. The re-check also found errors in my original spreadsheets and fixed them:

- A kJ vs. J mix-up in the Arrhenius term made the rate constant 4,700× too high.
- Adding 273.15 to temperature differences made two heat exchangers 8× too large.
- Steam demand dropped from 13.1 to 1.1 t per tonne of product after closing the energy balance.
- The investment dropped from 4.3 to 3.0 M€ with consistent cost indices and correct equipment sizes.

The instability only showed up once I included the heat capacity of the catalyst in a dynamic model. Lowering the feed temperature with a preheater bypass fixes it, which is the same scheme the industrial plant uses.

Natural next steps are to confirm the bed-to-wall heat transfer coefficient, refit one adsorption constant for the real feed ratio, and add the separation section to complete the production cost.
