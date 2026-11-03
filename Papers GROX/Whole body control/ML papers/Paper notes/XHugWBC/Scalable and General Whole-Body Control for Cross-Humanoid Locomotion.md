---
title: "Scalable and General Whole-Body Control for Cross-Humanoid Locomotion"
aliases:
  - XHugWBC
authors:
  - "[[Yufei Xue]]"
  - "[[Yunfeng Lin]]"
  - "[[Wentao Dong]]"
  - "[[Yang Tang]]"
  - "[[Jingbo Wang]]"
  - "[[Jiangmiao Pang]]"
  - "[[Ming Zhou]]"
  - "[[Minghuan Liu]]"
  - "[[Weinan Zhang]]"
corresponding: "[[Weinan Zhang]]"
institutions:
  - "[[Shanghai Jiao Tong University]]"
  - "[[Shanghai AI Laboratory]]"
labs: []
countries:
  - China
year: 2026
date: 2026-02-05
venue: ICML 2026
venue_type: conference
arxiv_category: cs.RO
arxiv_id: "2602.05791"
url: https://arxiv.org/abs/2602.05791
pdf: https://arxiv.org/pdf/2602.05791v1
code: "No official repo found, only the project page (checked 2026-11-02)"
project_page: https://xhugwbc.github.io
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/whole-body-control
  - topic/cross-embodiment
  - topic/morphology
  - topic/domain-randomization
  - topic/sim-to-real
  - topic/teleoperation
  - method/reinforcement-learning
  - method/ppo
  - method/transformer
  - method/graph-neural-network
  - sim/isaac-gym
  - robot/unitree-g1
  - robot/unitree-h1
  - eval/real-robot
---

# XHugWBC

> [!info] Quick view
> **Authors:** [[Yufei Xue]], [[Yunfeng Lin]], [[Wentao Dong]], [[Yang Tang]], [[Jingbo Wang]], [[Jiangmiao Pang]], [[Ming Zhou]], [[Minghuan Liu]], [[Weinan Zhang]] (corresponding)
> **From:** [[Shanghai Jiao Tong University]] · [[Shanghai AI Laboratory]]
> **Published:** ICML 2026 · arXiv Feb 2026 · cs.RO
> **Keywords:** cross-humanoid control, physics-consistent inertia randomization, pseudo-inertia Cholesky, global joint space, embodiment graph, hybrid-mask transformer, zero-shot transfer
> **Links:** [arXiv](https://arxiv.org/abs/2602.05791) · [PDF](https://arxiv.org/pdf/2602.05791v1) · [Project page](https://xhugwbc.github.io)
> **One line:** one whole-body controller trained on procedurally randomized, physically valid humanoids that works zero-shot on 12 unseen simulated and 7 real humanoids

## Summary

Instead of training on a few real robot models, XHugWBC generates a big family of humanoids from a template (G1 29-DoF plus 3 head joints) with randomization that keeps every link physically valid. The trick is to parameterize each link's 4x4 pseudo-inertia matrix by its Cholesky factor and write perturbations as an affine transform of the body plus density scaling, a 10-D vector (stretch along x, y, z, shears, CoM shift, density). Any value gives a valid inertia, and each parameter has a physical meaning. Joints get randomized positions, hip axis order (permutations), limits, torque limits, PD gains scaled with mass, and waist, arm and head joints can be randomly fixed, so robots range from 12 to 32 active DoF.

All robots map to a 32-slot global joint space plus an adjacency matrix of the kinematic tree (parallel linkages collapsed). The policy is a transformer whose first layer attends globally and later layers are masked by the body graph (or a GCN), with a concurrently trained state estimator, using the HugWBC command space (velocity, height, pelvis pitch, waist, gait). On 12 unseen sim humanoids it keeps 100% survival and reaches about 85% of specialist return, fine-tuning from the generalist beats specialists by about 10%, and it transfers zero-shot to 7 real humanoids including teleoperated loco-manipulation. Honestly the real-world claim of "100% task success" is reported without many numbers, and since all robots are G1-derived variations plus similar commercial humanoids, the morphology range is narrower than it sounds, but the randomization method is really useful.

## How it works

- Link randomization: pseudo-inertia J = LLᵀ, perturbation J' = (UL)(UL)ᵀ with U upper-triangular from θ = [density, d1, d2, d3, s12, s23, s13, t1, t2, t3], ranges per body part (e.g. stretch 0.5 to 1.5)
- Joint randomization: position within ±20%, hip axis order permuted, orientation offsets summing to zero, limits and torque scaled, revolute vs fixed sampling for waist, arms, head
- Universal representation: 32 canonical joints with fixed semantic indices, zero-padding, controllability indicator I(t), adjacency matrix of the kinematic tree
- Encoders: GCN with mask I + A, or transformer with learned positional embeddings, first layer global self-attention then graph-masked layers
- Decoder: per-node linear heads using node features, global observations and estimated base velocity and height
- Critic gets privileged info including morphological parameters, per-node values averaged
- Observation: 5-step proprioception history, commands from HugWBC (velocity, height, pitch, waist yaw/pitch/roll, gait), trained in Isaac Gym

## Results that matter

- 12 unseen sim humanoids (K1, T1, N1, G1, X2, PM01, Gen1, TianGong-1/2, Atom, H1-2, Kuavo, 20 to 66 kg, 0.95 to 1.8 m): 100% survival for all
- Average tracking: generalist vx error 0.084 vs specialist 0.060 m/s, overall about 85% of specialist return
- Fine-tuning the generalist converges much faster and ends about 10% above specialists
- Naive box-shaped randomization generalizes only to robots with similar mass distribution, MetaMorph and MorAL baselines are clearly worse
- Transformer and GNN encoders beat MLP, even when the MLP gets explicit morphology inputs
- Real: zero-shot on 7 humanoids with teleoperated whole-body tasks (toy picking, door opening)

## Interesting stuff

- Physics-consistent randomization is the key idea and is general: any body variation can be sampled as stretch, shear, shift and density of the existing links
- Latent space clusters by hip axis order and waist DoF, and orders by mass, so the policy learns structure, not just identity
- No body descriptor in the actor, the policy infers the body from history and graph mask, unlike MetaMorph or MimicAll
- The unified command space is a limitation they acknowledge, motion tracking still needs per-body retargeting
- Same group as EAGLE, with the opposite approach (randomization vs distillation)

## Useful for me

- **Apply.** This is exactly the randomization I need for limb length changes: stretching a link along its axis with the pseudo-inertia parameterization keeps mass, CoM and inertia consistent automatically.
- **Apply.** Graph-masked transformer over a fixed global joint space handles variable DoF and is a good architecture candidate for a morphing humanoid.
- **Apply.** Pretrain a generalist over a wide body family, then fine-tune on the target range, gave +10% over specialists.
- **Gap.** Bodies are fixed per episode. Nothing about bodies that change during a task, nor about choosing the body.

## Key prior work

- [[HugWBC - A Unified and General Humanoid Whole-Body Controller for Versatile Locomotion|HugWBC]] · the single-robot controller and command space they extend
- [[Identification of fully physical consistent inertial parameters using optimization on manifolds|Physically consistent inertia]] · the physical consistency condition
- [[Smooth Parameterization of Rigid-Body Inertia|Smooth inertia parameterization]] · the Cholesky parameterization idea
- [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · joint-descriptor cross-embodiment locomotion
- [[MetaMorph - Learning Universal Controllers with Transformers|MetaMorph]] · baseline with explicit morphology input
- [[MorAL - Learning Morphologically Adaptive Locomotion Controller for Quadrupedal Robots on Challenging Terrains|MorAL]] · baseline for morphology-adaptive quadrupeds
- [[Body Transformer - Leveraging Robot Embodiment for Policy Learning|Body Transformer]] · hybrid masking design
- [[H-Zero - Cross-Humanoid Locomotion Pretraining Enables Few-Shot Novel Embodiment Transfer|H-Zero]] · their earlier cross-humanoid pretraining

## References

1. [[Towards Embodiment Scaling Laws in Robot Locomotion|Embodiment scaling laws]] · Ai et al. · 2025 · CoRL · [arXiv](https://arxiv.org/abs/2505.05753)
2. [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · Bohlinger et al. · 2024 · CoRL · [arXiv](https://arxiv.org/abs/2409.06366)
3. [[AnyMorph - Learning Transferable Policies By Inferring Agent Morphology|AnyMorph]] · Trabucco et al. (printed as Brandon et al.) · 2022 · ICML · [Scholar](https://scholar.google.com/scholar?q=Anymorph+Learning+transferable+policies+by+inferring+agent+morphology)
4. [[Pink - Python inverse kinematics based on Pinocchio|Pink]] · Caron et al. · 2025 · software · [GitHub](https://github.com/stephane-caron/pink)
5. [[Pinocchio - fast forward and inverse dynamics for poly-articulated systems|Pinocchio software]] · Carpentier et al. · 2015-2021 · software · [Link](https://stack-of-tasks.github.io/pinocchio)
6. [[The Pinocchio C++ library - A fast and flexible implementation of rigid body dynamics algorithms and their analytical derivatives|Pinocchio]] · Carpentier et al. · 2019 · SII · [Scholar](https://scholar.google.com/scholar?q=The+pinocchio+c%2B%2B+library+a+fast+and+flexible+implementation+of+rigid+body+dynamics+algorithms+and+their+analytical+derivatives)
7. [[RHINO - Learning Real-Time Humanoid-Human-Object Interaction from Human Demonstrations|RHINO]] · Chen et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.13134)
8. [[GMT - General Motion Tracking for Humanoid Whole-Body Control|GMT]] · Chen et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Gmt+General+motion+tracking+for+humanoid+whole-body+control)
9. [[Expressive Whole-Body Control for Humanoid Robots|ExBody]] · Cheng et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2402.16796)
10. [[Open-TeleVision - Teleoperation with Immersive Active Visual Feedback|Open-TeleVision]] · Cheng et al. · 2024 · CoRL · [arXiv](https://arxiv.org/abs/2407.01512)
11. [[XR Teleoperate|xr_teleoperate]] · Unitree Robotics developers · 2026 · software · [GitHub](https://github.com/unitreerobotics/xr_teleoperate)
12. [[Scaling Cross-Embodied Learning - One Policy for Manipulation, Navigation, Locomotion and Aviation|CrossFormer]] · Doshi et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2408.11812)
13. [[GenLoco - Generalized Locomotion Controllers for Quadrupedal Robots|GenLoco]] · Feng et al. · 2022 · arXiv · [arXiv](https://arxiv.org/abs/2209.05309)
14. [[MetaMorph - Learning Universal Controllers with Transformers|MetaMorph]] · Gupta et al. · 2022 · ICLR · [arXiv](https://arxiv.org/abs/2203.11931)
15. [[OmniH2O - Universal and Dexterous Human-to-Humanoid Whole-Body Teleoperation and Learning|OmniH2O]] · He et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2406.08858)
16. [[ASAP - Aligning Simulation and Real-World Physics for Learning Agile Humanoid Whole-Body Skills|ASAP]] · He et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.01143)
17. [[Towards Adaptable Humanoid Control via Adaptive Motion Tracking|Adaptive motion tracking]] · Huang et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2510.14454)
18. [[One Policy to Control Them All - Shared Modular Policies for Agent-Agnostic Control|SMP]] · Huang et al. · 2020 · ICML · [arXiv](https://arxiv.org/abs/2007.04976)
19. [[Learning agile and dynamic motor skills for legged robots|ANYmal agile skills]] · Hwangbo et al. · 2019 · Science Robotics · [DOI](https://doi.org/10.1126/scirobotics.aau5872)
20. [[Concurrent Training of a Control Policy and a State Estimator for Dynamic and Robust Legged Locomotion|Concurrent estimator]] · Ji et al. · 2022 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Concurrent+training+of+a+control+policy+and+a+state+estimator+for+dynamic+and+robust+legged+locomotion)
21. [[Semi-Supervised Classification with Graph Convolutional Networks|GCN]] · Kipf & Welling · 2017 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Semi-supervised+classification+with+graph+convolutional+networks)
22. [[AMO - Adaptive Motion Optimization for Hyper-Dexterous Humanoid Whole-Body Control|AMO]] · Li et al. · 2025 · RSS · [Scholar](https://scholar.google.com/scholar?q=Amo+Adaptive+motion+optimization+for+hyper-dexterous+humanoid+whole-body+control)
23. [[BeyondMimic - From Motion Tracking to Versatile Humanoid Control via Guided Diffusion|BeyondMimic]] · Liao et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2508.08241)
24. [[H-Zero - Cross-Humanoid Locomotion Pretraining Enables Few-Shot Novel Embodiment Transfer|H-Zero]] · Lin et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=H-zero+Cross-humanoid+locomotion+pretraining+enables+few-shot+novel+embodiment+transfer)
25. [[UniCon - A Unified System for Efficient Robot Learning Transfers|UniCon]] · Lin et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2601.14617)
26. [[LocoFormer - Generalist Locomotion via Long-context Adaptation|LocoFormer]] · Liu et al. · 2025 · CoRL · [arXiv](https://arxiv.org/abs/2509.23745)
27. [[MorAL - Learning Morphologically Adaptive Locomotion Controller for Quadrupedal Robots on Challenging Terrains|MorAL]] · Luo et al. · 2024 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Moral+Learning+morphologically+adaptive+locomotion+controller+for+quadrupedal+robots+on+challenging+terrains)
28. [[SONIC - Supersizing Motion Tracking for Natural Humanoid Whole-Body Control|SONIC]] · Luo et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2511.07820)
29. [[Isaac Gym - High Performance GPU-Based Physics Simulation For Robot Learning|Isaac Gym]] · Makoviychuk et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2108.10470)
30. [[Learning robust perceptive locomotion for quadrupedal robots in the wild|Perceptive locomotion in the wild]] · Miki et al. · 2022 · Science Robotics · [DOI](https://doi.org/10.1126/scirobotics.abk2822)
31. [[Agility Meets Stability - Versatile Humanoid Control with Heterogeneous Data|Agility meets stability]] · Pan et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2511.17373)
32. [[Embodiment-Aware Generalist Specialist Distillation for Unified Humanoid Whole-Body Control|EAGLE]] · Peng et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2602.02960)
33. [[Sim-to-Real Transfer of Robotic Control with Dynamics Randomization|Dynamics randomization]] · Peng et al. · 2018 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Sim-to-real+transfer+of+robotic+control+with+dynamics+randomization)
34. [[Smooth Parameterization of Rigid-Body Inertia|Smooth inertia parameterization]] · Rucker & Wensing · 2022 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Smooth+parameterization+of+rigid-body+inertia)
35. [[Body Transformer - Leveraging Robot Embodiment for Policy Learning|Body Transformer]] · Sferrazza et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2408.06316)
36. [[ManyQuadrupeds - Learning a Single Locomotion Policy for Diverse Quadruped Robots|ManyQuadrupeds]] · Shafiee et al. · 2024 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Manyquadrupeds+Learning+a+single+locomotion+policy+for+diverse+quadruped+robots)
37. [[HITTER - A Humanoid Table Tennis Robot via Hierarchical Planning and Learning|HITTER]] · Su et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2508.21043)
38. [[ULC - A Unified and Fine-Grained Controller for Humanoid Loco-Manipulation|ULC]] · Sun et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2507.06905)
39. [[Identification of fully physical consistent inertial parameters using optimization on manifolds|Physically consistent inertia]] · Traversaro et al. · 2016 · IROS · [Scholar](https://scholar.google.com/scholar?q=Identification+of+fully+physical+consistent+inertial+parameters+using+optimization+on+manifolds)
40. [[Attention Is All You Need|Transformer]] · Vaswani et al. · 2017 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Attention+Is+All+You+Need)
41. [[MoRE - Mixture of Residual Experts for Humanoid Lifelike Gaits Learning on Complex Terrains|MoRE]] · Wang et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=More+Mixture+of+residual+experts+for+humanoid+lifelike+gaits+learning+on+complex+terrains)
42. [[PhysHSI - Towards a Real-World Generalizable and Natural Humanoid-Scene Interaction System|PhysHSI]] · Wang et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2510.11072)
43. [[HDMI - Learning Interactive Humanoid Whole-Body Control from Human Videos|HDMI]] · Weng et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Hdmi+Learning+interactive+humanoid+whole-body+control+from+human+videos)
44. [[Linear Matrix Inequalities for Physically Consistent Inertial Parameter Identification - A Statistical Perspective on the Mass Distribution|LMI inertia identification]] · Wensing et al. · 2018 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Linear+matrix+inequalities+for+physically+consistent+inertial+parameter+identification+A+statistical+perspective+on+the+mass+distribution)
45. [[Learning Robust and Agile Legged Locomotion Using Adversarial Motion Priors|AMP locomotion]] · Wu et al. · 2023 · IEEE RA-L · [DOI](https://doi.org/10.1109/LRA.2023.3290509)
46. [[KungfuBot - Physics-Based Humanoid Whole-Body Control for Learning Highly-Dynamic Skills|KungfuBot]] · Xie et al. · 2025 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Kungfubot+Physics-based+humanoid+whole-body+control+for+learning+highly-dynamic+skills)
47. [[HugWBC - A Unified and General Humanoid Whole-Body Controller for Versatile Locomotion|HugWBC]] · Xue et al. · 2025 · RSS · [arXiv](https://arxiv.org/abs/2502.03206)
48. [[OmniRetarget - Interaction-Preserving Data Generation for Humanoid Whole-Body Loco-Manipulation and Scene Interaction|OmniRetarget]] · Yang et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2509.26633)
49. [[Multi-Loco - Unifying Multi-Embodiment Legged Locomotion via Reinforcement Learning Augmented Diffusion|Multi-Loco]] · Yang et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2506.11470)
50. [[UniTracker - Learning Universal Whole-Body Motion Tracker for Humanoid Robots|UniTracker]] · Yin et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2507.07356)
51. [[Multi-embodiment Legged Robot Control as a Sequence Modeling Problem|EAT]] · Yu et al. · 2022 · arXiv / ICRA 2023 · [arXiv](https://arxiv.org/abs/2212.09078)
52. [[TWIST2 - Scalable, Portable, and Holistic Humanoid Data Collection System|TWIST2]] · Ze et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Twist2+Scalable+portable+and+holistic+humanoid+data+collection+system)
53. [[Behavior Foundation Model for Humanoid Robots|Behavior foundation model]] · Zeng et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2509.13780)
54. [[Unleashing Humanoid Reaching Potential via Real-world-Ready Skill Space|Humanoid reaching skill space]] · Zhang et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2505.10918)
55. [[Track Any Motions under Any Disturbances|Track any motions]] · Zhang et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Track+any+motions+under+any+disturbances)
56. [[Embrace Collisions - Humanoid Shadowing for Deployable Contact-Agnostics Motions|Embrace Collisions]] · Zhuang & Zhao · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Embrace+collisions+Humanoid+shadowing+for+deployable+contact-agnostics+motions)

## See also

- [[MimicAll - Structured Embodiment Conditioning for Cross-Humanoid Whole-Body Motion Tracking|MimicAll]] · cross-humanoid motion tracking with physically coupled randomization, citing this paper
- [[Multi-Embodiment Locomotion at Scale with extreme Embodiment Randomization|URMAv2]] · extreme randomization across 50 legged robots
