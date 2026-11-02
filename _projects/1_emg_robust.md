---
layout: page
title: EMG-Robust
description: Stress-testing EMG hand-pose models under realistic sensor failures. Paper submitted.
img: assets/img/projects/emg_robust_perturbations.png
importance: 1
category: research
github: https://github.com/Shahriarmsakib/emg-robust-toolkit
---

**Status:** paper submitted · [code on GitHub](https://github.com/Shahriarmsakib/emg-robust-toolkit)

Surface electromyography (EMG) armbands can tell a robot or prosthesis what the hand is about to do before it moves, which makes them attractive for wearable robots, prosthetic control and teleoperation. EMG models, however, are usually evaluated on clean benchmark recordings. In real use, electrodes lift off, cables move, the armband slides, and the signal drifts out of sync with the user's movement.

**EMG-Robust** is a lightweight, open-source toolkit that injects these failures into an existing EMG-to-hand-pose model under a fixed, reproducible protocol, and reports how much the model's 3D hand-landmark error grows compared with clean input.

{% include figure.liquid loading="eager" path="assets/img/projects/emg_robust_perturbations.png" title="The four EMG-Robust perturbations" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    The four perturbation families, shown on a synthetic three-channel signal: additive noise, channel dropout, amplitude scaling and temporal shift.
</div>

#### What the toolkit does

- **Four perturbations at three severity levels:** additive Gaussian noise scaled to each channel, dropout of 1, 2 or 4 channels, amplitude scaling (×0.75, ×0.5, ×0.25), and temporal shifts of 20, 50 and 100 ms.
- **Reproducible sweeps:** stochastic perturbations are repeated over fixed seeds, so every number in a report can be regenerated exactly.
- **Model-agnostic adapter:** any EMG-to-pose model plugs in through a single `predict()` method; the perturbation, metric and reporting code never touch model internals.
- **Ready-made reports:** robustness tables and heatmaps are generated from the result files without needing the dataset or a GPU.

Applied to two public baselines from the [emg2pose](https://github.com/facebookresearch/emg2pose) benchmark, the toolkit shows that models ranked one way on clean data can fail in quite different ways under perturbation. That is exactly the information needed when choosing a model for a deployed human–robot interface.
