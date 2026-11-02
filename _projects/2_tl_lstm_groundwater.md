---
layout: page
title: Basin-wide groundwater forecasting with transfer learning
description: One pre-trained LSTM, fine-tuned per well, for 1–3-month groundwater forecasts (Hydrological Sciences Journal, 2025).
img: assets/img/projects/tl_lstm.jpg
importance: 2
category: research
related_publications: true
github: https://github.com/Shahriarmsakib/LSTM-based-Time-Series-Forecasting-for-ground-water-Level
---

**Paper:** {% cite rahman2025basin %} · [Hydrological Sciences Journal, 2025](https://doi.org/10.1080/02626667.2025.2530120) · **Code:** [GitHub](https://github.com/Shahriarmsakib/LSTM-based-Time-Series-Forecasting-for-ground-water-Level)

Deep learning models forecast groundwater levels well, but each observation well normally gets its own model, trained and tuned from scratch. That is manageable for a handful of wells and impractical for a whole basin. We asked whether **one** model could serve every well.

{% include figure.liquid loading="eager" path="assets/img/projects/tl_lstm.jpg" title="TL-LSTM" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    TL-LSTM: an LSTM is pre-trained on the basin-average series. For each well, the early layers are frozen and only the last few layers are fine-tuned.
</div>

#### Approach

- **Study area:** seven observation wells in the Kumamoto Groundwater Basin, southern Japan, with monthly records from 1980–2017.
- **Inputs:** lagged groundwater level, rainfall, cumulative rainfall and air temperature, selected with mutual-information-based feature selection.
- **Baseline (IT-LSTM):** an independently trained and individually tuned LSTM for each well.
- **TL-LSTM:** a single model pre-trained on the basin average and transferred to each well by fine-tuning its last 2, 3 or 4 layers.

#### Results

Averaged over the seven wells (test period):

| Horizon  | IT-LSTM R² | IT-LSTM NSE | TL-LSTM (3 layers) R² | TL-LSTM (3 layers) NSE |
| -------- | ---------: | ----------: | --------------------: | ---------------------: |
| 1 month  |       0.93 |        0.92 |                  0.92 |                   0.91 |
| 2 months |       0.86 |        0.86 |                  0.85 |                   0.82 |
| 3 months |       0.78 |        0.77 |                  0.74 |                   0.72 |

The transferred models come close to the per-well baselines, and developing them for all seven wells took about one-sixth of the time needed for the per-well models. Accuracy was lowest at a well in hilly terrain, whose dynamics differ most from the basin average.

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="lazy" path="assets/img/projects/kumamoto_study_area.jpg" title="Study area" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="lazy" path="assets/img/projects/tl_lstm_training_time.jpg" title="Training time" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: the Kumamoto Groundwater Basin and its observation wells. Right: per-well training time of TL-LSTM (2, 3 or 4 fine-tuned layers) versus IT-LSTM. Figures from the paper.
</div>
