---
title: "Task-Aware Morphology Optimization of Planar Manipulators via Reinforcement Learning"
aliases:
  - Task-Aware Manipulator Morphology
  - Task-aware planar manipulator morphology
authors:
  - "[[Arvind Kumar Mishra]]"
  - "[[Sohom Chakrabarty]]"
corresponding: ""
institutions:
  - "[[Indian Institute of Technology Roorkee]]"
labs: []
countries:
  - India
year: 2025
date: 2025-11-16
venue: arXiv preprint
venue_type: preprint
arxiv_category: cs.RO
arxiv_id: "2511.12650"
url: https://arxiv.org/abs/2511.12650
pdf: https://arxiv.org/pdf/2511.12650v1
code: "No official repo found (checked 2026-11-02)"
status: to-read
relevance: low
added: 2026-11-02
tags:
  - paper
  - topic/morphology
  - topic/co-design
  - topic/manipulation
  - method/reinforcement-learning
  - method/sac
  - method/ppo
  - method/evolutionary-algorithms
  - method/bayesian-optimization
  - eval/sim-only
---

# Task-Aware Manipulator Morphology

> [!info] Quick view
> **Authors:** [[Arvind Kumar Mishra]], [[Sohom Chakrabarty]]
> **From:** [[Indian Institute of Technology Roorkee]]
> **Published:** arXiv preprint · Nov 2025 · cs.RO
> **Keywords:** morphology optimization, Yoshikawa manipulability, 2R planar arm, contextual bandit, SAC, DDPG, PPO, PSO, BO, CMA-ES
> **Links:** [arXiv](https://arxiv.org/abs/2511.12650) · [PDF](https://arxiv.org/pdf/2511.12650v1)
> **One line:** link lengths and elbow angle of a 2R planar arm chosen by RL as a one-step bandit to maximize manipulability along circle, ellipse and rectangle paths

## Summary

The paper checks if RL can find the morphology of a planar 2R manipulator from reward alone. For a circle centered at the base the answer is known (L1 = L2 = R/√2, θ2 = 90°), and SAC, DDPG, PPO, grid search, PSO, BO and CMA-ES all recover it. Then for an ellipse and a rectangle, where there is no closed form, they let RL pick (L1, L2, θ2) with a hybrid reward: normalized manipulability over 720 path points, minus penalties for unreachable points, total link length, and mismatch between the reachable annulus and the task radial band.

DDPG learns fastest but varies across seeds, SAC is slower and more stable, PPO is the least sample-efficient and gives over-extended arms. Honestly this is a very small problem (2-3 design variables, pure kinematics, no dynamics, no control), so calling RL "scalable" here is not demonstrated, and the "policy" is really a bandit over a fixed task. Still, it is a clear and simple example of morphology as an RL action.

## How it works

- Morphology as action of a contextual bandit: state = task path descriptor, action = (L1, L2, θ2) or a single angle φ for the circle
- Reward computed by sampling 720 points, checking reachability with elbow-up IK, averaging manipulability |L1 L2 sin θ2|
- Penalties: unreachable fraction, total link length, inner and outer band violation of [L1−L2, L1+L2] vs the task band
- Four rewards: raw, normalized, band-only and hybrid (hybrid used for ellipse and rectangle)
- Baselines on the circle: grid sweep, PSO (3630 evals), BO with GP+EI (45 evals), CMA-ES (720 evals)
- 5 seeds, 5000 episodes per algorithm

## Results that matter

- Circle: all methods reach φ ≈ 45° and normalized manipulability ≈ 1, BO slightly off (45.5°)
- Ellipse: L1 ≈ 0.26 to 0.43 and L2 ≈ 0.20 to 0.35 depending on algorithm, DDPG closest to the band
- Rectangle: similar pattern, SAC most consistent, PPO lowest reward
- No comparison of heuristics on the non-analytical tasks, so the claim that RL is better there is not tested

## Interesting stuff

- The band penalty (match the reachable annulus to the task radial band) is a simple and nice geometric reward term
- RL as a bandit is basically black-box optimization with a neural sampler, so BO with 45 evaluations is a strong competitor that they do not push further
- No dynamics, torque, or collision, just kinematics
- The future work list (higher DoF, dynamics, Pareto fronts, sim-to-real) is basically everything that would make it relevant

## Useful for me

- **As prior work.** Minor reference for "morphology as an RL action" in manipulators, only for completeness.
- **Apply.** The reachable-band reward is a simple way to score arm lengths of a humanoid for a reaching task before training a full policy.
- **Watch out.** For low-dimensional morphology spaces, BO or CMA-ES may be much more sample efficient than RL. Use RL for morphology only when the body is chosen in context during the task.

## Key prior work

- [[Manipulability of robotic mechanisms|Manipulability]] · the dexterity metric being optimized
- [[Efficient Global Optimization of Expensive Black-Box Functions|EGO]] · the BO baseline
- [[The CMA Evolution Strategy - A Comparing Review|CMA-ES]] · evolutionary baseline
- [[Soft Actor-Critic - Off-Policy Maximum Entropy Deep Reinforcement Learning with a Stochastic Actor|SAC]] · main RL algorithm

## References

1. [[Introduction to Robotics - Mechanics and Control|Craig Introduction to Robotics]] · Craig · 2005 · Pearson · [Scholar](https://scholar.google.com/scholar?q=Craig+Introduction+to+Robotics+Mechanics+and+Control)
2. [[Optimal design of tricept parallel manipulator with particle swarm optimization|Tricept PSO]] · Farooq et al. · 2021 · ISA Transactions · [Scholar](https://scholar.google.com/scholar?q=Optimal+design+of+tricept+parallel+manipulator+with+particle+swarm+optimization)
3. [[Numerical optimal control for nonlinear systems|Numerical optimal control]] · Gros & Zanon · 2020 · Annual Review of Control, Robotics, and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Numerical+optimal+control+for+nonlinear+systems+Gros+Zanon)
4. [[Soft Actor-Critic - Off-Policy Maximum Entropy Deep Reinforcement Learning with a Stochastic Actor|SAC]] · Haarnoja et al. · 2018 · ICML · [Scholar](https://scholar.google.com/scholar?q=Soft+Actor-Critic+Off-Policy+Maximum+Entropy+Deep+Reinforcement+Learning+with+a+Stochastic+Actor)
5. [[The CMA Evolution Strategy - A Comparing Review|CMA-ES]] · Hansen · 2006 · Springer chapter · [Scholar](https://scholar.google.com/scholar?q=The+CMA+evolution+strategy+A+comparing+review)
6. [[Efficient Global Optimization of Expensive Black-Box Functions|EGO]] · Jones et al. · 1998 · Journal of Global Optimization · [Scholar](https://scholar.google.com/scholar?q=Efficient+global+optimization+of+expensive+black-box+functions)
7. [[Particle swarm optimization|PSO]] · Kennedy & Eberhart · 1995 · ICNN · [Scholar](https://scholar.google.com/scholar?q=Kennedy+Eberhart+Particle+swarm+optimization)
8. [[Continuous Control with Deep Reinforcement Learning|DDPG]] · Lillicrap et al. · 2016 · ICLR · [Scholar](https://scholar.google.com/scholar?q=Continuous+control+with+deep+reinforcement+learning)
9. [[Closed-form dynamic equations of planar manipulators with flexible links|Flexible-link planar dynamics]] · De Luca & Siciliano · 1992 · IEEE T-SMC · [Scholar](https://scholar.google.com/scholar?q=Closed-form+dynamic+equations+of+planar+manipulators+with+flexible+links)
10. [[Optimizing the sensory apparatus of voxel-based soft robots through evolution and babbling|Voxel soft robot sensors]] · Medvet et al. · 2021 · GECCO · [DOI](https://doi.org/10.1145/3449639.3459377)
11. [[The application of Bayesian methods for seeking the extremum|Mockus BO]] · Mockus et al. · 1978 · Towards Global Optimization · [Scholar](https://scholar.google.com/scholar?q=The+application+of+bayesian+methods+for+seeking+the+extremum)
12. [[Gaussian Processes for Machine Learning|GPML]] · Rasmussen & Williams · 2006 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Gaussian+Processes+for+Machine+Learning)
13. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
14. [[Optimization of dynamic parameter design of Stewart manipulators using particle swarm optimization|Stewart PSO]] · Shahbazi et al. · 2024 · AIP Conference Proceedings · [Scholar](https://scholar.google.com/scholar?q=Optimization+of+dynamic+parameter+design+of+stewart+manipulators+using+particle+swarm+optimization)
15. [[Robotics - Modelling, Planning and Control|Siciliano Robotics]] · Siciliano et al. · 2010 · Springer · [Scholar](https://scholar.google.com/scholar?q=Robotics+Modelling+Planning+and+Control+Siciliano)
16. [[Optimizing soft robot design and tracking with and without evolutionary computation - An intensive survey|Soft robot optimization survey]] · Stroppa et al. · 2024 · Robotica · [DOI](https://doi.org/10.1017/S0263574724001079)
17. [[Reinforcement Learning - An Introduction|Sutton and Barto]] · Sutton & Barto · 2018 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Sutton+Barto+Reinforcement+Learning+An+Introduction)
18. [[Ellipse and rectangle (MathWorld)|MathWorld]] · Weisstein · n.d. · web · [Link](https://mathworld.wolfram.com/)
19. [[Manipulability of robotic mechanisms|Manipulability]] · Yoshikawa · 1985 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Manipulability+of+robotic+mechanisms)
20. [[Performance comparison of optimization techniques for robot design|Optimization techniques comparison]] · Zhang & Song · 2008 · Mechanism and Machine Theory · [Scholar](https://scholar.google.com/scholar?q=Performance+comparison+of+optimization+techniques+for+robot+design)

## See also

- [[Structural Optimization of Lightweight Bipedal Robot via SERL|SERL]] · RL-in-the-loop morphology optimization with dynamics and hardware
- [[Analysis of Task Feasibility for a Home Robot using Prismatic Joints|PRISM]] · graspability region as a morphology metric for arms
