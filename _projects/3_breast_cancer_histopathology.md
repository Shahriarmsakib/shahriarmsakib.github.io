---
layout: page
title: Data-efficient and lightweight models for histopathology
description: Self-supervised contrastive learning with few labels and a 3.6M-parameter ResNet–Inception hybrid, applied to breast-cancer histopathology (Heliyon, 2024).
img: assets/img/projects/histo_card.jpg
importance: 3
category: research
related_publications: true
github: https://github.com/Shahriarmsakib/Histopathological-image-classification-for-Breast-Cancer
---

**Paper:** {% cite ashraf2024enhancing %} · [Heliyon, 2024](https://doi.org/10.1016/j.heliyon.2024.e24094) · **Code:** [GitHub](https://github.com/Shahriarmsakib/Histopathological-image-classification-for-Breast-Cancer)

Pathologists diagnose breast cancer by inspecting biopsy slides under a microscope, which is slow, error-prone and depends on expensive expert labels. We studied benign-vs-malignant classification on the BreakHis dataset (7,909 images at 40X, 100X, 200X and 400X) from two directions.

{% include figure.liquid loading="eager" path="assets/img/projects/breakhis_magnifications.jpg" title="BreakHis magnifications" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    BreakHis tissue at the four magnification levels; each level is treated as a separate classification task.
</div>

#### Two approaches

1. **Learning with few labels.** A self-supervised contrastive scheme starts from an ImageNet-pretrained ResNet50 trained on 20% of the training split. After each epoch, unlabeled images that the model labels with more than 90% confidence are pseudo-labeled and added to the training set.
2. **A smaller model.** The first three stages of ResNet50 are kept, the last two are dropped, and a naive Inception block is added. Four transfer-learning schedules range from training only the new block to training the whole network.

{% include figure.liquid loading="lazy" path="assets/img/projects/histo_architecture.jpg" title="Architecture" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    The proposed Shallow ResNet50 + Naive Inception architecture, about 3.6M parameters versus 23.8M for ResNet50. Figure from the paper (CC BY 4.0).
</div>

#### Results

The hybrid reaches **98% accuracy at 40X and 200X** and 94% at 100X and 400X. At 40X and 200X this is the best accuracy in our comparison, with less than half the parameters of the next-best model.

{% include figure.liquid loading="lazy" path="assets/img/projects/histo_config_heatmap.jpg" title="Fine-tuning configurations" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    Test accuracy for each architecture and fine-tuning schedule across magnifications.
</div>

My contribution (per the paper's CRediT statement): data curation, investigation, validation, visualization, and writing the original draft.
