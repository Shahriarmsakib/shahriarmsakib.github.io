---
layout: page
title: "Label-efficient visual imitation: learning from imperfect action labels"
description: "Ongoing. Can a vision-based robot policy learn from a few action-labelled demonstrations plus many action-free ones, and which label errors break it? Simulation study in ManiSkill 3."
img: assets/img/projects/maniskill_card.jpg
importance: 1
category: ongoing
github: https://github.com/Shahriarmsakib/robot-visual-imitation-maniskill
---

**Status:** ongoing research, simulation only, one training seed per condition · **Code and full results:** [GitHub](https://github.com/Shahriarmsakib/robot-visual-imitation-maniskill)

#### Research question

Robot demonstrations with recorded actions are expensive to collect, while video of the same behavior without actions is cheap. I study how a vision-based manipulation policy can learn from **a few action-labelled demonstrations plus many action-free ones**. The usual route is to infer the missing actions and train on them. My main finding so far is that _how_ those inferred labels are wrong matters far more than _how much_.

Simulation is a deliberate choice here. Every frame comes with the true action, object poses and camera calibration, so I can corrupt labels in controlled ways, measure label quality exactly, and see why a policy fails.

{% include figure.liquid loading="eager" path="assets/img/projects/maniskill_rollouts.jpg" title="Policy rollouts" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    Rollouts of an ACT policy trained from RGB images on PickCube (pick up the cube and hold it at the green goal). Top: a successful episode. Bottom: a failure, where the cube is grasped but not brought to the goal.
</div>

#### Setup

- **Task and simulator:** ManiSkill 3, `PickCube-v1`, with end-effector delta-position control.
- **Policy:** ACT (Action Chunking with Transformers) from RGB camera images, starting from the official ManiSkill baseline.
- **Comparison:** 100 action-labelled plus 900 action-free demonstrations. Every condition trains on the same 1,000 demonstrations and images, differs only in the labels of the 900, and is tested on the same 100 scenes in the CPU simulator the demonstrations were recorded in.
- **Evaluation:** a stage-wise failure analysis that reads the simulator state at every step (reached, grasped, lifted, reached goal), with paired statistical tests between conditions.
- **Data:** an object-centric pipeline with synchronized RGB-D frames, ground-truth object and gripper geometry, and camera calibration in the policy's action space.

#### Findings so far

1. **Grasping is solved; goal precision is not.** The baseline policy trained on 100 demonstrations grasps the cube in 97% of episodes but brings it within the goal tolerance in only 25%.
2. **Inferred labels collapse the policy.** An inverse dynamics model (IDM) trained on the 100 labelled demonstrations labels the other 900. Its labels look acceptable step by step for vertical motion and the gripper, yet the policy trained on them succeeds in 0 of 100 scenes, against 95 with the true labels on identical images. Rebuilding the dataset with true labels reproduces the upper bound exactly, so the failure comes from the labels, not the pipeline.
3. **Structure, not size.** In a controlled sweep, random label noise as large as the IDM's errors is nearly harmless (93/100): over a 30-step action chunk it averages out. Systematic errors are not. Shrinking horizontal motion to 70% gives 25/100, shrinking it to 40% gives 1/100, and noise correlated over time gives 52/100. The IDM's labels have both flaws.
4. **Labels built to avoid those flaws.** A small CNN estimates the gripper position in each image, and actions are taken as differences of positions, which keeps their scale right. The policy then succeeds in 50–74 of 100 scenes across labeller builds and variants, against 0 with IDM labels. The same labelling from recorded positions reaches 93/100, and random noise of the image estimates' size still gives 88/100, so the remaining gap again comes from the structure of the image-based errors, not their size.
5. **Measuring my own uncertainty.** Rebuilding the same labeller moved success from 60 to 74 of 100 (GPU training is not bit-reproducible). Before differences between labellers can be trusted, every condition needs repeated runs.

{% include figure.liquid loading="lazy" path="assets/img/projects/maniskill_label_structure.jpg" title="Task success by label source" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    Task success when the labels of the 900 action-free demonstrations come from different sources, with the same images and the same 100 test scenes. Random errors barely matter; shrunk or time-correlated errors break the policy, and so do the IDM's learned labels. Labels built from image-based gripper positions recover much of the gap. One training seed per condition.
</div>

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="lazy" path="assets/img/projects/maniskill_stage_breakdown.jpg" title="Failure analysis" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="lazy" path="assets/img/projects/maniskill_object_centric.jpg" title="Object-centric data" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: where the 100-demo baseline policy's episodes drop out of the pick-and-place sequence (two checkpoints). Right: RGB and depth frames with simulator ground truth (cube and end-effector) projected through the camera parameters.
</div>

#### Where this is going

- **Repeated runs** (seeds and labeller rebuilds), so that differences between labellers can be trusted.
- **Video-only action-free demonstrations** with no robot state at all, and other tasks.
- **A label-quality measure** that predicts policy performance better than per-step accuracy does.
- **Latent action models and object-relative actions**, which may avoid explicit labels or fix the goal-precision bottleneck.

The longer-term aim is learning reliable robot skills from cheap, action-free human video. All results here are single-seed simulation results, and nothing claims transfer to a real robot.
