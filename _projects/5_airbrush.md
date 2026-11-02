---
layout: page
title: "Airbrush: real-time air drawing"
description: Touchless drawing from a webcam by tracking a colored fingertip marker with classical image processing.
img: assets/img/projects/airbrush_demo.jpg
importance: 2
category: projects
github: https://github.com/Shahriarmsakib/Virtual-canvas
---

**Code:** [GitHub](https://github.com/Shahriarmsakib/Virtual-canvas) · Course project, EE 241 Advanced Digital Image Processing, UC Riverside, 2023

Airbrush turns a webcam into a drawing tablet. A colored marker on the fingertip is tracked in live video, and its path is drawn onto a virtual canvas. On-screen buttons change the stroke color or clear the canvas without touching the keyboard or mouse.

{% include figure.liquid loading="eager" path="assets/img/projects/airbrush_demo.jpg" title="Airbrush demo" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    Left: the virtual canvas. Right: the mirrored webcam view with the on-screen palette, with the tracked marker circled. Bottom: the binary HSV mask of the marker and the sliders that set its color range.
</div>

#### How it works

1. **Segment:** each frame is converted to HSV and thresholded to the marker's color range, which can be tuned live with sliders.
2. **Clean up:** erosion, morphological opening and dilation remove speckle noise from the mask.
3. **Localize:** the largest contour is taken as the marker, and its centroid (from image moments) is the pen position.
4. **Interact:** hovering over a palette button changes the color or clears the canvas; anywhere else, the pen draws.
