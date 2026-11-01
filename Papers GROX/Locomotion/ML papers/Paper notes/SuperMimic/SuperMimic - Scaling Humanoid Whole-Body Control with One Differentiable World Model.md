---
title: "SuperMimic: Scaling Humanoid Whole-Body Control with One Differentiable World Model"
aliases:
  - SuperMimic
authors:
  - "[[Qiayuan Liao]]"
  - "[[Yiyang Shao]]"
  - "[[Junfeng Long]]"
  - "[[Kevin Zakka]]"
  - "[[Roberto Horowitz]]"
  - "[[Pieter Abbeel]]"
  - "[[Koushil Sreenath]]"
corresponding: ""
institutions:
  - "[[University of California, Berkeley]]"
countries:
  - USA
year: 2026
date: 2026-10-20
venue: CoRL 2026 (Oral)
venue_type: conference
arxiv_category: cs.RO
arxiv_id: "2610.28480"
url: https://arxiv.org/abs/2610.28480
pdf: https://arxiv.org/pdf/2610.28480
code: No official repo yet (checked 2026-10-31)
related_code: "Unofficial reproduction, not the authors' code: https://github.com/CuboiLeo/SuperMimic-Reproduced"
project_page: https://supermimic.github.io/
status: to-read
relevance: high
added: 2026-10-31
tags:
  - paper
  - topic/humanoids
  - topic/whole-body-control
  - topic/motion-tracking
  - topic/world-models
  - topic/sim-to-real
  - topic/domain-randomization
  - method/reinforcement-learning
  - method/model-based-rl
  - method/bptt
  - dataset/lafan1
  - dataset/bones-seed
  - robot/unitree-g1
  - sim/mujoco
  - eval/real-robot
---

# SuperMimic

> [!info] Quick view
> **Authors:** [[Qiayuan Liao]], [[Yiyang Shao]], [[Junfeng Long]], [[Kevin Zakka]], [[Roberto Horowitz]], [[Pieter Abbeel]], [[Koushil Sreenath]]
> **From:** [[University of California, Berkeley]]
> **Published:** CoRL 2026 (Oral) · October 2026 · cs.RO
> **Keywords:** humanoid motion tracking, differentiable world model, backpropagation through time, analytic policy gradients, model-based RL, scaling, sim-to-real, Unitree G1
> **Links:** [arXiv](https://arxiv.org/abs/2610.28480) · [PDF](https://arxiv.org/pdf/2610.28480) · [Project](https://supermimic.github.io/)
> **One line:** Learn one differentiable world model of the G1 dynamics shared by all motions, then train one tracking policy by backpropagating through short imagined rollouts, and you beat a 128-GPU PPO tracker in 3 hours on one GPU.

## Summary

This paper try to fix the compute problem of large-scale humanoid motion tracking. Right now the way to get a general whole-body controller is to track a huge motion library with PPO, and that means either distilling many per-motion experts or burning cluster-scale compute like SONIC (about 7 days on 128 L40 GPUs). The key observation is that every clip in the library shares the same robot dynamics, so instead of re-learning physics by trial and error for each motion, they fit one deterministic MLP world model on MuJoCo Warp transitions and train the policy through it with BPTT over 32-step imagined rollouts plus a critic for the value after the horizon.

The result is honestly impressive. On 129,514 BONES-SEED motions it passes the released SONIC checkpoint in 3 hours on one H100, with 25 failed motions vs 179, and reaches zero failures in 9.8 hours on two GPUs. A G1 does flips and side rolls on hardware. But the headline 1000x is not a controlled comparison: different GPUs, less than half the motion data, and evaluation only on training motions. The LAFAN1 matched-PPO experiment is the result I trust more, and it still favors SuperMimic clearly.

## How it works

- **Three-step loop, all online.** (1) The current policy collects data in MuJoCo Warp with action noise into a replay buffer, (2) the world model is fit on short windows from the buffer, (3) the policy is optimized through the frozen world model. The model and policy train together, not as a pretrain-then-use pipeline.
- **World model.** One shared deterministic MLP. Input: state, action and a per-episode physics context (friction, joint calibration offsets, torso CoM offsets). Output: next state, reward, noise-free observation and termination probability.
- **Autoregressive fitting.** Trained on 8-step windows rolled out with its own predictions, so the model sees its own compounding error during training.
- **Policy update.** Rollouts start from replay-buffer states and run 32 steps inside the world model. The policy gets first-order analytic gradients of the imagined return. Gradients never go through the simulator, so MuJoCo Warp does not need to be differentiable.
- **Critic for the tail.** A critic bootstraps the return beyond 32 steps. Without it, recoveries that take longer than the horizon basically dont get learned.
- **Noise outside the model.** Sensor noise is sampled outside the world model during imagination. The dynamics surrogate stays deterministic, but the policy still sees the noisy observations it will get on the robot.
- **Reward and setup from BeyondMimic.** The tracking reward design comes from BeyondMimic. Only the optimizer changes, from PPO to world-model BPTT.
- **Sim-to-real.** Domain randomization on friction, joint offsets and torso CoM, plus observation noise. They skipped root-velocity perturbations.

## Results that matter

- **vs SONIC.** Passes the released SONIC checkpoint's normalized return in 3 h on 1 H100 (SONIC: ~168 h on 128 L40, ~21k GPU-hours). Failed motions 25 vs 179 out of 129,514. With 2 H100s for 9.8 h it gets 0 failures. SONIC fails all 44 flips, all 22 side rolls and 15 of 24 broad jumps.
- **Controlled PPO comparison (40 LAFAN1 clips, matched rewards, observations, DR and networks).** SuperMimic reaches 0.82 normalized return in 1.2 h on one GPU. PPO after 18 h only gets 0.60 / 0.70 / 0.72 on 1 / 4 / 8 GPUs. This is the fair comparison and it still wins by a lot.
- **Throughput.** The world model runs >10x faster than MuJoCo Warp at typical batch sizes and ~62x faster at 64k environments. Against PPO running on the same world model, BPTT needs ~64x fewer imagined steps to reach PPO's best return, so the gain comes from the gradient and not only from a fast model.
- **Scaling.** From 1 to 264 hours of motion data, the 8-step prediction error stays around 0.007 to 0.011, and the GPU-hours to a fixed return grow slowly and then flatten. That supports the idea that more clips are mostly more observations of the same dynamics.
- **Ablations.** Without a critic, H=8 and H=16 fail. With a critic, H=16 matches critic-free H=32, and H=32 is fastest. H=128 almost triples time-to-target and ends at lower return. Under pushes, the critic policy settles in 0.5 to 2.4 s, while the critic-free one stumbles at 0.5 m/s and falls at 4 m/s.
- **What hurts the story.** No held-out motion benchmark, so "zero failures" only means it fits the training library. The real G1 results are only demos, with no tracking error, success rate or trial count.

## Interesting stuff

- The core insight is simple and really strong: in motion tracking the dynamics are shared across the whole library, so you learn them once and amortize them. PPO kinda re-discovers the same physics through high-variance sampling for every motion.
- It is a learned model, not a differentiable simulator. That avoids the contact non-smoothness that kills SHAC-style methods on hard contacts, and it works with any normal simulator. The cost is model bias, and they don't really study how much the policy exploits model errors.
- The SONIC comparison is marketing-flavored: H100 vs L40, 264 h vs 611 h of data, different software stack, and SONIC's own failure rule is different from theirs. The 1000x number depends on their accounting.
- There's no official code, but several people already reproduced it from the paper alone (one community version reports training on a single RTX 4090 in about 10.5 h). That is a good sign that the method is real and not brittle.
- All of it is flat ground and free-standing motions. No terrain, no objects, no real-world data refining the model. The scaling claim depends on the assumption that every clip uses the same G1 body.

## Useful for me

- **Apply.** The world model is already conditioned on a per-episode physics context (friction, joint offsets, CoM). Adding morphology parameters to that context is the most direct way to get a morphology-aware world model, and then BPTT gives a morphology-aware tracker almost for free in compute.
- **Gap.** Their amortization works because every clip has the same body. A body that changes during the episode breaks that assumption unless the model knows the morphology and can predict the transition. Nobody shows a world model that stays accurate while the body changes mid-rollout.
- **As prior work.** Someone already built a community extension called MorphMimic, a morphology-conditioned world model with policy sharing across humanoids ([GitHub](https://github.com/lzyang2000/morphmimic)). It's unofficial and I haven't checked how good it is, but it's the closest thing to our idea built on this paper, so read it before claiming novelty.
- **Apply.** If training on one GPU in hours holds up, we can iterate on the controller as fast as we iterate on the design. That fits co-design loops where the body changes between runs.
- **Watch out.** The real-robot evidence is only demos, and the model never sees real data. For sim-to-real on a new body, we would still need our own system ID or real-data fine-tuning on top.

## Key prior work

- [[SONIC - Supersizing Motion Tracking for Natural Humanoid Whole-Body Control|SONIC]] · the cluster-scale PPO tracker they beat, the main baseline
- [[BeyondMimic - From Motion Tracking to Versatile Humanoid Control via Guided Diffusion|BeyondMimic]] · same group, they keep its tracking reward and only swap the optimizer
- [[Accelerated Policy Learning with Parallel Differentiable Simulation|SHAC]] · short-horizon BPTT plus a critic, the same recipe but through a differentiable simulator instead of a learned model
- [[Dream to Control - Learning Behaviors by Latent Imagination|Dreamer]] · learning behaviors in imagination with analytic value gradients through a learned model
- [[SuperTrack - Motion Tracking for Physically Simulated Characters using Supervised Learning|SuperTrack]] · the graphics ancestor, a world model used as a differentiable simulator for motion tracking
- [[Proximal Policy Optimization Algorithms|PPO]] · the model-free baseline the whole paper argues against

## References

1. [[SONIC - Supersizing Motion Tracking for Natural Humanoid Whole-Body Control|SONIC]] · Luo et al. · 2026 · Science Robotics · [DOI](https://doi.org/10.1126/scirobotics.aed4592)
2. [[DeepMimic - Example-Guided Deep Reinforcement Learning of Physics-Based Character Skills|DeepMimic]] · Peng et al. · 2018 · ACM TOG · [DOI](https://doi.org/10.1145/3197517.3201311)
3. [[BeyondMimic - From Motion Tracking to Versatile Humanoid Control via Guided Diffusion|BeyondMimic]] · Liao et al. · 2026 · Science Robotics · [DOI](https://doi.org/10.1126/scirobotics.adx8924)
4. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
5. [[Neural Probabilistic Motor Primitives for Humanoid Control|NPMP]] · Merel et al. · 2019 · ICLR 2019 · [Link](https://openreview.net/forum?id=BJl6TjRcY7)
6. [[ControlVAE - Model-Based Learning of Generative Controllers for Physics-Based Characters|ControlVAE]] · Yao et al. · 2022 · ACM TOG · [DOI](https://doi.org/10.1145/3550454.3555434)
7. [[Physics-based Character Controllers Using Conditional VAEs|Conditional VAE Controllers]] · Won et al. · 2022 · ACM TOG · [DOI](https://doi.org/10.1145/3528223.3530067)
8. [[Universal Humanoid Motion Representations for Physics-Based Control|PULSE]] · Luo et al. · 2024 · ICLR 2024 · [Link](https://proceedings.iclr.cc/paper_files/paper/2024/hash/fb19caf91f4966e426026685028cb73a-Abstract-Conference.html)
9. [[MaskedMimic - Unified Physics-Based Character Control Through Masked Motion Inpainting|MaskedMimic]] · Tessler et al. · 2024 · ACM TOG · [DOI](https://doi.org/10.1145/3687951)
10. [[Learning Athletic Humanoid Tennis Skills from Imperfect Human Motion Data|Humanoid Tennis]] · Zhang et al. · 2026 · IROS 2026 · [arXiv](https://arxiv.org/abs/2603.12686)
11. [[Perpetual Humanoid Control for Real-time Simulated Avatars|PHC]] · Luo et al. · 2023 · ICCV 2023 · [Link](https://openaccess.thecvf.com/content/ICCV2023/html/Luo_Perpetual_Humanoid_Control_for_Real-time_Simulated_Avatars_ICCV_2023_paper.html)
12. [[SuperTrack - Motion Tracking for Physically Simulated Characters using Supervised Learning|SuperTrack]] · Fussell et al. · 2021 · ACM TOG · [DOI](https://doi.org/10.1145/3478513.3480527)
13. [[MoConVQ - Unified Physics-Based Motion Control via Scalable Discrete Representations|MoConVQ]] · Yao et al. · 2024 · ACM TOG · [DOI](https://doi.org/10.1145/3658137)
14. [[Scalable Humanoid Whole-Body Control via Differentiable Neural Network Dynamics|Neural Dynamics WBC]] · Lei et al. · 2025 · ICLR 2025 Workshop on World Models · [Link](https://openreview.net/pdf/b3031990328dd52267a571b265c15328bf6d6120.pdf)
15. [[Dream to Control - Learning Behaviors by Latent Imagination|Dreamer]] · Hafner et al. · 2020 · ICLR 2020 · [Link](https://openreview.net/forum?id=S1lOTC4tDS)
16. [[Mastering Diverse Control Tasks through World Models|DreamerV3]] · Hafner et al. · 2025 · Nature · [DOI](https://doi.org/10.1038/s41586-025-08744-2)
17. [[Temporal Difference Learning for Model Predictive Control|TD-MPC]] · Hansen et al. · 2022 · ICML 2022 · [Link](https://proceedings.mlr.press/v162/hansen22a.html)
18. [[DayDreamer - World Models for Physical Robot Learning|DayDreamer]] · Wu et al. · 2023 · CoRL 2022 · [Link](https://proceedings.mlr.press/v205/wu23c.html)
19. [[Accelerated Policy Learning with Parallel Differentiable Simulation|SHAC]] · Xu et al. · 2022 · ICLR 2022 · [Link](https://openreview.net/forum?id=ZSKRQMvttc)
20. [[Learning Quadruped Locomotion Using Differentiable Simulation|Diff-Sim Quadruped]] · Song et al. · 2025 · CoRL 2024 · [Link](https://proceedings.mlr.press/v270/song25a.html)
21. [[Robotic World Model - A Neural Network Simulator for Robust Policy Optimization in Robotics|Robotic World Model]] · Li et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2501.10100)
22. [[Uncertainty-Aware Robotic World Model Makes Offline Model-Based Reinforcement Learning Work on Real Robots|Uncertainty-Aware RWM]] · Li et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2504.16680)
23. [[Dyna, an Integrated Architecture for Learning, Planning, and Reacting|Dyna]] · Sutton · 1991 · ACM SIGART Bulletin · [DOI](https://doi.org/10.1145/122344.122377)
24. [[When to Trust Your Model - Model-Based Policy Optimization|MBPO]] · Janner et al. · 2019 · NeurIPS 2019 · [Link](https://papers.nips.cc/paper_files/paper/2019/hash/5faf461eff3099671ad63c6f3f094f7f-Abstract.html)
25. [[High-Dimensional Continuous Control Using Generalized Advantage Estimation|GAE]] · Schulman et al. · 2016 · ICLR 2016 · [arXiv](https://arxiv.org/abs/1506.02438)
26. [[MuJoCo - A physics engine for model-based control|MuJoCo]] · Todorov et al. · 2012 · IROS 2012 · [Scholar](https://scholar.google.com/scholar?q=MuJoCo+A+physics+engine+for+model-based+control)
27. [[BONES-SEED - Skeletal Everyday Embodiment Dataset|BONES-SEED]] · Bones Studio · 2026 · Dataset · [Link](https://huggingface.co/datasets/bones-studio/seed)
28. [[Robust Motion In-betweening|LAFAN1]] · Harvey et al. · 2020 · ACM TOG · [DOI](https://doi.org/10.1145/3386569.3392480)
## See also

- [[Scalable Humanoid Whole-Body Control via Differentiable Neural Network Dynamics|Neural Dynamics WBC]] · Lei et al. · 2025 · ICLR 2025 Workshop on World Models · [Link](https://mlanthology.org/iclrw/2025/lei2025iclrw-scalable/) · the same idea of neural dynamics as a differentiable simulator for humanoid tracking, one year earlier