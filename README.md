# Localization for Visual Servoing

Open source research code comparing FoundationPose against EKF-based stereo pose tracking for manipulator visual servoing.

A robotic arm can only servo to a target it can localize. This project asks a focused question: which perception back-end lets an eye-in-hand manipulator converge on a target object most reliably? We answer it by swapping three perception back-ends through one shared visual servoing loop on real hardware, so the observed differences are attributable to perception and not to the controller, the robot, or the logger.

The full source for all three pipelines, the EKF core, the PBVS controller, the experiment runner, and the ground-truth probing tool is in this repository under the MIT license.

**Authors**

- Akanksha Singal, Carnegie Mellon University (`asingal2@andrew.cmu.edu`)
- Sidd Vohra, Carnegie Mellon University (`svohra@andrew.cmu.edu`)

Both authors contributed equally to this work.

## Overview

We compare three perception back-ends inside a shared visual servoing loop on real hardware (xArm 7 + ZED Mini, eye-in-hand):

- **Pipeline A (IBVS).** DINOv2 + SAM2 centroid driving a calibrated image Jacobian.
- **Pipeline B (EKF + DINOv2 / SAM2 / Depth Anything V2).** Monocular centroid plus periodic metric depth, fused through an EKF.
- **Pipeline C (EKF + FoundationPose).** FoundationPose 6-DoF pose per frame, translation fused via a direct-3D update.

The same PBVS controller, EKF core, camera, robot, and per-frame CSV logger are shared across pipelines so observed differences are attributable to the perception back-end.

## Repository layout

```
scripts/     Source for the three pipelines and the evaluation tooling
data/        Reference photos, reference masks, per-trial CSV logs
results/     Post-hoc ground-truth evaluation outputs
```

## Ground truth

Translation error is measured against an independent ground truth produced by xArm TCP corner probing, not against FoundationPose's own output. The probing tool uses only the arm's joint encoders (no camera, no depth, no pose network), so it is an independent reference for all three pipelines. See `scripts/ground_truth/README.md` for the probing procedure and accuracy budget.

## License

MIT, open source. See [LICENSE](LICENSE).
