---
layout: page
title: Neural Network Model of a Slug Flow Crystallizer
description: Data-driven NARX models that predict crystal size in a continuous crystallizer, fast enough for model-predictive control.
img: assets/img/projects/crystallizer_openloop.png
importance: 2
category: university
---

**Project study, TH Köln (phase 1) and elective project, TU Dortmund (phase 2) · October 2025 to March 2026**
**Tools:** Python (PyTorch, scikit-learn, pandas, statsmodels), MATLAB (Deep Learning Toolbox)

## Introduction

In a slug flow crystallizer, air bubbles split a crystal suspension into small liquid "slugs". Each slug moves through a cooled tube like a small batch reactor. As it cools, the dissolved product grows onto seed crystals. The main quality target is the median crystal size at the outlet, d50.

Operators can change six inputs: two flow rates, the gas flow, two inlet temperatures and the seed loading. But a change only shows up at the outlet after the slugs have passed through the whole tube. For a 3 m crystallizer, that takes about 40 to 90 seconds. A controller for such a delayed process has to predict ahead. A detailed physical model exists, but it is too slow for real-time use.

The goal was to train a small neural network on simulation data that reproduces the crystallizer in a fraction of the time.

## Methodology

I used a NARX network (nonlinear autoregressive with exogenous inputs). It predicts the next outlet values from the recent history of inputs and outputs. The key design choice is how much history the network sees, the lag _l_. I picked it from process physics first and then checked it with data:

1. **Delay from physics.** Tube length divided by slug velocity gives the transit time: about 3 s for a 0.2 m tube and 31 to 57 s for a 3 m tube. The cooling jacket adds a slower delay of 57 to 95 s.
2. **Check with data.** An impulse-response fit showed d50 reacting 43 steps after an input change, which matches the physics.
3. **Train and compress.** I trained models for both tube lengths with lags from 0 to 80, on about 370,000 time steps. PCA reduced the long input window, for example from 610 to 66 inputs.
4. **Test the hard way.** Each model ran recursively on its own predictions over test runs it had never seen. This is how a model-predictive controller would use it. I repeated the key comparison with several random seeds.

One lesson shaped the project. The usual one-step-ahead score looked good for every model, even for models that fail after a few dozen steps. Only the recursive test shows if a model is fit for control.

The project had two phases. Phase 1 used MATLAB on a 24 m pilot crystallizer. Phase 2 moved to Python and to 0.2 m and 3 m crystallizers.

## Results

| Crystallizer  | Model                            | Recursive d50 error | R²    |
| ------------- | -------------------------------- | ------------------- | ----- |
| 3 m           | no history (l = 0)               | 44 µm               | −14   |
| 3 m           | lag from residence time (l = 60) | 1.6 µm              | 0.98  |
| 3 m           | l = 60 + PCA 99 % (final)        | **0.41 µm**         | 0.999 |
| 0.2 m         | inputs-only model, l = 10        | 0.038 µm            | 0.999 |
| 24 m, phase 1 | lag 4, re-tested recursively     | 16 µm               | 0.93  |

<div class="row justify-content-sm-center">
  <div class="col-sm-12 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/projects/crystallizer_openloop.png" title="Open-loop prediction, 3 m crystallizer" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  3 m crystallizer, withheld test run. After a short start window, the final model (blue) runs on its own predictions for 939 steps and follows the simulator (black). The model without history (grey) drifts away.
</div>

- Matching the lag to the residence time cut the error by more than 100×.
- PCA made the large model 9× smaller and 4× more accurate. It also made results more consistent across random seeds.
- The sensitivity study showed that the cooling-jacket inlet temperature is the strongest lever on crystal size: +4.3 K gives −14.7 µm on the 3 m tube. This makes it the natural input for a controller.
- In phase 1 the error stayed at about 16 µm for all models. The reason was random noise in the simulator data, not the model.

The natural next step is to use the 3 m model inside a model-predictive controller and to add uncertainty estimates to the predictions.
