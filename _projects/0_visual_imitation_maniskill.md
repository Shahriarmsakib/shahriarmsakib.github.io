---
layout: page
title: Label-efficient visual imitation for robot manipulation
description: "Ongoing. How much action supervision does a vision-based manipulation policy need, and when do action-free demonstrations help? Simulation study in ManiSkill 3."
img: assets/img/projects/maniskill_card.jpg
importance: 1
category: ongoing
github: https://github.com/Shahriarmsakib/robot-visual-imitation-maniskill
---

**Status:** ongoing research, simulation only · **Code:** [GitHub](https://github.com/Shahriarmsakib/robot-visual-imitation-maniskill)

#### Research question

Robot demonstrations with recorded actions are expensive to collect, while video of the same behavior without actions is cheap. I am studying how a vision-based manipulation policy can learn from **a few action-labelled demonstrations plus many action-free ones**, and which intermediate structure makes the action-free part useful:

- **inverse dynamics** (pseudo-labelling the missing actions),
- **latent actions** learned from frame-to-frame changes, or
- **object-centric geometry** (motion expressed relative to the object and goal).

Simulation is a deliberate choice here. Every frame comes with the true action, object poses and camera calibration, so I can measure what real-video studies cannot: how accurate pseudo-labels are, what latent actions encode, and why a policy fails.

{% include figure.liquid loading="eager" path="assets/img/projects/maniskill_rollouts.jpg" title="Policy rollouts" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    Rollouts of an ACT policy trained from RGB images on PickCube (pick up the cube and hold it at the green goal). Top: a successful episode. Bottom: a failure, where the cube is grasped but not brought to the goal.
</div>

#### Setup

- **Task and simulator:** ManiSkill 3, `PickCube-v1`, with end-effector delta-position control.
- **Policy:** ACT (Action Chunking with Transformers) from RGB camera images, starting from the official ManiSkill baseline.
- **Evaluation:** fixed, reproducible evaluation scenes, plus a stage-wise failure analysis that reads the simulator state at every step (reached, grasped, lifted, reached goal).

#### Work so far (preliminary, single seed)

1. **Baseline and diagnosis.** I reproduced the RGB baseline and analyzed where episodes fail. The policy grasps the cube in almost every episode but brings it within the goal tolerance in only about a quarter of them. The bottleneck is precise goal reaching, not grasping.
2. **Object-centric data pipeline.** I built a dataset of synchronized RGB-D frames, ground-truth object and gripper geometry, and camera calibration in the policy's action space. Replays match the recorded demonstrations.
3. **Learning from action-free demonstrations (in progress).** An inverse dynamics model (IDM) trained on a small labelled set labels the rest. With very few labelled demos the labels are worse than predicting no motion; from about 25 they become usable. With 100 labelled demos the IDM recovers vertical motion and the gripper well (R² 0.81, 94% correct), but horizontal motion only poorly (R² 0.2–0.3). Even so, adding 900 pseudo-labelled demos made the policy much worse at both budgets: with 100 labelled demos it reaches the cube in 9 of 100 test scenes, against 97 when trained on the 100 labelled demos alone.
4. **Checking that the comparison is sound.** The same 1,000 demos with their true actions give 95 of 100 successes under the same training schedule, so the schedule is long enough and the gap comes from the labels. Rebuilding the pseudo-labelled dataset with true actions reproduces that training data exactly, which rules out a pipeline bug. I also traced a large GPU-vs-CPU simulator gap to the physics backends (even the expert's recorded actions fail 12% of replays on the GPU simulator), so methods are compared within the CPU simulator the demos were recorded in.

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

{% include figure.liquid loading="lazy" path="assets/img/projects/maniskill_label_sources.jpg" title="Policies by action-label source" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    How far policies trained on different action labels get on the same 100 test scenes. More true labels mainly help the approach, grasp and lift; placing the cube at the goal stays rare until the label budget is large. With IDM pseudo-labels for the extra 900 demos (orange), the policy rarely even reaches the cube, although its images are identical to those of the 1,000-label policy. One seed per policy.
</div>

#### Open questions I am working toward

- Why do pseudo-labels that are reasonable on most dimensions break the policy so completely? Is it the poor horizontal labels, a small systematic bias, or errors that are correlated over time? I am testing this with a controlled label-noise sweep: corrupting the true labels in known ways and measuring the effect on the policy.
- Can partial or confidence-filtered pseudo-labels (keeping vertical motion and gripper, dropping horizontal motion) recover the benefit of the extra demos?
- Do latent action models, which never commit to explicit action labels, avoid this failure at small label budgets?
- Do actions expressed relative to the object and goal fix the goal-precision bottleneck?
- Which visual representation degrades least under camera, lighting and distractor changes?

Next: the label-noise sweep, then partial pseudo-labels and latent action models across label budgets with several seeds. All results so far are single-seed and simulation only, and nothing here claims transfer to a real robot.
