---
title: "Structural Optimization of Lightweight Bipedal Robot via SERL"
aliases:
  - SERL
  - Wow Orin
authors:
  - "[[Yi Cheng]]"
  - "[[Chenxi Han]]"
  - "[[Yuheng Min]]"
  - "[[Houde Liu]]"
  - "[[Linqi Ye]]"
  - "[[Hang Liu]]"
corresponding: "[[Houde Liu]]"
institutions:
  - "[[Tsinghua University]]"
  - "[[Jianghuai Advanced Technology Center]]"
  - "[[Shanghai University]]"
  - "[[University of Michigan]]"
labs: []
countries:
  - China
  - USA
year: 2024
date: 2024-10-14
venue: IEEE/RSJ IROS 2024
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1109/IROS58592.2024.10801349
pdf: https://doi.org/10.1109/IROS58592.2024.10801349
code: "No official repo found, project page only (checked 2026-11-02)"
project_page: https://SERL-IROS2024.github.io
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/co-design
  - topic/morphology
  - topic/legged-locomotion
  - topic/hardware-design
  - topic/leg-design
  - topic/sim-to-real
  - method/reinforcement-learning
  - method/evolutionary-algorithms
  - method/teacher-student
  - sim/isaac-gym
  - robot/cassie
  - robot/unitree-h1
  - eval/real-robot
---

# SERL

> [!info] Quick view
> **Authors:** [[Yi Cheng]], [[Chenxi Han]], [[Yuheng Min]], [[Houde Liu]] (corresponding), [[Linqi Ye]] (corresponding), [[Hang Liu]]
> **From:** [[Tsinghua University]] · [[Jianghuai Advanced Technology Center]] · [[Shanghai University]] · [[University of Michigan]]
> **Published:** IEEE/RSJ IROS 2024 · Oct 2024 · conference
> **Keywords:** structure evolution reinforcement learning, genetic algorithm, leg length optimization, co-design, Bowden cable ankle, Wow Orin, cost of transport
> **Links:** [DOI](https://doi.org/10.1109/IROS58592.2024.10801349) · [Project page](https://SERL-IROS2024.github.io)
> **One line:** a genetic algorithm over thigh and shin lengths with an RL locomotion policy trained inside the loop picks the leg lengths of a 10.5 kg biped, which is then built and walks on hardware

## Summary

SERL couples a genetic algorithm with RL to choose a biped's thigh and shin lengths in [0.2, 0.4] m (1 cm resolution). A population of 250 leg-length pairs is trained with PPO in Isaac Gym, the reward of each individual is its fitness, and roulette selection, crossover and mutation produce new lengths, until convergence. During this stage the policy gets privileged terrain and dynamics info plus the leg lengths, encoded to a latent, and mass and inertia scale with link length (uniform density). They insist on "fair rules": same command and push sampling for all individuals. In a second stage, the policy for the chosen legs is distilled into a deployable proprioceptive policy with a GRU history encoder and velocity estimation.

For a general locomotion task with pushes, it converges to thigh 0.31 m and shin 0.36 m, so a longer shin than thigh, unlike the usual equal-length design. For a maximum speed task, it converges faster to more similar lengths (0.33, 0.35). A grid of rewards over the design space agrees with the GA optimum. They then build Wow Orin, 9 DoF, 10.5 kg, Unitree A1 motors, knee driven by a belt and ankle by a "fishbone" Bowden cable from the body, legs of 1.3 kg each. In simulation with the same RL setup it has a lower COT (0.41) than Cassie (0.76) and H1 (0.72), and 2.1 m/s top speed vs 3.2 for Cassie and 1.9 for H1. Honestly, comparing very different robots with one training recipe in sim is a weak comparison, and the design space is only 2-D, but the full loop from optimization to built robot is nice.

## How it works

- GA: 9-bit DNA per individual, population 250, crossover 0.8, mutation 0.03, 10 policy iterations per evolution step, roulette selection on shifted reward
- RL stage 1: PPO with proprioception, velocity and privileged info (terrain, body mass, friction) plus leg lengths encoded to a latent
- 500 warm-up iterations before evolution so the GA does not just pick the lengths that learn fastest
- Link mass and inertia scale with length assuming uniform density
- Stage 2: teacher-student distillation to a proprioceptive policy with a GRU history encoder and a velocity estimator
- Robot: 9 DoF (shared hip roll motor), Unitree A1 motors, belt knee, Bowden cable ankle with spherical-joint segments in an oil tube
- About 12 h for SERL and 1 h for distillation on one RTX 4090

## Results that matter

- Comprehensive locomotion task: converges to thigh 0.31 m, shin 0.36 m at about 700k steps
- Max velocity task: converges to (0.33, 0.35) after about 200k steps
- Exhaustive reward map over the design space matches the GA result
- Wow Orin vs Cassie vs H1 (sim, same recipe): COT 0.407 vs 0.762 vs 0.718, max speed 2.1 vs 3.2 vs 1.9 m/s, Froude 0.75 vs 1.04 vs 0.46
- Real robot walking shown, mostly qualitative

## Interesting stuff

- Treating leg lengths as part of the observation during evolution lets one policy share knowledge across bodies, close to a body-conditioned policy
- Different tasks give different optimal legs, a direct argument for task-dependent morphology
- The fairness rule for comparing individuals is an important detail that many co-design papers miss
- COT comparison across robots of 10 to 47 kg trained with the same recipe is not a fair hardware comparison
- No comparison against other co-design methods (BO, DERL style, gradient)

## Useful for me

- **As prior work.** Direct example of RL-in-the-loop leg length optimization for a biped, with hardware. Good citation for task-dependent limb proportions.
- **Apply.** The finding that optimal thigh/shin ratio changes with the task supports a humanoid that changes limb lengths between tasks.
- **Apply.** Warm-up training before morphology selection and fixed evaluation conditions ("fair rules") are cheap tricks to avoid biased morphology search.
- **Gap.** The leg length is fixed after design. A body that can change lengths online could use exactly this kind of map from task to optimal length.

## Key prior work

- [[Embodied intelligence via learning and evolution|DERL]] · evolution plus RL for morphology
- [[Meta Reinforcement Learning for Optimal Design of Legged Robots|Meta-RL legged design]] · RL-based design optimization for legged robots
- [[Evolution Gym - A Large-Scale Benchmark for Evolving Soft Robots|Evolution Gym]] · co-design benchmark
- [[Humanoid robot co-design - Coupling hardware design with gait generation via hybrid zero dynamics|HZD humanoid co-design]] · model-based humanoid co-design
- [[Codesign of Humanoid Robots for Ergonomic Collaboration with Multiple Humans via Genetic Algorithms and Nonlinear Optimization|Ergonomic humanoid co-design]] · GA humanoid co-design
- [[Computational design of energy-efficient legged robots - Optimizing for size and actuators|Size and actuator co-design]] · bilevel co-design of size
- [[Task-based limb optimization for legged robots|Task-based limb optimization]] · task-driven limb lengths

## References

1. [[BirdBot achieves energy-efficient gait with minimal control using avian-inspired leg clutching|BirdBot]] · Badri-Spröwitz et al. · 2022 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=BirdBot+achieves+energy-efficient+gait+with+minimal+control+using+avian-inspired+leg+clutching)
2. [[Towards a bio-inspired leg design for high-speed running|Bio-inspired high-speed leg]] · Ananthanarayanan et al. · 2012 · Bioinspiration & Biomimetics · [Scholar](https://scholar.google.com/scholar?q=Towards+a+bio-inspired+leg+design+for+high-speed+running)
3. [[Flexible muscle-based locomotion for bipedal creatures|Muscle-based bipedal locomotion]] · Geijtenbeek et al. · 2013 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=Flexible+muscle-based+locomotion+for+bipedal+creatures)
4. [[Task-based limb optimization for legged robots|Task-based limb optimization]] · Ha et al. · 2016 · IROS · [Scholar](https://scholar.google.com/scholar?q=Task-based+limb+optimization+for+legged+robots)
5. [[Embodied intelligence via learning and evolution|DERL]] · Gupta et al. · 2021 · Nature Communications · [arXiv](https://arxiv.org/abs/2102.02202)
6. [[Meta Reinforcement Learning for Optimal Design of Legged Robots|Meta-RL legged design]] · Belmonte-Baeza et al. · 2022 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Meta+Reinforcement+Learning+for+Optimal+Design+of+Legged+Robots)
7. [[Evolution Gym - A Large-Scale Benchmark for Evolving Soft Robots|Evolution Gym]] · Bhatia et al. · 2021 · NeurIPS · [Scholar](https://scholar.google.com/scholar?q=Evolution+Gym+A+Large-Scale+Benchmark+for+Evolving+Soft+Robots)
8. [[Learning quadrupedal locomotion over challenging terrain|Challenging terrain locomotion]] · Lee et al. · 2020 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+quadrupedal+locomotion+over+challenging+terrain)
9. [[RMA - Rapid Motor Adaptation for Legged Robots|RMA]] · Kumar et al. · 2021 · RSS · [arXiv](https://arxiv.org/abs/2107.04034)
10. [[Learning robust perceptive locomotion for quadrupedal robots in the wild|Perceptive locomotion in the wild]] · Miki et al. · 2022 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+robust+perceptive+locomotion+for+quadrupedal+robots+in+the+wild)
11. [[Rapid Locomotion via Reinforcement Learning|Rapid locomotion]] · Margolis et al. · 2022 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Rapid+locomotion+via+reinforcement+learning)
12. [[Neural Volumetric Memory for Visual Locomotion Control|Neural volumetric memory]] · Yang et al. · 2023 · CVPR · [Scholar](https://scholar.google.com/scholar?q=Neural+Volumetric+Memory+for+Visual+Locomotion+Control)
13. [[Robot Parkour Learning|Robot Parkour Learning]] · Zhuang et al. · 2023 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Robot+Parkour+Learning)
14. [[ANYmal parkour - Learning agile navigation for quadrupedal robots|ANYmal parkour]] · Hoeller et al. · 2024 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=ANYmal+parkour+Learning+agile+navigation+for+quadrupedal+robots)
15. [[Sim-to-Real Learning of All Common Bipedal Gaits via Periodic Reward Composition|Periodic reward composition]] · Siekmann et al. · 2021 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Sim-to-Real+Learning+of+All+Common+Bipedal+Gaits+via+Periodic+Reward+Composition)
16. [[Reinforcement Learning for Robust Parameterized Locomotion Control of Bipedal Robots|Parameterized bipedal RL]] · Li et al. · 2021 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Reinforcement+Learning+for+Robust+Parameterized+Locomotion+Control+of+Bipedal+Robots)
17. [[Benchmarking Potential Based Rewards for Learning Humanoid Locomotion|Potential-based rewards]] · Jeon et al. · 2023 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Benchmarking+potential+based+rewards+for+learning+humanoid+locomotion)
18. [[Learning to Walk in Minutes Using Massively Parallel Deep Reinforcement Learning|legged_gym]] · Rudin et al. · 2021 · CoRL · [arXiv](https://arxiv.org/abs/2109.11978)
19. [[Humanoid robot co-design - Coupling hardware design with gait generation via hybrid zero dynamics|HZD humanoid co-design]] · Ghansah et al. · 2023 · CDC · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+co-design+Coupling+hardware+design+with+gait+generation+via+hybrid+zero+dynamics)
20. [[Codesign of Humanoid Robots for Ergonomic Collaboration with Multiple Humans via Genetic Algorithms and Nonlinear Optimization|Ergonomic humanoid co-design]] · Sartore et al. · 2023 · Humanoids · [arXiv](https://arxiv.org/abs/2312.07459)
21. [[Computational design of energy-efficient legged robots - Optimizing for size and actuators|Size and actuator co-design]] · Fadini et al. · 2021 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Computational+design+of+energy-efficient+legged+robots+Optimizing+for+size+and+actuators)
22. [[A versatile co-design approach for dynamic legged robots|Versatile co-design]] · Dinev et al. · 2022 · IROS · [Scholar](https://scholar.google.com/scholar?q=A+versatile+co-design+approach+for+dynamic+legged+robots)
23. [[Large-scale ADMM-based co-design of legged robots|ADMM co-design]] · Bravo-Palacios & Wensing · 2022 · IROS · [Scholar](https://scholar.google.com/scholar?q=Large-scale+ADMM-based+codesign+of+legged+robots)
24. [[Computational co-optimization of design parameters and motion trajectories for robotic systems|Design and trajectory co-optimization]] · Ha et al. · 2018 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Computational+co-optimization+of+design+parameters+and+motion+trajectories+for+robotic+systems)
25. [[One robot for many tasks - Versatile co-design through stochastic programming|One robot for many tasks]] · Bravo-Palacios et al. · 2020 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=One+robot+for+many+tasks+Versatile+co-design+through+stochastic+programming)
26. [[Froude and the contribution of naval architecture to our understanding of bipedal locomotion|Froude and bipedal locomotion]] · Vaughan & O'Malley · 2005 · Gait & Posture · [Scholar](https://scholar.google.com/scholar?q=Froude+and+the+contribution+of+naval+architecture+to+our+understanding+of+bipedal+locomotion)

## See also

- [[Task-Aware Morphology Optimization of Planar Manipulators via Reinforcement Learning|Task-aware planar manipulator morphology]] · RL-based morphology optimization for arms
- [[Multi-embodiment Legged Robot Control as a Sequence Modeling Problem|EAT]] · body-conditioned policy plus BO over leg lengths on hardware
