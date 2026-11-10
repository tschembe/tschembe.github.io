---
layout: page
title: Electroplating from Deep Eutectic Solvents
description: Literature review and screening model on whether deep eutectic solvents can replace water in electroplating. Best Poster Award, PEC 2026.
img: assets/img/projects/des_tank_area.png
importance: 3
category: university
---

**27th Process Engineering Conference, TH Köln · winter term 2025/26 · Best Poster Award**
**Tools:** Python (NumPy, SciPy, pandas, Matplotlib), Word, PowerPoint

## Introduction

Most metal coatings, such as copper, nickel and chromium, are plated from water-based baths. Water limits which metals can be plated, and some baths need toxic chemicals such as hexavalent chromium.

Deep eutectic solvents (DES) are cheap mixtures of a salt and an organic compound that are liquid at room temperature. They have been promoted as a greener plating medium for about twenty years. Still, no DES plating line runs in industry. My question was simple: what stops them, in numbers a plant engineer would use?

## Methodology

1. **Review.** I read about 30 papers on DES plating and wrote an extended abstract, an A0 poster and a conference paper as sole author. I looked at the topic from a process engineering side: viscosity, conductivity, mass transfer and scale-up.
2. **Check the sources.** Later I traced every number in my paper back to the source that measured it. Four claims were wrong. The largest was a "70–80 % viscosity increase" that the source actually reports as a rise in metal concentration.
3. **Screening model.** I built a Python model of a plating cell. It covers mass transfer to the part, electrode kinetics (Butler–Volmer), ohmic losses and current uniformity (Wagner number). It uses one consistent published data set: copper in ethaline with 1 to 15 % water.
4. **Validate first.** The model reproduced five published values within 4 % before I used it to compare a DES bath with an aqueous bath. A ±20 % sensitivity study gave the range of the main result.

## Results

At the same agitation, a dry ethaline bath plates copper at about 5 % of the aqueous rate (0.48 vs. 9.3 µm/h). A production line would need about 19× the tank area (17–23× in the sensitivity study).

| Question                       | Answer                                                                                                    |
| ------------------------------ | --------------------------------------------------------------------------------------------------------- |
| What limits a DES bath?        | Mass transfer. Limiting current 0.76 vs. 14.7 mA/cm² for the aqueous bath                                 |
| Is low conductivity the issue? | Not at feasible currents. Ohmic drop is 0.095 V; slow electrode kinetics dominate the 0.45 V cell voltage |
| Energy per kg copper           | 0.40 vs. 0.17 kWh/kg. Small next to heating and solvent cost                                              |
| Coating uniformity             | 11× worse at equal current                                                                                |
| Best lever                     | Water and heat together: 2.5× tank area. Ethaline degrades from about 70 °C, which caps heating           |

<div class="row justify-content-sm-center">
  <div class="col-sm-12 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/projects/des_tank_area.png" title="Tank area relative to an aqueous bath" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Cathode (tank) area needed for the same copper throughput, relative to an aqueous chloride bath.
</div>

The review had blamed low conductivity. The model shows that mass transfer is the first limit, and that plating rate and coating uniformity trade against each other. A Stokes–Einstein check also flagged a diffusion coefficient that was 41× too low, and a viscosity fit found a typo in a published data table.

Natural next steps are to extend the model to nickel, add a heat balance and solvent cost, and test pulsed current or ultrasound, which act directly on the diffusion layer.
