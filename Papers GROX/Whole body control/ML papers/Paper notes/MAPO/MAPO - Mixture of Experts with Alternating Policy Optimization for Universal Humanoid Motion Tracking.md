---
title: "MAPO: Mixture of Experts with Alternating Policy Optimization for Universal Humanoid Motion Tracking"
aliases:
  - MAPO
authors:
  - "[[Seonghyeon Lim]]"
  - "[[Hyeonwoo Lee]]"
  - "[[Mincheol Kim]]"
  - "[[Kimin Lee]]"
  - "[[Hyun Myung]]"
corresponding: "[[Hyun Myung]]"
institutions:
  - "[[Korea Advanced Institute of Science and Technology]]"
  - "[[Config]]"
labs: []
countries:
  - South Korea
year: 2026
date: 2026-11-02
venue: arXiv preprint
venue_type: preprint
arxiv_category: cs.RO
arxiv_id: "2611.02613"
url: https://arxiv.org/abs/2611.02613
pdf: https://arxiv.org/pdf/2611.02613v1
code: "No official repo linked in the paper (checked 2026-11-03)"
project_page: https://mapo-corl2026.github.io/MAPO/
status: to-read
relevance: medium
added: 2026-11-03
tags:
  - paper
  - topic/humanoids
  - topic/whole-body-control
  - topic/motion-tracking
  - topic/sim-to-real
  - method/reinforcement-learning
  - method/ppo
  - method/mixture-of-experts
  - method/teacher-student
  - method/distillation
  - dataset/amass
  - dataset/lafan1
  - sim/isaac-lab
  - robot/unitree-g1
  - eval/real-robot
---

# MAPO

> [!info] Quick view
> **Authors:** [[Seonghyeon Lim]], [[Hyeonwoo Lee]], [[Mincheol Kim]], [[Kimin Lee]] (co-corresponding), [[Hyun Myung]] (co-corresponding)
> **From:** [[Korea Advanced Institute of Science and Technology]] · [[Config]]
> **Published:** arXiv preprint · Nov 2026 · cs.RO
> **Keywords:** mixture of experts, PPO, trust region, divergence misestimation, router, universal motion tracking, teacher-student, Unitree G1
> **Links:** [arXiv](https://arxiv.org/abs/2611.02613) · [PDF](https://arxiv.org/pdf/2611.02613v1) · [Project](https://mapo-corl2026.github.io/MAPO/)
> **One line:** PPO measures the KL of the whole MoE mixture, which mixes up router and expert changes, so MAPO clips the router and the experts separately and gets a better universal motion tracker.

## Summary

Universal motion trackers like GMT or KungfuBot2 use a mixture of experts policy, where a router gives softmax weights over several expert MLPs and the action mean is the weighted sum. This paper points out a problem nobody looked at: PPO clips on the ratio of the full mixed policy, and the KL of that mixture is KL_router + KL_expert + a cross term dw·dµ/σ². When the router and expert changes point the same way the KL is overestimated and PPO blocks good updates, when they point opposite it is underestimated and aggressive updates pass. With more experts it is mostly overestimation, with few experts mostly underestimation, so you cant fix it by tuning the number of experts or the clip range.

MAPO is a very small change. From the same rollout batch they compute two PPO surrogates, one where only the router is new (experts frozen at rollout values) and one where only the experts are new (mixing weights frozen), and each one is clipped on its own ratio. Trained on AMASS + LAFAN1 for a G1 in Isaac Lab, it goes from 83.4% to 86.6% success on train motions and tracks 182 motions that PPO with MoE fails, and the student deploys on a real G1. Honestly the idea is clean and the ablations are careful, but the gain on the unseen OMOMO test set is small and the real robot part is only qualitative.

## How it works

- Dense MoE teacher with 6 experts and a softmax router, Gaussian policy with the mean m = Σ w_e µ_e and a shared σ
- Expert ratio uses new experts with old mixing weights, router ratio uses new mixing weights with old experts, each one goes into the normal clipped PPO loss
- Both losses come from the same batch in one update step, so "alternating" means separate gradient computations, not a different schedule
- Teacher observation has full proprioception plus per-link positions, velocities and the reference motion (future link positions, heading and per-link errors to the reference), reward is tracking plus regularization like BeyondMimic and SONIC
- Student is a monolithic MLP distilled with DAgger, without privileged inputs, with the observations and domain randomization of TWIST
- Motions retargeted to a 29 DoF G1 with GMR, 4096 envs on one A6000, ε = 0.2 and adaptive learning rate from legged_gym
- Supplementary shows the expert gradients keep the same stationary points, and the router bias depends only on how differently the experts move

## Results that matter

- Train set (AMASS + LAFAN1): success 86.6% vs 83.4% PPO with MoE vs 81.8% monolithic PPO, local position error 78.1 mm vs 88.0 mm vs 96.5 mm
- Test set (OMOMO): 85.5% vs 82.8% PPO with MoE, but monolithic PPO already reaches 84.4% with almost the same global position error (227.2 vs 225.8 mm) and beats PPO with MoE, so on unseen motions the MoE story is weak
- HOVER reimplemented gets 83.4% on test and the released TWIST2 checkpoint only 57.9%, not a fair comparison because TWIST2 was not retrained on their data
- With 4, 8 and 16 experts MAPO always beats PPO with MoE in success (best 87.9% with N = 8), but PPO with MoE keeps lower root roll and pitch errors in most settings
- A smaller clip range for MAPO (0.5ε) still beats PPO with MoE at bigger ε, so the gain is not just a bigger update budget. A KL penalty, early stopping and separate learning rates for the router don't save PPO with MoE either
- 182 motions that MAPO tracks in all 5 trials and PPO with MoE fails in all 5, against only 1 the other way

## Interesting stuff

- The KL decomposition is simple and kinda obvious once you see it, which is why it's a nice catch. Any PPO policy with a gated mixture has this issue, not only motion tracking
- Sharper routing makes the cross term bigger, so PPO punishes the router exactly when it tries to specialize. That explains why PPO-trained MoE often ends with almost uniform weights
- MAPO with a fixed router is about as good as PPO with MoE, so the misestimation costs about as much as not training the router at all
- Weak points: the analysis assumes shared σ and dense routing, it's evaluated on teacher policies only, they use one training seed as far as I can tell (the "5 trials" are 5 evaluation rollouts), and there are no numbers for the real robot

## Useful for me

- **Apply.** If we use a mixture of experts for a humanoid whose body changes, for example one expert per limb configuration with a router that sees the morphology, MAPO is basically a free fix: two surrogate losses instead of one, same rollouts.
- **Apply.** The teacher-student setup is the standard one (privileged MoE teacher, monolithic DAgger student). It's a clean reference for how to distill an expressive teacher into a small deployable policy.
- **Watch out.** The paper is about PPO training of the teacher. It says nothing about robustness or transfer to a different body, and the test-set gains are small, so dont expect MoE alone to help with generalization.
- **Gap.** All motions are retargeted to a single fixed G1. Nobody here asks how routing should behave when the body itself changes during the task, which is exactly where a router conditioned on morphology could be useful.

## Key prior work

- [[GMT - General Motion Tracking for Humanoid Whole-Body Control|GMT]] · dense MoE motion tracker, the closest architecture and the one MAPO basically trains better
- [[KungfuBot2 - Learning Versatile Motion Skills for Humanoid Whole-Body Control|KungfuBot2]] · orthogonal MoE for many skills, sparse routing
- [[Proximal Policy Optimization Algorithms|PPO]] · the algorithm whose clipping goes wrong with MoE
- [[Trust Region Policy Optimization|TRPO]] · where the KL trust region idea comes from
- [[Adaptive Mixtures of Local Experts|Adaptive mixtures of experts]] · the original MoE paper, worth knowing the gating math
- [[HOVER - Versatile Neural Whole-Body Controller for Humanoid Robots|HOVER]] · teacher-student universal controller used as a baseline
- [[TWIST - Teleoperated Whole-Body Imitation System|TWIST]] · student observations and domain randomization come from here
- [[Retargeting Matters - General Motion Retargeting for Humanoid Motion Tracking|GMR]] · the retargeting they use for AMASS and LAFAN1

## References

1. [[Real-world humanoid locomotion with reinforcement learning|Real-world humanoid RL]] · Radosavovic et al. · 2024 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Real-world+humanoid+locomotion+with+reinforcement+learning)
2. [[Learning Humanoid Locomotion with Perceptive Internal Model|PIM]] · Long et al. · 2025 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Learning+Humanoid+Locomotion+with+Perceptive+Internal+Model)
3. [[Advancing Humanoid Locomotion - Mastering Challenging Terrains with Denoising World Model Learning|DWL]] · Gu et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2408.14472)
4. [[Learning Humanoid Locomotion over Challenging Terrain|Humanoid challenging terrain]] · Radosavovic et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2410.03654)
5. [[Humanoid Parkour Learning|Humanoid Parkour]] · Zhuang et al. · 2025 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Humanoid+Parkour+Learning)
6. [[Hiking in the Wild - A Scalable Perceptive Parkour Framework for Humanoids|Hiking in the Wild]] · Zhu et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2601.07718)
7. [[Learning Humanoid Standing-up Control across Diverse Postures|HoST]] · Huang et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.08378)
8. [[Learning Getting-Up Policies for Real-World Humanoid Robots|HumanUP]] · He et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.12152)
9. [[AMASS - Archive of Motion Capture as Surface Shapes|AMASS]] · Mahmood et al. · 2019 · ICCV · [Scholar](https://scholar.google.com/scholar?q=AMASS+Archive+of+Motion+Capture+as+Surface+Shapes)
10. [[Robust Motion In-betweening|LAFAN1]] · Harvey et al. · 2020 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=Robust+Motion+In-betweening)
11. [[Object Motion Guided Human Motion Synthesis|OMOMO]] · Li et al. · 2023 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=Object+Motion+Guided+Human+Motion+Synthesis)
12. [[DeepMimic - Example-Guided Deep Reinforcement Learning of Physics-Based Character Skills|DeepMimic]] · Peng et al. · 2018 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=DeepMimic+Example-Guided+Deep+Reinforcement+Learning+of+Physics-Based+Character+Skills)
13. [[Expressive Whole-Body Control for Humanoid Robots|ExBody]] · Cheng et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2402.16796)
14. [[ExBody2 - Advanced Expressive Humanoid Whole-Body Control|ExBody2]] · Ji et al. · 2025 · RSS Workshop · [Scholar](https://scholar.google.com/scholar?q=ExBody2+Advanced+Expressive+Humanoid+Whole-Body+Control)
15. [[BeyondMimic - From Motion Tracking to Versatile Humanoid Control via Guided Diffusion|BeyondMimic]] · Liao et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2508.08241)
16. [[TWIST - Teleoperated Whole-Body Imitation System|TWIST]] · Ze et al. · 2025 · CoRL · [Scholar](https://scholar.google.com/scholar?q=TWIST+Teleoperated+Whole-Body+Imitation+System)
17. [[SONIC - Supersizing Motion Tracking for Natural Humanoid Whole-Body Control|SONIC]] · Luo et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2511.07820)
18. [[Learning Human-to-Humanoid Real-Time Whole-Body Teleoperation|H2O]] · He et al. · 2024 · IROS · [Scholar](https://scholar.google.com/scholar?q=Learning+Human-to-Humanoid+Real-Time+Whole-Body+Teleoperation)
19. [[GMT - General Motion Tracking for Humanoid Whole-Body Control|GMT]] · Chen et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2506.14770)
20. [[General Humanoid Whole-Body Control via Pretraining and Fast Adaptation|FAST]] · Wang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2602.11929)
21. [[Gradient Surgery for Multi-Task Learning|PCGrad]] · Yu et al. · 2020 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Gradient+Surgery+for+Multi-Task+Learning)
22. [[Conflict-Averse Gradient Descent for Multi-task Learning|CAGrad]] · Liu et al. · 2021 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Conflict-Averse+Gradient+Descent+for+Multi-task+Learning)
23. [[Ray Interference - a Source of Plateaus in Deep Reinforcement Learning|Ray interference]] · Schaul et al. · 2019 · arXiv · [arXiv](https://arxiv.org/abs/1904.11455)
24. [[Adaptive Mixtures of Local Experts|Adaptive mixtures of experts]] · Jacobs et al. · 1991 · Neural Computation · [Scholar](https://scholar.google.com/scholar?q=Adaptive+Mixtures+of+Local+Experts)
25. [[Mixture of Experts in a Mixture of RL Settings|MoE in RL settings]] · Willi et al. · 2024 · RLJ · [Scholar](https://scholar.google.com/scholar?q=Mixture+of+Experts+in+a+Mixture+of+RL+Settings)
26. [[Multi-Task Reinforcement Learning with Mixture of Orthogonal Experts|MOORE]] · Hendawy et al. · 2024 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Multi-Task+Reinforcement+Learning+with+Mixture+of+Orthogonal+Experts)
27. [[Multi-Task Reinforcement Learning With Attention-Based Mixture of Experts|Attention MoE MTRL]] · Cheng et al. · 2023 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Multi-Task+Reinforcement+Learning+With+Attention-Based+Mixture+of+Experts)
28. [[Multi-Task Reinforcement Learning with Soft Modularization|Soft modularization]] · Yang et al. · 2020 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Multi-Task+Reinforcement+Learning+with+Soft+Modularization)
29. [[MoE-Loco - Mixture of Experts for Multitask Locomotion|MoE-Loco]] · Huang et al. · 2025 · IROS · [Scholar](https://scholar.google.com/scholar?q=MoE-Loco+Mixture+of+Experts+for+Multitask+Locomotion)
30. [[KungfuBot2 - Learning Versatile Motion Skills for Humanoid Whole-Body Control|KungfuBot2]] · Han et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2509.16638)
31. [[CLONE - Closed-Loop Whole-Body Humanoid Teleoperation for Long-Horizon Tasks|CLONE]] · Li et al. · 2025 · CoRL · [Scholar](https://scholar.google.com/scholar?q=CLONE+Closed-Loop+Whole-Body+Humanoid+Teleoperation+for+Long-Horizon+Tasks)
32. [[Trust Region Policy Optimization|TRPO]] · Schulman et al. · 2015 · ICML · [Scholar](https://scholar.google.com/scholar?q=Trust+Region+Policy+Optimization)
33. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
34. [[Outrageously Large Neural Networks - The Sparsely-Gated Mixture-of-Experts Layer|Sparsely-gated MoE]] · Shazeer et al. · 2017 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Outrageously+Large+Neural+Networks+The+Sparsely-Gated+Mixture-of-Experts+Layer)
35. [[GShard - Scaling Giant Models with Conditional Computation and Automatic Sharding|GShard]] · Lepikhin et al. · 2021 · ICLR · [Scholar](https://scholar.google.com/scholar?q=GShard+Scaling+Giant+Models+with+Conditional+Computation+and+Automatic+Sharding)
36. [[Switch Transformers - Scaling to Trillion Parameter Models with Simple and Efficient Sparsity|Switch Transformers]] · Fedus et al. · 2022 · JMLR · [Scholar](https://scholar.google.com/scholar?q=Switch+Transformers+Scaling+to+Trillion+Parameter+Models+with+Simple+and+Efficient+Sparsity)
37. [[Mixture-of-Experts with Expert Choice Routing|Expert choice routing]] · Zhou et al. · 2022 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Mixture-of-Experts+with+Expert+Choice+Routing)
38. [[Scaling Vision with Sparse Mixture of Experts|V-MoE]] · Riquelme et al. · 2021 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Scaling+Vision+with+Sparse+Mixture+of+Experts)
39. [[Probabilistic Mixture-of-Experts for Efficient Deep Reinforcement Learning|PMOE]] · Ren et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2104.09122)
40. [[Continuous Action Reinforcement Learning from a Mixture of Interpretable Experts|Interpretable experts]] · Akrour et al. · 2022 · IEEE TPAMI · [Scholar](https://scholar.google.com/scholar?q=Continuous+Action+Reinforcement+Learning+from+a+Mixture+of+Interpretable+Experts)
41. [[Terrain-Adaptive Locomotion Skills Using Deep Reinforcement Learning|Terrain-adaptive locomotion]] · Peng et al. · 2016 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=Terrain-Adaptive+Locomotion+Skills+Using+Deep+Reinforcement+Learning)
42. [[From Sparse to Soft Mixtures of Experts|Soft MoE]] · Puigcerver et al. · 2024 · ICLR · [Scholar](https://scholar.google.com/scholar?q=From+Sparse+to+Soft+Mixtures+of+Experts)
43. [[CondConv - Conditionally Parameterized Convolutions for Efficient Inference|CondConv]] · Yang et al. · 2019 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=CondConv+Conditionally+Parameterized+Convolutions+for+Efficient+Inference)
44. [[Conditional Convolutions for Instance Segmentation|CondInst]] · Tian et al. · 2020 · ECCV · [Scholar](https://scholar.google.com/scholar?q=Conditional+Convolutions+for+Instance+Segmentation)
45. [[Soft Merging of Experts with Adaptive Routing|SMEAR]] · Muqeeth et al. · 2024 · TMLR · [Scholar](https://scholar.google.com/scholar?q=Soft+Merging+of+Experts+with+Adaptive+Routing)
46. [[Mixtures of Experts Unlock Parameter Scaling for Deep RL|MoE parameter scaling]] · Obando-Ceron et al. · 2024 · ICML · [Scholar](https://scholar.google.com/scholar?q=Mixtures+of+Experts+Unlock+Parameter+Scaling+for+Deep+RL)
47. [[Learning Whole-Body Manipulation for Quadrupedal Robot|Quadruped whole-body manipulation]] · Jeon et al. · 2024 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Learning+Whole-Body+Manipulation+for+Quadrupedal+Robot)
48. [[Learning Visual Quadrupedal Loco-Manipulation from Demonstrations|Visual quadruped loco-manipulation]] · He et al. · 2024 · IROS · [Scholar](https://scholar.google.com/scholar?q=Learning+Visual+Quadrupedal+Loco-Manipulation+from+Demonstrations)
49. [[Deep Whole-Body Control - Learning a Unified Policy for Manipulation and Locomotion|Deep Whole-Body Control]] · Fu et al. · 2023 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Deep+Whole-Body+Control+Learning+a+Unified+Policy+for+Manipulation+and+Locomotion)
50. [[Whole-Body End-Effector Pose Tracking|Whole-body EE pose tracking]] · Portela et al. · 2025 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Whole-Body+End-Effector+Pose+Tracking)
51. [[OpenHEART - Opening Heterogeneous Articulated Objects with a Legged Manipulator|OpenHEART]] · Lim et al. · 2026 · ICRA · [Scholar](https://scholar.google.com/scholar?q=OpenHEART+Opening+Heterogeneous+Articulated+Objects+with+a+Legged+Manipulator)
52. [[Learning Robust Whole-Body Control for Legged Manipulation via Learned Terrain Estimation|Learned terrain estimation WBC]] · Lim et al. · 2024 · ICCAS · [Scholar](https://scholar.google.com/scholar?q=Learning+Robust+Whole-Body+Control+for+Legged+Manipulation+via+Learned+Terrain+Estimation)
53. [[Multi-Critic Learning for Whole-Body End-Effector Twist Tracking|Multi-critic twist tracking]] · Vijayan et al. · 2025 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Multi-Critic+Learning+for+Whole-Body+End-Effector+Twist+Tracking)
54. [[Perpetual Humanoid Control for Real-time Simulated Avatars|PHC]] · Luo et al. · 2023 · ICCV · [Scholar](https://scholar.google.com/scholar?q=Perpetual+Humanoid+Control+for+Real-time+Simulated+Avatars)
55. [[OmniH2O - Universal and Dexterous Human-to-Humanoid Whole-Body Teleoperation and Learning|OmniH2O]] · He et al. · 2025 · CoRL · [Scholar](https://scholar.google.com/scholar?q=OmniH2O+Universal+and+Dexterous+Human-to-Humanoid+Whole-Body+Teleoperation+and+Learning)
56. [[HOVER - Versatile Neural Whole-Body Controller for Humanoid Robots|HOVER]] · He et al. · 2025 · ICRA · [Scholar](https://scholar.google.com/scholar?q=HOVER+Versatile+Neural+Whole-Body+Controller+for+Humanoid+Robots)
57. [[AMO - Adaptive Motion Optimization for Hyper-Dexterous Humanoid Whole-Body Control|AMO]] · Li et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2505.03738)
58. [[TWIST2 - Scalable, Portable, and Holistic Humanoid Data Collection System|TWIST2]] · Ze et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2511.02832)
59. [[Retargeting Matters - General Motion Retargeting for Humanoid Motion Tracking|GMR]] · Araujo et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2510.02252)
60. [[Reinforcement Learning - An Introduction|Sutton and Barto]] · Sutton and Barto · 1998 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Reinforcement+Learning+An+Introduction)
61. [[A Reduction of Imitation Learning and Structured Prediction to No-Regret Online Learning|DAgger]] · Ross et al. · 2011 · AISTATS · [Scholar](https://scholar.google.com/scholar?q=A+Reduction+of+Imitation+Learning+and+Structured+Prediction+to+No-Regret+Online+Learning)
62. [[Isaac Lab - A GPU-Accelerated Simulation Framework for Multi-Modal Robot Learning|Isaac Lab]] · Mittal et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2511.04831)
63. [[Unitree G1|Unitree G1]] · Unitree Robotics · 2026 · Website · [Link](https://www.unitree.com/g1)
64. [[Learning to Walk in Minutes Using Massively Parallel Deep Reinforcement Learning|legged_gym]] · Rudin et al. · 2022 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Learning+to+Walk+in+Minutes+Using+Massively+Parallel+Deep+Reinforcement+Learning)
65. [[Diversity is All You Need - Learning Skills without a Reward Function|DIAYN]] · Eysenbach et al. · 2019 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Diversity+is+All+You+Need+Learning+Skills+without+a+Reward+Function)
66. [[METRA - Scalable Unsupervised RL with Metric-Aware Abstraction|METRA]] · Park et al. · 2024 · ICLR · [Scholar](https://scholar.google.com/scholar?q=METRA+Scalable+Unsupervised+RL+with+Metric-Aware+Abstraction)
67. [[Learning More Skills through Optimistic Exploration|DISDAIN]] · Strouse et al. · 2022 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Learning+More+Skills+through+Optimistic+Exploration)
68. [[Disentangled Unsupervised Skill Discovery for Efficient Hierarchical Reinforcement Learning|DUSDi]] · Hu et al. · 2024 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Disentangled+Unsupervised+Skill+Discovery+for+Efficient+Hierarchical+Reinforcement+Learning)
69. [[Divide, Discover, Deploy - Factorized Skill Learning with Symmetry and Style Priors|D3]] · Cathomen et al. · 2025 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Divide+Discover+Deploy+Factorized+Skill+Learning+with+Symmetry+and+Style+Priors)
70. [[Adam - A Method for Stochastic Optimization|Adam]] · Kingma and Ba · 2015 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Adam+A+Method+for+Stochastic+Optimization)
71. [[Fast and Accurate Deep Network Learning by Exponential Linear Units (ELUs)|ELU]] · Clevert et al. · 2016 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Fast+and+Accurate+Deep+Network+Learning+by+Exponential+Linear+Units+ELUs)
72. [[Why M Heads are Better than One - Training a Diverse Ensemble of Deep Networks|M heads]] · Lee et al. · 2015 · arXiv · [arXiv](https://arxiv.org/abs/1511.06314)
73. [[Sharing Knowledge in Multi-Task Deep Reinforcement Learning|Shared knowledge MTRL]] · D'Eramo et al. · 2020 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Sharing+Knowledge+in+Multi-Task+Deep+Reinforcement+Learning)
