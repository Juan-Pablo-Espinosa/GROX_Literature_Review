---
title: "Control of Humanoid Robots with Parallel Mechanisms using Differential Actuation Models"
aliases:
  - Differential Actuation Models
authors:
  - "[[Victor Lutz]]"
  - "[[Ludovic De Matteïs]]"
  - "[[Virgile Batto]]"
  - "[[Nicolas Mansard]]"
corresponding: "[[Victor Lutz]]"
institutions:
  - "[[LAAS-CNRS]]"
  - "[[Université de Toulouse]]"
  - "[[Inria]]"
  - "[[Artificial and Natural Intelligence Toulouse Institute]]"
labs:
  - "[[Gepetto]]"
  - "[[Auctus]]"
countries:
  - France
year: 2025
date: 2025-10-08
venue: arXiv preprint
venue_type: preprint
arxiv_category: cs.RO
arxiv_id: "2503.22459"
url: https://arxiv.org/abs/2503.22459
pdf: https://arxiv.org/pdf/2503.22459v2
code: "Announced as open source in the paper, no repo link printed (checked 2026-11-02)"
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/whole-body-control
  - topic/legged-locomotion
  - topic/parallel-mechanisms
  - topic/sim-to-real
  - method/reinforcement-learning
  - method/ppo
  - method/trajectory-optimization
  - method/mpc
  - sim/isaac-lab
  - sim/mujoco
  - robot/bipetto
  - eval/real-robot
---

# Differential Actuation Models

> [!info] Quick view
> **Authors:** [[Victor Lutz]] (corresponding), [[Ludovic De Matteïs]], [[Virgile Batto]], [[Nicolas Mansard]]
> **From:** [[LAAS-CNRS]] · [[Gepetto]] · [[Université de Toulouse]] · [[Inria]] · [[Auctus]] · [[Artificial and Natural Intelligence Toulouse Institute]]
> **Published:** arXiv preprint · Mar 2025 (v2 Oct 2025) · cs.RO
> **Keywords:** parallel actuation, four-bar knee, intricate four-bar ankle, actuation Jacobian, impedance transfer, trajectory optimization, Crocoddyl, RL deployment
> **Links:** [arXiv](https://arxiv.org/abs/2503.22459) · [PDF](https://arxiv.org/pdf/2503.22459v2)
> **One line:** a compact, twice differentiable model of the four-bar knee and two-four-bar ankle that lets TO use motor-space limits and lets an RL policy trained on a serial model run on a parallel-actuated biped

## Summary

Almost every new humanoid (G1, H1, Digit, T1, GR1) has knees or ankles driven through linkages, but controllers and RL policies are trained in serial joint space, usually assuming a constant reduction ratio. This paper writes the exact nonlinear transmission of the two standard mechanisms (planar four-bar for the knee, two projected four-bars for the 2-DoF ankle) in closed form, with first and second order derivatives. With that, they get the actuation Jacobian that maps motor torque to joint torque, the derivatives needed for FDDP trajectory optimization, and a way to transfer joint-space PD gains to motor-space gains.

In TO (Crocoddyl), walking on flat ground gives the same result as a naive serial model, but on rough terrain and stairs the ankle needs configurations that no joint-space box limit can describe, and motor-space limits handle it. In RL (Isaac Lab with CaT), the policy is trained on a plain serial model, and at deployment the serial PD is converted to motor stiffness and damping at 100 Hz. This makes omnidirectional walking work on their Bipetto robot, while sending only the converted torques without gains made the robot fall. Honestly the RL part is only flat ground and the ankle limits are not used in RL, but the impedance transfer trick is clean and cheap.

## How it works

- Knee: law of cosines on the four-bar gives qm = atan2(yB, xB) + acos(r(b)), with b from the serial forward kinematics
- Ankle: each side of the two-linkage ankle is projected into a plane orthogonal to its motor, giving two virtual four-bars and a 2x2 actuation Jacobian
- Second order terms (dτs/dqs) derived analytically with Pinocchio spatial algebra, because autodiff gave bloated expressions
- Impedance transfer: motor gains KPm = A + J⁻ᵀ KPs J⁻¹ + C, keeping the A and C terms that earlier work dropped because in RL the error qs* − qs is not small
- State estimation: qs from motor encoders by a warm-started least squares inverse of f
- TO: multiple shooting FDDP with serial dynamics but motor torques as controls, motor-space joint limits as constraints
- RL: CaT constrained PPO in Isaac Lab, 4096 envs, serial joint position offsets as actions, 6-step history, deployed at 30 Hz with gains updated at 100 Hz and a 1 kHz motor loop on a Raspberry Pi 5

## Results that matter

- Flat walking at 0.7 m/s: actuated serial model and minimal serial model give similar results
- Rough terrain and stairs: required ankle configurations fall outside any serial box that is also feasible for the motors, motor-space limits fix that
- Transmission ratio varies a lot near singularities (knee fully bent or stretched), so constant ratio is wrong there
- Real robot omnidirectional walking with the converted gains, while feeding the converted torques without feedback gains made the robot fall
- Conversion plus state estimation under 10 ms in pure Python

## Interesting stuff

- The main message is practical: you can keep training RL on the simple serial model if you convert impedance correctly at deployment, no need to simulate closed chains on GPU
- Policy validated in MuJoCo with the full closed-loop model before hardware, a good sim-to-sim habit
- Only flat ground RL on hardware, no quantitative tracking comparison against a constant ratio baseline on the real robot
- They release f and J_A on GPU through CusADi so the ankle limits could go into RL later
- Nice argument that hidden vendor conversion layers make it hard to know what commercial robots actually do

## Useful for me

- **Apply.** If our humanoid uses linkage-driven knees or ankles, train in serial space and do the gain transfer at deployment. It is cheap and avoids closed chains in the simulator.
- **Watch out.** When link lengths change, the transmission ratio of any linkage changes too. The actuation Jacobian is a function of the geometry, so it must be recomputed for every body configuration, and the reachable joint ranges change with it.
- **Apply.** Put joint limits in motor space instead of a fixed box in joint space, especially for ankles, to not lose feasible configurations.
- **As prior work.** Good reference for sim-to-real with parallel actuation and for why a constant reduction ratio is a weak assumption.

## Key prior work

- [[An Analytical and Modular Software Workbench for Solving Kinematics and Dynamics of Series-Parallel Hybrid Robots|Series-parallel workbench]] · the analytical transmission model they build on
- [[A fail-safe semi-centralized impedance controller - Validation on a parallel kinematics ankle|Fail-safe impedance ankle]] · early impedance transfer that dropped the extra terms
- [[Optimal Control of Walkers with Parallel Actuation|Optimal control with parallel actuation]] · their group's full closed-loop TO
- [[Robust RL Control for Bipedal Locomotion with Closed Kinematic Chains|Closed-chain RL]] · the alternative of training RL with closed chains in sim
- [[LiPS - Large-Scale Humanoid Robot Reinforcement Learning with Parallel-Series Structures|LiPS]] · RL with serial model and actuator-space outputs
- [[CaT - Constraints as Terminations for Legged Locomotion Reinforcement Learning|CaT]] · the constrained PPO used for training
- [[Crocoddyl - An Efficient and Versatile Framework for Multi-Contact Optimal Control|Crocoddyl]] · the TO solver

## References

1. [[Comparative metrics of advanced serial-parallel biped design and characterization of the main contemporary architectures|Serial-parallel biped metrics]] · Batto et al. · 2023 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Comparative+metrics+of+advanced+serial+parallel+biped+design+and+characterization+of+the+main+contemporary+architectures)
2. [[Whole-Body Control of Series-Parallel Hybrid Robots|Series-parallel WBC]] · Mronga et al. · 2022 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Whole-Body+Control+of+Series-Parallel+Hybrid+Robots)
3. [[Investigations into Exploiting the Full Capabilities of a Series-Parallel Hybrid Humanoid Using Whole Body Trajectory Optimization|Full-capability TO]] · Boukheddimi et al. · 2023 · IROS · [Scholar](https://scholar.google.com/scholar?q=Investigations+into+Exploiting+the+Full+Capabilities+of+a+Series-Parallel+Hybrid+Humanoid+Using+Whole+Body+Trajectory+Optimization)
4. [[Rigid Body Dynamics Algorithms|Rigid Body Dynamics Algorithms]] · Featherstone · 2008 · Springer book · [Scholar](https://scholar.google.com/scholar?q=Featherstone+Rigid+Body+Dynamics+Algorithms)
5. [[Proximal and Sparse Resolution of Constrained Dynamic Equations|Proximal constrained dynamics]] · Carpentier et al. · 2021 · RSS · [Scholar](https://scholar.google.com/scholar?q=Proximal+and+Sparse+Resolution+of+Constrained+Dynamic+Equations)
6. [[Model Simplification For Dynamic Control of Series-Parallel Hybrid Robots - A Representative Study on the Effects of Neglected Dynamics|Model simplification study]] · Kumar et al. · 2019 · IROS · [Scholar](https://scholar.google.com/scholar?q=Model+Simplification+For+Dynamic+Control+of+Series-Parallel+Hybrid+Robots)
7. [[Optimal Control of Walkers with Parallel Actuation|Optimal control with parallel actuation]] · De Matteïs et al. · 2024 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Optimal+Control+of+Walkers+with+Parallel+Actuation)
8. [[Constrained articulated body dynamics algorithms|Constrained ABA]] · Sathya & Carpentier · 2025 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Constrained+articulated+body+dynamics+algorithms)
9. [[An Analytical and Modular Software Workbench for Solving Kinematics and Dynamics of Series-Parallel Hybrid Robots|Series-parallel workbench]] · Kumar & Mueller · 2019 · arXiv / ASME JMR · [Scholar](https://scholar.google.com/scholar?q=An+Analytical+and+Modular+Software+Workbench+for+Solving+Kinematics+and+Dynamics+of+Series-Parallel+Hybrid+Robots)
10. [[On the Comprehensive Kinematics Analysis of a Humanoid Parallel Ankle Mechanism|Parallel ankle kinematics]] · Zhou & Tsagarakis · 2018 · ASME JMR · [Scholar](https://scholar.google.com/scholar?q=On+the+Comprehensive+Kinematics+Analysis+of+a+Humanoid+Parallel+Ankle+Mechanism)
11. [[Modeling and Numerical Analysis of Kangaroo Lower Body based on Constrained Dynamics of Hybrid Serial-Parallel Floating-Base Systems|Kangaroo constrained dynamics]] · Hoffman et al. · 2024 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Modeling+and+Numerical+Analysis+of+Kangaroo+Lower+Body+based+on+Constrained+Dynamics+of+Hybrid+Serial-Parallel+Floating-Base+Systems)
12. [[Reduced-dimensional whole-body control based on model simplification for bipedal robots with parallel mechanisms|Reduced-dimensional WBC]] · Liang et al. · 2025 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Reduced-Dimensional+Whole-Body+Control+Based+on+Model+Simplification+for+Bipedal+Robots+With+Parallel+Mechanisms)
13. [[Robust RL Control for Bipedal Locomotion with Closed Kinematic Chains|Closed-chain RL]] · Maslennikov et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Robust+RL+Control+for+Bipedal+Locomotion+with+Closed+Kinematic+Chains)
14. [[Berkeley Humanoid - A Research Platform for Learning-based Control|Berkeley Humanoid]] · Liao et al. · 2025 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Berkeley+humanoid+A+research+platform+for+learning-based+control)
15. [[Reinforcement Learning for Versatile, Dynamic, and Robust Bipedal Locomotion Control|Versatile bipedal RL]] · Li et al. · 2025 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Reinforcement+learning+for+versatile+dynamic+and+robust+bipedal+locomotion+control)
16. [[Mechanical Intelligence-Aware Curriculum Reinforcement Learning for Humanoids with Parallel Actuation|MI-aware curriculum RL]] · Tanaka et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Mechanical+Intelligence-Aware+Curriculum+Reinforcement+Learning+for+Humanoids+with+Parallel+Actuation)
17. [[Learning to Walk with Hybrid Serial-Parallel Linkages - a Case Study on the Kangaroo Robot|Kangaroo RL]] · Amadio et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Learning+to+Walk+with+Hybrid+Serial-Parallel+Linkages+a+Case+Study+on+the+Kangaroo+Robot)
18. [[Contact Models in Robotics - a Comparative Analysis|Contact models analysis]] · Le Lidec et al. · 2024 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Contact+Models+in+Robotics+a+Comparative+Analysis)
19. [[LiPS - Large-Scale Humanoid Robot Reinforcement Learning with Parallel-Series Structures|LiPS]] · Zhang et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=LiPS+Large-Scale+Humanoid+Robot+Reinforcement+Learning+with+Parallel-Series+Structures)
20. [[A fail-safe semi-centralized impedance controller - Validation on a parallel kinematics ankle|Fail-safe impedance ankle]] · Ruscelli et al. · 2018 · IROS · [Scholar](https://scholar.google.com/scholar?q=A+fail-safe+semi-centralized+impedance+controller+Validation+on+a+parallel+kinematics+ankle)
21. [[Unitree H1|Unitree H1]] · Unitree · 2025 · product page · [Link](https://www.unitree.com/h1)
22. [[TALOS - A new humanoid research platform targeted for industrial applications|TALOS]] · Stasse et al. · 2017 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=TALOS+A+new+humanoid+research+platform+targeted+for+industrial+applications)
23. [[Analytical Derivatives of Rigid Body Dynamics Algorithms|Analytical RBD derivatives]] · Carpentier & Mansard · 2018 · RSS · [Scholar](https://scholar.google.com/scholar?q=Analytical+Derivatives+of+Rigid+Body+Dynamics+Algorithms)
24. [[The Pinocchio C++ library - A fast and flexible implementation of rigid body dynamics algorithms and their analytical derivatives|Pinocchio]] · Carpentier et al. · 2019 · SII · [GitHub](https://github.com/stack-of-tasks/pinocchio)
25. [[Full-Order Sampling-Based MPC for Torque-Level Locomotion Control via Diffusion-Style Annealing|DIAL-MPC]] · Xue et al. · 2025 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Full-order+sampling-based+mpc+for+torque-level+locomotion+control+via+diffusion-style+annealing)
26. [[Fast direct multiple shooting algorithms for optimal robot control|Direct multiple shooting]] · Diehl et al. · 2006 · Springer chapter · [Scholar](https://scholar.google.com/scholar?q=Fast+direct+multiple+shooting+algorithms+for+optimal+robot+control)
27. [[Crocoddyl - An Efficient and Versatile Framework for Multi-Contact Optimal Control|Crocoddyl]] · Mastalli et al. · 2020 · ICRA · [GitHub](https://github.com/loco-3d/crocoddyl)
28. [[Whole-Body Model Predictive Control for Biped Locomotion on a Torque-Controlled Humanoid Robot|WB-MPC on TALOS]] · Dantec et al. · 2022 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Whole-Body+Model+Predictive+Control+for+Biped+Locomotion+on+a+Torque-Controlled+Humanoid+Robot)
29. [[CaT - Constraints as Terminations for Legged Locomotion Reinforcement Learning|CaT]] · Chane-Sane et al. · 2024 · arXiv · [Scholar](https://scholar.google.com/scholar?q=CaT+Constraints+as+Terminations+for+Legged+Locomotion+Reinforcement+Learning)
30. [[Constrained reinforcement learning for unstable point-feet bipedal locomotion applied to the Bolt robot|Bolt constrained RL]] · Roux et al. · 2025 · arXiv · [Scholar](https://scholar.google.com/scholar?q=Constrained+reinforcement+learning+for+unstable+point-feet+bipedal+locomotion+applied+to+the+bolt+robot)
31. [[MuJoCo - A physics engine for model-based control|MuJoCo]] · Todorov et al. · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=MuJoCo+A+physics+engine+for+model-based+control)
32. [[CusADi - A GPU Parallelization Framework for Symbolic Expressions and Optimal Control|CusADi]] · Jeon et al. · 2024 · arXiv · [Scholar](https://scholar.google.com/scholar?q=CusADi+A+GPU+Parallelization+Framework+for+Symbolic+Expressions+and+Optimal+Control)

## See also

- [[A Framework for Optimal Ankle Design of Humanoid Robots|Optimal Ankle Design Framework]] · the design side of the same parallel ankle problem
