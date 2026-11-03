---
title: "Embodiment-Aware Generalist Specialist Distillation for Unified Humanoid Whole-Body Control"
aliases:
  - EAGLE
authors:
  - "[[Quanquan Peng]]"
  - "[[Yunfeng Lin]]"
  - "[[Yufei Xue]]"
  - "[[Jiangmiao Pang]]"
  - "[[Weinan Zhang]]"
corresponding: "[[Weinan Zhang]]"
institutions:
  - "[[Shanghai Jiao Tong University]]"
  - "[[Shanghai AI Laboratory]]"
labs: []
countries:
  - China
year: 2026
date: 2026-02-27
venue: arXiv preprint
venue_type: preprint
arxiv_category: cs.RO
arxiv_id: "2602.02960"
url: https://arxiv.org/abs/2602.02960
pdf: https://arxiv.org/pdf/2602.02960v2
code: "No official repo linked in the paper (checked 2026-11-02)"
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/whole-body-control
  - topic/cross-embodiment
  - topic/morphology
  - topic/sim-to-real
  - method/reinforcement-learning
  - method/ppo
  - method/distillation
  - method/teacher-student
  - sim/isaac-gym
  - robot/unitree-g1
  - robot/unitree-h1
  - eval/real-robot
---

# EAGLE

> [!info] Quick view
> **Authors:** [[Quanquan Peng]], [[Yunfeng Lin]], [[Yufei Xue]], [[Jiangmiao Pang]], [[Weinan Zhang]] (corresponding)
> **From:** [[Shanghai Jiao Tong University]] · [[Shanghai AI Laboratory]]
> **Published:** arXiv preprint · Feb 2026 · cs.RO
> **Keywords:** cross-embodiment, generalist-specialist distillation, DAgger, embodiment-aware observation, whole-body control, HugWBC, zero-shot sim-to-real
> **Links:** [arXiv](https://arxiv.org/abs/2602.02960) · [PDF](https://arxiv.org/pdf/2602.02960v2)
> **One line:** one whole-body controller for five different humanoids, obtained by repeatedly forking per-robot specialists from a generalist and distilling them back

## Summary

Most humanoid WBC policies are trained for one robot, and cross-embodiment locomotion works usually only take velocity commands. EAGLE wants one policy for H1, G1, T1, Fourier N1 and Adam that also does squatting and leaning. It uses the HugWBC command interface (vx, vy, yaw rate, base height, body pitch), maps all robots into a shared 32-slot action and observation layout with zero padding, and adds an embodiment-aware signal: mass, CoM and inertia of torso and both feet are given to the critic and the actor must predict them. A PPO generalist trained on all robots saturates at a mediocre level, so they loop: copy the generalist into one specialist per robot, fine-tune, then distill back with a DAgger action loss plus a latent alignment loss while keeping the PPO loss.

On tracking errors the iterative version beats a joint PPO baseline, Kickstarting (which blows up on T1 with 0.761 m/s vx error) and a reimplemented COMPASS on most robot-metric pairs. On H1 the generalist matches a single-robot PPO and the forked specialist even beats it. Zero-shot deployment works on four real robots. Honestly the gains vs joint PPO are moderate on several robots, there is no test on unseen robots, and the embodiment descriptor is very coarse (only three bodies), as they admit.

## How it works

- Command c = [vx, vy, ω, h, p], task part says where to go and behavior part sets height and pitch
- Actor sees 5 frames of proprioception plus a gait clock, critic gets privileged terrain and contact info plus the embodiment observation (mass, CoM, inertia of 3 bodies)
- Unified action space of 32 DoF (legs, waist, head, arms) with per-robot permutation matrices, robot-specific PD gains and swing heights kept
- Rewards mostly from HugWBC, shared weights across robots, only targets like nominal base height change per robot
- Distillation loss L = L_PPO + 0.02·L_action + 1·L_latent, actions relabeled by the matching specialist on generalist rollouts
- Isaac Gym, 4096 envs, robots sampled with equal probability, deployed with their UniCon control interface

## Results that matter

- H1: vx error 0.051 for EAGLE w/ ID vs 0.108 joint PPO, 0.323 Kickstarting, 0.725 COMPASS
- Removing the embodiment-aware observation hurts H1 a lot (vx error 0.108 to 0.290), small effect on other robots
- Single-round EAGLE already beats Kickstarting on 80% of robot-metric pairs
- H1 single-robot PPO vx error 0.067 vs EAGLE generalist 0.061 vs EAGLE specialist 0.048
- Some behavior errors (height, pitch) get slightly worse after iterating
- Zero-shot walking, leaning and squatting on H1, G1, N1 and T1

## Interesting stuff

- The specialist-generalist loop is a clean way to beat the "average of many robots" problem of joint training
- Latent alignment works because generalist and specialist share the architecture, simple but effective
- t-SNE shows that with the embodiment signal each robot gets its own latent cluster, so the policy does identify which body it controls
- Only five fixed robots, no morphology randomization and no unseen robot test, so it is multi-robot more than general
- Real robot results are qualitative, no tracking numbers on hardware

## Useful for me

- **As prior work.** Directly related: one policy that knows which body it is controlling. A humanoid that changes morphology needs exactly this kind of embodiment awareness.
- **Apply.** Asking the actor to predict the body's mass, CoM and inertia (with the true values in the critic) is cheap and could be used to estimate the current configuration of a morphing robot online.
- **Apply.** Specialist fine-tuning and distilling back is a good recipe if a single policy over a continuous range of body configurations saturates, e.g. fork specialists for a few body sizes and distill back.
- **Gap.** Bodies are discrete and fixed during an episode. Continuous morphology change during the task is not addressed.

## Key prior work

- [[HugWBC - A Unified and General Humanoid Whole-Body Controller for Versatile Locomotion|HugWBC]] · the command interface and reward design they build on
- [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · cross-embodiment locomotion with URDF randomization
- [[Towards Embodiment Scaling Laws in Robot Locomotion|Embodiment scaling laws]] · scaling the number of training bodies
- [[COMPASS - Cross-embodiment Mobility Policy via Residual RL and Skill Synthesis|COMPASS]] · the main cross-embodiment baseline
- [[Kickstarting Deep Reinforcement Learning|Kickstarting]] · KL distillation baseline that is unstable here
- [[A Reduction of Imitation Learning and Structured Prediction to No-Regret Online Learning|DAgger]] · the relabeling idea for distillation
- [[Multi-Loco - Unifying Multi-Embodiment Legged Locomotion via Reinforcement Learning Augmented Diffusion|Multi-Loco]] · diffusion prior route to multi-embodiment

## References

1. [[Open X-Embodiment - Robotic Learning Datasets and RT-X Models|Open X-Embodiment]] · O'Neill et al. · 2024 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Open+X-Embodiment+Robotic+Learning+Datasets+and+RT-X+Models)
2. [[π0 - A Vision-Language-Action Flow Model for General Robot Control|π0]] · Black et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2410.24164)
3. [[PoliFormer - Scaling On-Policy RL with Transformers Results in Masterful Navigators|PoliFormer]] · Zeng et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2406.20083)
4. [[COMPASS - Cross-embodiment Mobility Policy via Residual RL and Skill Synthesis|COMPASS]] · Liu et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.16372)
5. [[Open-TeleVision - Teleoperation with Immersive Active Visual Feedback|Open-TeleVision]] · Cheng et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2407.01512)
6. [[Human-Agent Joint Learning for Efficient Robot Manipulation Skill Acquisition|Human-agent joint learning]] · Luo et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2407.00299)
7. [[AirExo - Low-Cost Exoskeletons for Learning Whole-Arm Manipulation in the Wild|AirExo]] · Fang et al. · 2024 · ICRA · [Scholar](https://scholar.google.com/scholar?q=AirExo+Low-Cost+Exoskeletons+for+Learning+Whole-Arm+Manipulation+in+the+Wild)
8. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
9. [[Soft Actor-Critic - Off-Policy Maximum Entropy Deep Reinforcement Learning with a Stochastic Actor|SAC]] · Haarnoja et al. · 2018 · ICML · [Scholar](https://scholar.google.com/scholar?q=Soft+Actor-Critic+Off-Policy+Maximum+Entropy+Deep+Reinforcement+Learning+with+a+Stochastic+Actor)
10. [[ExBody2 - Advanced Expressive Humanoid Whole-Body Control|ExBody2]] · Ji et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2412.13196)
11. [[HugWBC - A Unified and General Humanoid Whole-Body Controller for Versatile Locomotion|HugWBC]] · Xue et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.03206)
12. [[OmniH2O - Universal and Dexterous Human-to-Humanoid Whole-Body Teleoperation and Learning|OmniH2O]] · He et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2406.08858)
13. [[HOVER - Versatile Neural Whole-Body Controller for Humanoid Robots|HOVER]] · He et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2410.21229)
14. [[Multi-Loco - Unifying Multi-Embodiment Legged Locomotion via Reinforcement Learning Augmented Diffusion|Multi-Loco]] · Yang et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2506.11470)
15. [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · Bohlinger et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2409.06366)
16. [[Towards Embodiment Scaling Laws in Robot Locomotion|Embodiment scaling laws]] · Ai et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2505.05753)
17. [[LocoFormer - Generalist Locomotion via Long-context Adaptation|LocoFormer]] · Liu et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2509.23745)
18. [[A Reduction of Imitation Learning and Structured Prediction to No-Regret Online Learning|DAgger]] · Ross et al. · 2011 · AISTATS · [Scholar](https://scholar.google.com/scholar?q=A+reduction+of+imitation+learning+and+structured+prediction+to+no-regret+online+learning)
19. [[Preparing for the Unknown - Learning a Universal Policy with Online System Identification|UP-OSI]] · Yu et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1702.02453)
20. [[PEAC - Unsupervised Pre-training for Cross-Embodiment Reinforcement Learning|PEAC]] · Ying et al. · 2024 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=PEAC+Unsupervised+pre-training+for+cross-embodiment+reinforcement+learning)
21. [[Pushing the Limits of Cross-Embodiment Learning for Manipulation and Navigation|Cross-embodiment limits]] · Yang et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2402.19432)
22. [[XSkill - Cross Embodiment Skill Discovery|XSkill]] · Xu et al. · 2023 · CoRL · [Scholar](https://scholar.google.com/scholar?q=XSkill+Cross+embodiment+skill+discovery)
23. [[Scaling Cross-Embodied Learning - One Policy for Manipulation, Navigation, Locomotion and Aviation|CrossFormer]] · Doshi et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2408.11812)
24. [[UniGrasp - Learning a Unified Model to Grasp with Multifingered Robotic Hands|UniGrasp]] · Shao et al. · 2020 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=UniGrasp+Learning+a+unified+model+to+grasp+with+multifingered+robotic+hands)
25. [[Domain Randomization for Transferring Deep Neural Networks from Simulation to the Real World|Domain randomization]] · Tobin et al. · 2017 · IROS · [Scholar](https://scholar.google.com/scholar?q=Domain+randomization+for+transferring+deep+neural+networks+from+simulation+to+the+real+world)
26. [[RMA - Rapid Motor Adaptation for Legged Robots|RMA]] · Kumar et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2107.04034)
27. [[Extreme Parkour with Legged Robots|Extreme Parkour]] · Cheng et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2309.14341)
28. [[Learning agile and dynamic motor skills for legged robots|ANYmal agile skills]] · Hwangbo et al. · 2019 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+agile+and+dynamic+motor+skills+for+legged+robots)
29. [[HumanPlus - Humanoid Shadowing and Imitation from Humans|HumanPlus]] · Fu et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2406.10454)
30. [[TWIST - Teleoperated Whole-Body Imitation System|TWIST]] · Ze et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2505.02833)
31. [[Learning Humanoid Standing-up Control across Diverse Postures|HoST]] · Huang et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.08378)
32. [[Distilling the Knowledge in a Neural Network|Knowledge distillation]] · Hinton et al. · 2015 · arXiv · [arXiv](https://arxiv.org/abs/1503.02531)
33. [[Knowledge Distillation - A Survey|KD survey]] · Gou et al. · 2021 · IJCV · [Scholar](https://scholar.google.com/scholar?q=Knowledge+distillation+A+survey+Gou)
34. [[Relational Knowledge Distillation|Relational KD]] · Park et al. · 2019 · CVPR · [Scholar](https://scholar.google.com/scholar?q=Relational+knowledge+distillation)
35. [[Distral - Robust Multitask Reinforcement Learning|Distral]] · Teh et al. · 2017 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Distral+Robust+multitask+reinforcement+learning)
36. [[Kickstarting Deep Reinforcement Learning|Kickstarting]] · Schmitt et al. · 2018 · arXiv · [arXiv](https://arxiv.org/abs/1803.03835)
37. [[Learning robust perceptive locomotion for quadrupedal robots in the wild|Perceptive locomotion in the wild]] · Miki et al. · 2022 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+robust+perceptive+locomotion+for+quadrupedal+robots+in+the+wild)
38. [[Learning quadrupedal locomotion on deformable terrain|Deformable terrain locomotion]] · Choi et al. · 2023 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+quadrupedal+locomotion+on+deformable+terrain)
39. [[Isaac Gym - High Performance GPU-Based Physics Simulation For Robot Learning|Isaac Gym]] · Makoviychuk et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2108.10470)
40. [[UniCon - A Unified System for Efficient Robot Learning Transfers|UniCon]] · Lin et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2601.14617)

## See also

- [[Scalable and General Whole-Body Control for Cross-Humanoid Locomotion|XHugWBC]] · same group, randomization route instead of distillation
- [[MimicAll - Structured Embodiment Conditioning for Cross-Humanoid Whole-Body Motion Tracking|MimicAll]] · cross-humanoid motion tracking with an explicit body descriptor
