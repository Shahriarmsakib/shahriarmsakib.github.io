---
layout: page
title: Real-time facial expression recognition
description: Seven-class emotion recognition on FER2013 with a custom CNN and VGG16, running live on webcam video.
img: assets/img/projects/fer_demo.jpg
importance: 1
category: projects
github: https://github.com/Shahriarmsakib/Facial-Expression-Recognition-using-CNN
---

**Code:** [GitHub](https://github.com/Shahriarmsakib/Facial-Expression-Recognition-using-CNN) · Graduate team project, UC Riverside, 2023

This project recognizes seven facial expressions (angry, disgust, fear, happy, sad, surprise and neutral) from 48×48 grayscale face crops, and runs the trained model live on webcam video.

{% include figure.liquid loading="eager" path="assets/img/projects/fer_demo.jpg" title="Real-time demo" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    Live demo: OpenCV detects the face, the model classifies the crop, and the predicted expression is shown next to the video.
</div>

#### Approach

- **Data:** FER2013, with 28,709 training and 3,589 public-test images across seven classes.
- **Models:** a compact custom CNN (four convolution layers, dropout and a 1024-unit dense layer) and VGG16 trained from scratch on single-channel input. Each model was trained with and without data augmentation (rotation, shifts, shear, zoom and flips).
- **Deployment:** Haar-cascade face detection with OpenCV; each detected face is cropped, resized and classified in real time.

VGG16 with augmentation performed best, at about 65% test accuracy, versus about 60% without augmentation.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="lazy" path="assets/img/projects/fer_vgg16_test.jpg" title="VGG16 test accuracy" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    VGG16 test accuracy during training, with and without augmentation.
</div>
