---
title: "MimicAll: Structured Embodiment Conditioning for Cross-Humanoid Whole-Body Motion Tracking"
aliases:
  - MimicAll
authors:
  - "[[Jia Li]]"
  - "[[Lingyun Xu]]"
  - "[[Yitang Li]]"
  - "[[Hao Zhao]]"
corresponding: "[[Hao Zhao]]"
institutions:
  - "[[Huazhong University of Science and Technology]]"
  - "[[Peking University]]"
  - "[[Tsinghua University]]"
labs:
  - "[[Roboparty Lab]]"
countries:
  - China
year: 2026
date: 2026-10-24
venue: arXiv preprint
venue_type: preprint
arxiv_category: cs.RO
arxiv_id: "2610.33508"
url: https://arxiv.org/abs/2610.33508
pdf: https://arxiv.org/pdf/2610.33508v1
code: "No official repo yet (checked 2026-10-29)"
related_code: https://github.com/roboparty/mimiclite
status: read
relevance: medium
added: 2026-10-29
tags:
  - paper
  - topic/humanoids
  - topic/motion-tracking
  - topic/whole-body-control
  - topic/cross-embodiment
  - topic/morphology
  - topic/domain-randomization
  - method/reinforcement-learning
  - method/ppo
  - method/transformer
  - method/motion-retargeting
  - dataset/lafan1
  - dataset/100style
  - robot/unitree-g1
  - eval/sim-only
---

# MimicAll

> [!info] Quick view
> **Authors:** [[Jia Li]], [[Lingyun Xu]], [[Yitang Li]], [[Hao Zhao]] (corresponding)
> **From:** [[Tsinghua University]] · [[Roboparty Lab]] · [[Peking University]] · [[Huazhong University of Science and Technology]]
> **Published:** arXiv preprint · Oct 2026 · cs.RO
> **Keywords:** cross-embodiment, whole-body motion tracking, embodiment descriptor, morphology randomization, reinforcement learning, humanoids
> **Links:** [arXiv](https://arxiv.org/abs/2610.33508) · [PDF](https://arxiv.org/pdf/2610.33508v1) · [Base code, MimicLite (not MimicAll)](https://github.com/roboparty/mimiclite)
> **One line:** one RL motion tracking policy shared across thousands of humanoid bodies, conditioned on an explicit description of each body

## Summary

This paper try to train one single RL policy that can track whole-body human motion across many different humanoids instead of only one robot. Normally motion trackers are trained for a specific body, like the Unitree G1, and the policy breaks when the body changes. MimicAll takes 16 humanoid families, randomizes them into 4,096 body variants, and conditions the policy on an explicit embodiment descriptor that includes the joint topology, link geometry, mass and inertia, PD gains and actuator limits. The policy combines that descriptor with its observation history and a body-matched reference motion, and outputs joint target offsets.

The results are honestly mixed. Training across many bodies makes the policy much more robust than a single-body specialist, on randomized G1 bodies the success rate goes from 43% to 99%. However, the structured encoder adds very little over a simple baseline, and on robot families the policy never saw it basically fails. So in practice it works more like a very strong embodiment randomization than a true general cross-humanoid controller.

## How it works

- Every robot is mapped into 38 shared semantic joint slots with presence and action masks, so one network can control robots with different numbers of joints
- The embodiment descriptor has three parts, structure (joint tree and joint types), morphology (link offsets, joint axes, limits) and dynamics (mass, COM, inertia, PD gains, effort limits, armature, friction)
- A graph transformer, or a flat MLP as the ablation, encodes the descriptor into one global body vector and one vector per joint
- The policy observes 0.32 s of joint history, the last three actions and a reference window with only 80 ms of lookahead
- Actions are normalized joint target offsets sent to a PD controller
- Training uses PPO with 8 GPUs and 8,192 environments each, DeepMimic style tracking rewards, and 64 LAFAN1 clips retargeted to every body variant
- The randomization is physically coupled, when the geometry changes the mass and inertia change with it, so the simulated body and its descriptor always match
- They also propose a benchmark of 464 bodies split into seen families, new bodies of seen families, and fully unseen families

## Results that matter

- Seen families reach around 67% success on 30 s LAFAN1 clips and around 98% on 100STYLE
- Unseen families reach at most 8% on LAFAN1, so zero-shot transfer to new robots does not work
- With no body information at all (only joint masks) the policy already gets 64.8% vs 67.6% for the best configuration, and each configuration is trained with a single seed
- The flat MLP encoder performs the same as the graph encoder, while the attention variants cost almost 2x compute and perform worse
- Pretraining on all bodies and then fine-tuning on the G1 beats training only on the G1 (545 mm vs 689 mm global error)
- Feeding the wrong body description drops success from 61.7% to 47%, so the policy does use the embodiment information

## Interesting stuff

- The strongest contribution is the infrastructure more than the architecture, mixed robot families in one simulation, streaming references so GPU memory dont scale with the dataset, and caching body encodings
- There is no real robot or sim-to-sim validation, and the randomization ranges are not published, which makes it hard to reproduce
- Global error of 600 to 900 mm is large, the local error of 50 to 70 mm is the number that actually reflects tracking quality
- On unseen robots a simple identity label beats the full physical descriptor, which suggests the policy memorizes families more than it learns a physics-to-control mapping
- The 80 ms lookahead is short compared with other trackers, good for teleoperation latency but probably limiting for highly dynamic motions

## Useful for me

- **As prior work.** One of the closest papers to a policy that knows its own body. Good reference for arguing that a humanoid that changes its morphology needs body-aware control. Their bodies stay fixed during each episode while ours would change during the task, and that is a gap this paper does not cover.
- **Apply.** When the morphology changes in simulation, mass, COM and inertia have to change together instead of being randomized independently, otherwise the policy trains on bodies that are physically impossible.
- **Apply.** Feed the current body parameters directly into the policy. A simple MLP encoder is enough, the graph and attention versions are not worth the compute.
- **Apply.** Pretrain across many body variants and then fine-tune on the nominal configuration, which gave better tracking than a specialist trained from scratch.
- **Watch out.** Do not expect transfer from other robot families. It is better to train across the full range of our own body configurations.

## Key prior work

- [[MimicLite - Efficient and Effective General Humanoid Motion Tracking|MimicLite]] · the training stack MimicAll is built on, same lab
- [[Hardware Conditioned Policies for Multi-Robot Transfer Learning|Hardware Conditioned Policies]] · the original idea of feeding the robot hardware as a vector into the policy
- [[MetaMorph - Learning Universal Controllers with Transformers|MetaMorph]] · transformers over the body graph for universal control, the base of the graph encoder idea
- [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · one locomotion policy across many legged robots, very close in spirit
- [[Scalable and General Whole-Body Control for Cross-Humanoid Locomotion|XHugWBC]] · direct cross-humanoid competitor, but without explicit body descriptors in the actor
- [[Embodiment-Aware Generalist Specialist Distillation for Unified Humanoid Whole-Body Control|EAGLE]] · the other route, distill many specialists into one generalist
- [[X-WBC - A Cross-Embodiment Foundation Model for Humanoid Whole-Body Control|X-WBC]] · foundation model version of the same problem, worth comparing
- [[DeepMimic - Example-Guided Deep Reinforcement Learning of Physics-Based Character Skills|DeepMimic]] · where the tracking reward formulation comes from

## References

1. [[DeepMimic - Example-Guided Deep Reinforcement Learning of Physics-Based Character Skills|DeepMimic]] · Peng et al. · 2018 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=DeepMimic+Example-Guided+Deep+Reinforcement+Learning+of+Physics-Based+Character+Skills)
2. [[Perpetual Humanoid Control for Real-time Simulated Avatars|PHC]] · Luo et al. · 2023 · ICCV · [Scholar](https://scholar.google.com/scholar?q=Perpetual+Humanoid+Control+for+Real-time+Simulated+Avatars)
3. [[SONIC - Supersizing Motion Tracking for Natural Humanoid Whole-Body Control|SONIC]] · Luo et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2511.07820)
4. [[HoloMotion-1 Technical Report|HoloMotion-1]] · Chen et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2605.15336)
5. [[Hardware Conditioned Policies for Multi-Robot Transfer Learning|Hardware Conditioned Policies]] · Chen et al. · 2018 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Hardware+Conditioned+Policies+for+Multi-Robot+Transfer+Learning)
6. [[MetaMorph - Learning Universal Controllers with Transformers|MetaMorph]] · Gupta et al. · 2022 · ICLR · [Scholar](https://scholar.google.com/scholar?q=MetaMorph+Learning+Universal+Controllers+with+Transformers)
7. [[Universal Morphology Control via Contextual Modulation|ModuMorph]] · Xiong et al. · 2023 · ICML · [Scholar](https://scholar.google.com/scholar?q=Universal+Morphology+Control+via+Contextual+Modulation)
8. [[Scalable and General Whole-Body Control for Cross-Humanoid Locomotion|XHugWBC]] · Xue et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2602.05791)
9. [[Embodiment-Aware Generalist Specialist Distillation for Unified Humanoid Whole-Body Control|EAGLE]] · Peng et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2602.02960)
10. [[VENOM - Versatile Embodied Network for Omni-bodied Motion tracking|VENOM]] · Padmanabhan et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2606.16696)
11. [[One Policy to Run Them All - an End-to-end Learning Approach to Multi-Embodiment Locomotion|URMA]] · Bohlinger et al. · 2025 · CoRL · [Scholar](https://scholar.google.com/scholar?q=One+Policy+to+Run+Them+All+an+End-to-end+Learning+Approach+to+Multi-Embodiment+Locomotion)
12. [[Body Transformer - Leveraging Robot Embodiment for Policy Learning|Body Transformer]] · Sferrazza et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2408.06316)
13. [[Any2Any - Efficient Cross-Embodiment Transfer for Humanoid Whole-Body Tracking|Any2Any]] · Yang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2605.23733)
14. [[Retargeting Matters - General Motion Retargeting for Humanoid Motion Tracking|GMR]] · Araujo et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2510.02252)
15. [[OmniRetarget - Interaction-Preserving Data Generation for Humanoid Whole-Body Loco-Manipulation and Scene Interaction|OmniRetarget]] · Yang et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2509.26633)
16. [[Visualizing Data using t-SNE|t-SNE]] · van der Maaten & Hinton · 2008 · JMLR · [Scholar](https://scholar.google.com/scholar?q=Visualizing+Data+using+t-SNE)
17. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
18. [[On the Continuity of Rotation Representations in Neural Networks|6D Rotation]] · Zhou et al. · 2019 · CVPR · [Scholar](https://scholar.google.com/scholar?q=On+the+Continuity+of+Rotation+Representations+in+Neural+Networks)
19. [[FiLM - Visual Reasoning with a General Conditioning Layer|FiLM]] · Perez et al. · 2018 · AAAI · [Scholar](https://scholar.google.com/scholar?q=FiLM+Visual+Reasoning+with+a+General+Conditioning+Layer)
20. [[Scalable Diffusion Models with Transformers|DiT]] · Peebles & Xie · 2023 · ICCV · [Scholar](https://scholar.google.com/scholar?q=Scalable+Diffusion+Models+with+Transformers)
21. [[Attention Is All You Need|Transformer]] · Vaswani et al. · 2017 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Attention+Is+All+You+Need)
22. [[VICReg - Variance-Invariance-Covariance Regularization for Self-Supervised Learning|VICReg]] · Bardes et al. · 2022 · ICLR · [Scholar](https://scholar.google.com/scholar?q=VICReg+Variance-Invariance-Covariance+Regularization+for+Self-Supervised+Learning)
23. [[Robust Motion In-betweening|LAFAN1]] · Harvey et al. · 2020 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=Robust+Motion+In-betweening)
24. [[Real-Time Style Modelling of Human Locomotion via Feature-Wise Transformations and Local Motion Phases|100STYLE]] · Mason et al. · 2022 · ACM CGIT · [Scholar](https://scholar.google.com/scholar?q=Real-Time+Style+Modelling+of+Human+Locomotion+via+Feature-Wise+Transformations+and+Local+Motion+Phases)
25. [[X-WBC - A Cross-Embodiment Foundation Model for Humanoid Whole-Body Control|X-WBC]] · Zhang et al. · 2026 · arXiv · [arXiv](https://arxiv.org/abs/2609.15213)
26. [[Muon - An optimizer for hidden layers in neural networks|Muon]] · Jordan et al. · 2024 · writeup · [Scholar](https://scholar.google.com/scholar?q=Muon+An+optimizer+for+hidden+layers+in+neural+networks)
27. [[SOMA Retargeter - SOMA BVH to Humanoid Robot Motion Retargeting|SOMA Retargeter]] · NVIDIA · 2026 · software · [GitHub search](https://github.com/search?q=SOMA+Retargeter&type=repositories)
28. [[human-humanoid-tools (hhtools) - Humanoid Motion Retargeting and Dataset Analysis|hhtools]] · hhtools contributors · 2026 · software · [GitHub search](https://github.com/search?q=human-humanoid-tools&type=repositories)
29. [[ProtoMotions3 - An Open-source Framework for Humanoid Simulation and Control|ProtoMotions3]] · Tessler et al. · 2025 · software · [GitHub search](https://github.com/search?q=ProtoMotions&type=repositories)
30. [[MimicLite - Efficient and Effective General Humanoid Motion Tracking|MimicLite]] · Roboparty Lab · 2026 · tech report · [GitHub](https://github.com/roboparty/mimiclite)

## See also

- [[BeyondMimic - From Motion Tracking to Versatile Humanoid Control via Guided Diffusion|BeyondMimic]] · not cited here, but it's the main real-robot reference for general motion tracking · [arXiv](https://arxiv.org/abs/2508.08241)