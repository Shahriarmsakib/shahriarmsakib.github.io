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
3. **Learning from action-free demonstrations (in progress).** An inverse dynamics model trained on a small labelled set labels the rest. With very few labelled demos the labels are worse than predicting no motion, and they become usable at around 25. At that budget, adding the pseudo-labelled demos still hurt the policy, which suggests label quality has to clear a threshold before pseudo-labelling helps.

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="lazy" path="assets/img/projects/maniskill_stage_breakdown.jpg" title="Failure analysis" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="lazy" path="assets/img/projects/maniskill_object_centric.jpg" title="Object-centric data" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: where episodes drop out of the pick-and-place sequence. Right: RGB and depth frames with simulator ground truth (cube and end-effector) projected through the camera parameters.
</div>

#### Open questions I am working toward

- Do actions expressed relative to the object and goal fix the precision bottleneck and make action-free learning more reliable?
- At small label budgets, do latent action models do better than inverse-dynamics pseudo-labelling?
- Which visual representation degrades least under camera, lighting and distractor changes?

The next step is a systematic comparison across label budgets (behavior cloning, inverse-dynamics pseudo-labelling and latent action models) with several seeds, measuring label and latent quality against ground truth. This is work in progress, and nothing here claims transfer to a real robot.
