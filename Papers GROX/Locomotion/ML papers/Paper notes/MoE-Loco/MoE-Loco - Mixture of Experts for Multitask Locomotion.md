---
title: "MoE-Loco: Mixture of Experts for Multitask Locomotion"
aliases:
  - MoE-Loco
  - MoELoco
authors:
  - "[[Runhan Huang]]"
  - "[[Shaoting Zhu]]"
  - "[[Yilun Du]]"
  - "[[Hang Zhao]]"
corresponding: "[[Hang Zhao]]"
institutions:
  - "[[Tsinghua University]]"
  - "[[Shanghai Qi Zhi Institute]]"
  - "[[Massachusetts Institute of Technology]]"
labs:
  - "[[Institute for Interdisciplinary Information Sciences]]"
countries:
  - China
  - USA
year: 2025
date: 2025-03-11
venue: IEEE/RSJ IROS 2025
venue_type: conference
arxiv_category: cs.RO
arxiv_id: "2503.08564"
url: https://arxiv.org/abs/2503.08564
pdf: https://arxiv.org/pdf/2503.08564
code: "No official repo yet (checked 2026-11-09)"
project_page: https://moe-loco.github.io
status: to-read
relevance: high
added: 2026-11-09
tags:
  - paper
  - topic/legged-locomotion
  - topic/multitask-learning
  - topic/sim-to-real
  - method/mixture-of-experts
  - method/reinforcement-learning
  - method/ppo
  - method/teacher-student
  - robot/unitree-go2
  - sim/isaac-gym
  - eval/real-robot
---

# MoE-Loco

> [!info] Quick view
> **Authors:** [[Runhan Huang]], [[Shaoting Zhu]], [[Yilun Du]], [[Hang Zhao]] (corresponding)
> **From:** [[Tsinghua University]] · [[Institute for Interdisciplinary Information Sciences]] · [[Shanghai Qi Zhi Institute]] · [[Massachusetts Institute of Technology]]
> **Published:** IEEE/RSJ IROS 2025 · Mar 2025 (arXiv) · cs.RO
> **Keywords:** mixture of experts, multitask RL, gradient conflict, bipedal and quadrupedal gaits, blind locomotion, skill composition, expert specialization
> **Links:** [arXiv](https://arxiv.org/abs/2503.08564) · [PDF](https://arxiv.org/pdf/2503.08564) · [DOI](https://doi.org/10.1109/IROS60139.2025.11246585) · [Project page](https://moe-loco.github.io)
> **One line:** put a mixture of experts with a shared gating network inside both actor and critic so one blind Go2 policy can cross bars, pits, baffles, stairs and slopes as a quadruped and also stand up and walk as a biped, with less gradient conflict than a plain MLP

## Summary

Training one RL locomotion policy on many terrains and gaits usually fails because the gradients from different tasks point in different directions, and with gaits as different as quadrupedal crawling and bipedal walking the training can even diverge. MoE-Loco replaces the MLP head of a standard two-stage blind locomotion pipeline (privileged oracle, then proprioception-only with an estimator) with 6 experts mixed by a softmax gating network. The gate is shared between actor and critic. A one-hot command selects quadrupedal or bipedal mode, and everything is trained in Isaac Gym with PPO and deployed zero-shot on a Unitree Go2.

The MoE policy wins on almost every terrain in sim and real, and they measure that gradient conflict actually goes down, especially between bipedal and quadrupedal tasks. The nicer part is that the experts become interpretable (one balances, one lifts a front leg), so they can compose a dribbling gait by hand and add a new expert for three-legged walking while freezing the rest. Honestly the core idea is simple and the baselines are weak (one is RMA, which basically fails everything), but the expert analysis makes it worth reading.

## How it works

- **Tasks.** 9 tasks on a Go2: quadrupedal bar crossing, pit crossing, baffle crawling, stairs and slopes, plus bipedal stand up, plane walking, slopes and stair descending. A joystick velocity command plus a one-hot gait flag
- **Observations.** Proprioception (gravity, angular velocity, joints, last action), explicit privileged state (base linear velocity and friction) and implicit privileged state (link contact forces, encoded to a latent)
- **Architecture.** Estimator + LSTM (256) feed a gating network and 6 expert MLP heads [256, 128, 128]. The action is the gate-weighted sum of expert outputs. The baseline replaces this with one MLP [640, 640, 128] of the same total size
- **Critic.** Also an MoE, using the same gating network as the actor so value estimation and action selection route the same way
- **Stage 1.** Oracle policy with all privileged info, while the estimator is trained with an L2 loss to reconstruct the privileged latent from proprioception
- **Stage 2.** Probability Annealing Selection gradually swaps the true privileged latent for the estimated one, so the policy adapts to the estimator without a separate student network
- **Training.** 4096 envs on one RTX 3090. 40k iterations of plane walking in both gaits, 80k on hard terrains, 10k of PAS. PD control with Kp = 40, Kd = 0.5 at 50 Hz
- **Composition.** Expert weights can be rescaled by hand (w[i] times the softmax), or a new expert can be added and only it and the gate trained

## Results that matter

- **Sim mixed runway.** Success 0.879 vs 0.571 for the same pipeline without MoE and 0.000 for RMA. Average travel distance 89.4 m vs 57.1 m
- **Biggest gaps** are the hard and bipedal tasks: baffle 0.924 vs 0.264, bipedal slope 0.961 vs 0.504, bipedal stairs 0.964 vs 0.453
- **Where it loses.** Quadrupedal slope 0.956 vs 0.988 for the plain MLP, and travel distance on quadrupedal stairs is also lower. So for easy tasks MoE does not help
- **Real world** (20 trials per test, values read from a bar chart so approximate): about 0.85 vs 0.4 success on the full mixed course, and MoE is equal or better on every single terrain
- **Gradient conflict.** Cosine similarity between bar crossing and bipedal slope up goes from -0.132 (MLP) to 0.278 (MoE), and negative gradient entries drop from 55% to 45.5%. Inside quadrupedal tasks the change is small, sometimes slightly worse (stair vs baffle)

## Interesting stuff

- The expert specialization is visible without any supervision. Bipedal and quadrupedal tasks form separate clusters in the t-SNE of gate outputs, and slope and pit sit close to plane walking because the gait is basically the same
- The dribbling gait is cool: take only the balancing expert and the leg-lifting expert, double the lifting weight, and the robot walks while kicking a ball, zero-shot
- Adding a new expert with the old ones frozen gives three-legged walking on flat ground and slopes. That's a form of continual learning almost for free
- **Weak point.** Baselines are thin. RMA uses the original MLP + CNN setup and gets 0 on most tasks, which says more about that setup than about MoE. No comparison with gradient surgery methods (PCGrad, CAGrad) even if they cite them
- **Weak point.** Blind locomotion only, single seed curves, and the real-world numbers are only in a bar chart

## Useful for me

- **Apply.** An MoE head with a gate shared by actor and critic is an easy change to an existing PPO pipeline, and it's a natural fit when the same policy must control very different modes, like a humanoid that changes its limb configuration
- **Apply.** The "add one new expert and freeze the rest" experiment is basically adapting to a new morphology (three legs) without forgetting. That's exactly the kind of mechanism a body that changes over time could use instead of retraining from zero
- **As prior work.** Good citation for the claim that multitask locomotion with gaits that differ a lot suffers from gradient conflict, with actual measurements (cosine similarity, negative entry ratio)
- **Gap.** Body morphology is fixed here and the gait is chosen by a one-hot command. Nobody checks if the gate could route by body configuration automatically, with no explicit label
- **Watch out.** Quadruped only, and the gains are mostly on the hard tasks. For simple walking a plain MLP of the same size did as well or better

## Key prior work

- [[Robust Robot Walker - Learning Agile Locomotion over Tiny Traps|Robust Robot Walker]] · same group, the two-stage training framework and the "w/o MoE" baseline come directly from it
- [[SARO - Space-Aware Robot System for Terrain Crossing via Vision-Language Model|SARO]] · source of Probability Annealing Selection, used for stage 2
- [[RMA - Rapid Motor Adaptation for Legged Robots|RMA]] · main external baseline, teacher-student adaptation without MoE
- [[Multi-expert learning of adaptive legged locomotion|MELA]] · earlier multi-expert locomotion, but with pretrained experts and lots of reward engineering
- [[Mixtures of Experts Unlock Parameter Scaling for Deep RL|MoE for Deep RL]] · MoE in RL from DeepMind, the scaling argument behind using experts
- [[Gradient Surgery for Multi-Task Learning|PCGrad]] · the classic gradient conflict fix, the obvious missing baseline
- [[ManyQuadrupeds - Learning a Single Locomotion Policy for Diverse Quadruped Robots|ManyQuadrupeds]] · one locomotion policy across different quadruped bodies, the multi-embodiment side of multitask
- [[Learning Agile Bipedal Motions on a Quadrupedal Robot|Bipedal Motions on a Quadruped]] · the bipedal gait on a quadruped that they fold into the multitask policy

## References

1. [[Cascaded Compositional Residual Learning for Complex Interactive Behaviors|Cascaded Compositional Residual Learning]] · Kumar et al. · 2023 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Cascaded+Compositional+Residual+Learning+for+Complex+Interactive+Behaviors)
2. [[Learning a Single Policy for Diverse Behaviors on a Quadrupedal Robot using Scalable Motion Imitation|Scalable Motion Imitation]] · Klipfel et al. · 2023 · IROS · [Scholar](https://scholar.google.com/scholar?q=Learning+a+Single+Policy+for+Diverse+Behaviors+on+a+Quadrupedal+Robot+using+Scalable+Motion+Imitation)
3. [[RMA - Rapid Motor Adaptation for Legged Robots|RMA]] · Kumar et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2107.04034)
4. [[Leveraging Symmetry in RL-based Legged Locomotion Control|Symmetry in RL Locomotion]] · Su et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2403.17320)
5. [[Concurrent Training of a Control Policy and a State Estimator for Dynamic and Robust Legged Locomotion|Concurrent Estimator Training]] · Ji et al. · 2022 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Concurrent+Training+of+a+Control+Policy+and+a+State+Estimator+for+Dynamic+and+Robust+Legged+Locomotion)
6. [[Learning Humanoid Locomotion with Perceptive Internal Model|PIM]] · Long et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2411.14386)
7. [[Robot Parkour Learning|Robot Parkour Learning]] · Zhuang et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2309.05665)
8. [[Learning Robust and Agile Legged Locomotion Using Adversarial Motion Priors|AMP Legged Locomotion]] · Wu et al. · 2023 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Learning+Robust+and+Agile+Legged+Locomotion+Using+Adversarial+Motion+Priors)
9. [[PIE - Parkour with Implicit-Explicit Learning Framework for Legged Robots|PIE]] · Luo et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2408.13740)
10. [[Extreme Parkour with Legged Robots|Extreme Parkour]] · Cheng et al. · 2024 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Extreme+Parkour+with+Legged+Robots)
11. [[Improving Generalization in Visual Reinforcement Learning via Conflict-Aware Gradient Agreement Augmentation|CG2A]] · Liu et al. · 2023 · ICCV · [Scholar](https://scholar.google.com/scholar?q=Improving+Generalization+in+Visual+Reinforcement+Learning+via+Conflict-Aware+Gradient+Agreement+Augmentation)
12. [[On the Convergence of Stochastic Multi-Objective Gradient Manipulation and Beyond|Stochastic MO Gradient Manipulation]] · Zhou et al. · 2022 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=On+the+Convergence+of+Stochastic+Multi-Objective+Gradient+Manipulation+and+Beyond)
13. [[Adaptive Mixtures of Local Experts|Adaptive Mixtures of Local Experts]] · Jacobs et al. · 1991 · Neural Computation · [Scholar](https://scholar.google.com/scholar?q=Adaptive+Mixtures+of+Local+Experts)
14. [[Mixtures of Experts Unlock Parameter Scaling for Deep RL|MoE for Deep RL]] · Obando-Ceron et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2402.08609)
15. [[Mixtures of Experts for Scaling up Neural Networks in Order Execution|MoE for Order Execution]] · Li et al. · 2024 · ICAIF · [Scholar](https://scholar.google.com/scholar?q=Mixtures+of+Experts+for+Scaling+up+Neural+Networks+in+Order+Execution)
16. [[Acquiring Diverse Skills using Curriculum Reinforcement Learning with Mixture of Experts|Di-SkilL]] · Celik et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2403.06966)
17. [[Isaac Gym - High Performance GPU-Based Physics Simulation For Robot Learning|Isaac Gym]] · Makoviychuk et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2108.10470)
18. [[Orbit - A Unified Simulation Framework for Interactive Robot Learning Environments|Orbit]] · Mittal et al. · 2023 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Orbit+A+Unified+Simulation+Framework+for+Interactive+Robot+Learning+Environments)
19. [[Learning agile and dynamic motor skills for legged robots|ANYmal agile skills]] · Hwangbo et al. · 2019 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+agile+and+dynamic+motor+skills+for+legged+robots)
20. [[Robust Robot Walker - Learning Agile Locomotion over Tiny Traps|Robust Robot Walker]] · Zhu et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2409.07409)
21. [[Learning quadrupedal locomotion over challenging terrain|Challenging Terrain]] · Lee et al. · 2020 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+quadrupedal+locomotion+over+challenging+terrain)
22. [[Rapid Locomotion via Reinforcement Learning|Rapid Locomotion]] · Margolis et al. · 2024 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Rapid+Locomotion+via+Reinforcement+Learning)
23. [[Agile But Safe - Learning Collision-Free High-Speed Legged Locomotion|ABS]] · He et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2401.17583)
24. [[Learning Agile Bipedal Motions on a Quadrupedal Robot|Bipedal Motions on a Quadruped]] · Li et al. · 2024 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Learning+Agile+Bipedal+Motions+on+a+Quadrupedal+Robot)
25. [[Learning and Adapting Agile Locomotion Skills by Transferring Experience|Transferring Experience]] · Smith et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2304.09834)
26. [[Quadruped Robot Traversing 3D Complex Environments with Limited Perception|3D Complex Environments]] · Cheng et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2404.18225)
27. [[ANYmal Parkour - Learning Agile Navigation for Quadrupedal Robots|ANYmal Parkour]] · Hoeller et al. · 2024 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=ANYmal+Parkour+Learning+Agile+Navigation+for+Quadrupedal+Robots)
28. [[Multitask Learning|Multitask Learning]] · Caruana · 1997 · Machine Learning · [Scholar](https://scholar.google.com/scholar?q=Multitask+Learning)
29. [[A Unified Architecture for Natural Language Processing - Deep Neural Networks with Multitask Learning|Unified NLP Architecture]] · Collobert & Weston · 2008 · ICML · [Scholar](https://scholar.google.com/scholar?q=A+Unified+Architecture+for+Natural+Language+Processing+Deep+Neural+Networks+with+Multitask+Learning)
30. [[Multi-Task Learning for Dense Prediction Tasks - A Survey|MTL Dense Prediction Survey]] · Vandenhende et al. · 2021 · IEEE TPAMI · [Scholar](https://scholar.google.com/scholar?q=Multi-Task+Learning+for+Dense+Prediction+Tasks+A+Survey)
31. [[Recurrent Neural Network for Text Classification with Multi-Task Learning|RNN Multi-Task Text Classification]] · Liu et al. · 2016 · arXiv · [arXiv](https://arxiv.org/abs/1605.05101)
32. [[Learning to Push by Grasping - Using Multiple Tasks for Effective Learning|Push by Grasping]] · Pinto & Gupta · 2017 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Learning+to+Push+by+Grasping+Using+Multiple+Tasks+for+Effective+Learning)
33. [[Gradient Surgery for Multi-Task Learning|PCGrad]] · Yu et al. · 2020 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Gradient+Surgery+for+Multi-Task+Learning)
34. [[Conflict-Averse Gradient Descent for Multi-task Learning|CAGrad]] · Liu et al. · 2021 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Conflict-Averse+Gradient+Descent+for+Multi-task+Learning)
35. [[MENTOR - Mixture-of-Experts Network with Task-Oriented Perturbation for Visual Reinforcement Learning|MENTOR]] · Huang et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2410.14972)
36. [[Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks|MAML]] · Finn et al. · 2017 · ICML · [Scholar](https://scholar.google.com/scholar?q=Model-Agnostic+Meta-Learning+for+Fast+Adaptation+of+Deep+Networks)
37. [[RL2 - Fast Reinforcement Learning via Slow Reinforcement Learning|RL2]] · Duan et al. · 2016 · arXiv · [arXiv](https://arxiv.org/abs/1611.02779)
38. [[Multi-Task Reinforcement Learning with Context-based Representations|CARE]] · Sodhani et al. · 2021 · ICML · [Scholar](https://scholar.google.com/scholar?q=Multi-Task+Reinforcement+Learning+with+Context-based+Representations)
39. [[Multi-expert learning of adaptive legged locomotion|MELA]] · Yang et al. · 2020 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Multi-expert+learning+of+adaptive+legged+locomotion)
40. [[GNFactor - Multi-Task Real Robot Learning with Generalizable Neural Feature Fields|GNFactor]] · Ze et al. · 2023 · CoRL · [Scholar](https://scholar.google.com/scholar?q=GNFactor+Multi-Task+Real+Robot+Learning+with+Generalizable+Neural+Feature+Fields)
41. [[ManyQuadrupeds - Learning a Single Locomotion Policy for Diverse Quadruped Robots|ManyQuadrupeds]] · Shafiee et al. · 2024 · ICRA · [Scholar](https://scholar.google.com/scholar?q=ManyQuadrupeds+Learning+a+Single+Locomotion+Policy+for+Diverse+Quadruped+Robots)
42. [[Multi-expert learning of adaptive legged locomotion|MELA (duplicate of 39)]] · Yang et al. · 2020 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Multi-expert+learning+of+adaptive+legged+locomotion)
43. [[MTAC - Hierarchical Reinforcement Learning-based Multi-gait Terrain-adaptive Quadruped Controller|MTAC]] · Shah et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2401.03337)
44. [[Hierarchical Mixtures of Experts and the EM Algorithm|Hierarchical MoE]] · Jordan & Jacobs · 1994 · Neural Computation · [Scholar](https://scholar.google.com/scholar?q=Hierarchical+Mixtures+of+Experts+and+the+EM+Algorithm)
45. [[Distributed Gaussian Processes|Distributed GPs]] · Deisenroth & Ng · 2015 · ICML · [Scholar](https://scholar.google.com/scholar?q=Distributed+Gaussian+Processes)
46. [[GShard - Scaling Giant Models with Conditional Computation and Automatic Sharding|GShard]] · Lepikhin et al. · 2020 · arXiv · [arXiv](https://arxiv.org/abs/2006.16668)
47. [[Mixtral of Experts|Mixtral]] · Jiang et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2401.04088)
48. [[DeepSeek-V3 Technical Report|DeepSeek-V3]] · Liu et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2412.19437)
49. [[M3oE - Multi-Domain Multi-Task Mixture-of Experts Recommendation Framework|M3oE]] · Zhang et al. · 2024 · SIGIR · [Scholar](https://scholar.google.com/scholar?q=M3oE+Multi-Domain+Multi-Task+Mixture-of+Experts+Recommendation+Framework)
50. [[AdaMCT - Adaptive Mixture of CNN-Transformer for Sequential Recommendation|AdaMCT]] · Jiang et al. · 2023 · CIKM · [Scholar](https://scholar.google.com/scholar?q=AdaMCT+Adaptive+Mixture+of+CNN-Transformer+for+Sequential+Recommendation)
51. [[Mixture of Cluster-conditional LoRA Experts for Vision-language Instruction Tuning|MoCLE]] · Gou et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2312.12379)
52. [[LLaVA-MoLE - Sparse Mixture of LoRA Experts for Mitigating Data Conflicts in Instruction Finetuning MLLMs|LLaVA-MoLE]] · Chen et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2401.16160)
53. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
54. [[SARO - Space-Aware Robot System for Terrain Crossing via Vision-Language Model|SARO]] · Zhu et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2407.16412)

## See also
