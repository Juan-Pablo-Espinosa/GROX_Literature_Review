---
title: "DecARt Leg: Design and Evaluation of a Novel Humanoid Robot Leg with Decoupled Actuation for Agile Locomotion"
aliases:
  - DecARt Leg
  - FAST metric
authors:
  - "[[Egor Davydenko]]"
  - "[[Andrei Volchenkov]]"
  - "[[Vladimir Gerasimov]]"
  - "[[Roman Gorbachev]]"
corresponding: ""
institutions:
  - "[[Moscow Institute of Physics and Technology]]"
labs:
  - "[[MIPT Laboratory of Wave Processes and Control Systems]]"
countries:
  - Russia
year: 2025
date: 2025-10-19
venue: IEEE/RSJ IROS 2025
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1109/IROS60139.2025.11247361
pdf: https://doi.org/10.1109/IROS60139.2025.11247361
code: https://github.com/egordv/the_FAST_metric
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/leg-design
  - topic/hardware-design
  - topic/legged-locomotion
  - topic/parallel-mechanisms
  - topic/variable-length-limbs
  - topic/benchmark
  - method/reinforcement-learning
  - method/mpc
  - sim/pybullet
  - sim/mujoco
  - robot/cassie
  - robot/unitree-g1
  - eval/real-robot
---

# DecARt Leg

> [!info] Quick view
> **Authors:** [[Egor Davydenko]], [[Andrei Volchenkov]], [[Vladimir Gerasimov]], [[Roman Gorbachev]]
> **From:** [[Moscow Institute of Physics and Technology]] · [[MIPT Laboratory of Wave Processes and Control Systems]]
> **Published:** IEEE/RSJ IROS 2025 · Oct 2025 · conference
> **Keywords:** decoupled actuation, quasi-telescopic leg, forward-facing knee, multi-bar ankle, FAST metric, swing time, leg design comparison
> **Links:** [DOI](https://doi.org/10.1109/IROS60139.2025.11247361) · [Code (FAST metric)](https://github.com/egordv/the_FAST_metric)
> **One line:** a human-looking leg that internally works like a telescope (one motor swings, one motor sets length) with all motors above the knee, plus a new metric, the fastest achievable swing time

## Summary

Cassie and Digit show that decoupled legs, where one actuator controls swing and another controls leg length, are great for agile locomotion, but they look like birds. Most humanoids (G1, H1, GR1, T1) use a coupled serial leg. DecARt tries to have both: a pantograph-like quasi-telescopic structure with a pair of passive gears at the knee and a 4-bar, so the leg length motor moves the foot straight up and down without the pitch motor, a forward facing knee so it looks human, and a multi-bar linkage so even the 2-DoF ankle motors sit above the knee. Inverse kinematics is analytic, and in simulators it only needs a joint mimic constraint.

To compare leg designs independent of upper body and controller, they define FAST, the shortest swing time a fixed-base leg can do with TSID respecting actuator limits. DecARt gets 0.17 s vs 0.24 s Cassie, 0.24 s GR1T2, 0.34 s G1 and 0.33 s T1. The same leg modified into a serial version gets 0.25 s, so decoupling alone is a big part of the gain. Simulation shows 2.2 m/s walking with ALIP and TSID, and a first hardware biped walks with IK, WBC and an RL controller, but the hardware part is preliminary with no numbers. Honestly the metric is useful but very dependent on the leg pitch motor, as they admit.

## How it works

- Quasi-telescopic structure: geared knee plus 4-bar parallel links, leg length motor rotation becomes vertical foot motion
- Multi-bar ankle transmission with front and rear rods that are active in different leg poses, so the ankle works fully stretched and fully crouched
- Analytic IK: j1, j2 from foot position and height, j3 from leg length, ankle angles compensate j1 to j3, ankle pitch is not fully decoupled (like Cassie)
- FAST: cubic spline swing trajectories of decreasing duration (from OCS2), tracked with TSID under torque and speed limits, integrated with Pinocchio, accepted if final error < 3% of swing length and velocity error small
- Swing length scaled to each robot by tilting the leg ±30°, fixed base, legs at 0.98 of full extension
- Sim robot about 35 kg with torso and arms, PyBullet, ALIP-based planner plus TSID whole body controller
- Hardware: two legs with a pelvis, controllers are IK position control, ALIP+WBC torque control, and an RL policy following Li et al. 2024

## Results that matter

- FAST: DecARt 0.17 s (theoretical 4.18 m/s), Cassie 0.24 s (3.94 m/s), GR1T2 0.24 s (2.92 m/s), G1 0.34 s (1.87 m/s), T1 0.33 s (1.55 m/s)
- DecARt converted to a serial leg with the same motors and masses: 0.25 s, so decoupling gives about 30% faster swing
- Robots with the knee motor at the knee are all slower than those with motors near the hip
- Simulation: 2.2 m/s flat, 0.8 m/s on 5 cm rough terrain, push recovery 95 N sagittal and 50 N lateral for 200 ms, 10 cm stairs
- Hardware: untethered walking, squatting, semi-rough terrain and jumping shown, no quantitative results yet

## Interesting stuff

- The serial vs decoupled ablation on the same leg is the most convincing result, it isolates the kinematic structure from motor choice
- FAST ignores stance, upper body mass, and impact, and can be gamed with a huge pitch motor, so it is a partial metric
- Being able to fully stretch and fully crouch avoids the singularity issues of pantographs, useful for squatting and climbing
- Hardware is preliminary, no RL vs WBC comparison, no energy data
- A telescopic leg hidden inside a human-like appearance is a clever compromise for humanoids that need to look friendly

## Useful for me

- **As prior work.** Very relevant: the leg length is a directly actuated DoF. That is basically a fast, dynamic version of changing leg length during locomotion, even if the range is set by the knee geometry.
- **Apply.** FAST is a simple, open-source metric to compare leg morphologies in simulation. It could compare different limb length configurations of the same humanoid.
- **Apply.** The serial vs decoupled conversion done on the URDF is a good template for ablating a morphology change while keeping masses the same.
- **Watch out.** Ankle pitch depends on the leg length motor, so the policy has to coordinate them. If the robot changes its limb proportions, this coupling changes too.

## Key prior work

- [[ATRIAS - Design and validation of a tether-free 3D-capable spring-mass bipedal robot|ATRIAS]] · the origin of the pantograph quasi-telescopic leg
- [[Coupled and decoupled actuation of robotic mechanisms|Coupled vs decoupled actuation]] · the concept the whole design is built on
- [[Tello Leg - The Study of Design Principles and Metrics for Dynamic Humanoid Robots|Tello Leg]] · another leg design with its own metrics
- [[Comparative metrics of advanced serial-parallel biped design and characterization of the main contemporary architectures|Serial-parallel biped metrics]] · the other recent leg comparison metrics
- [[Passive-dynamic leg design for agile robots|Passive-dynamic leg design]] · why motors should be near the leg root
- [[Design and Control of SLIDER - An Ultra-lightweight, Knee-less, Low-cost Bipedal Walking Robot|SLIDER]] · pure telescopic leg biped
- [[Reinforcement Learning for Versatile, Dynamic, and Robust Bipedal Locomotion Control|Versatile bipedal RL]] · the RL controller they used on hardware

## References

1. [[Coupled and decoupled actuation of robotic mechanisms|Coupled vs decoupled actuation]] · Hirose & Arikawa · 2000 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Coupled+and+decoupled+actuation+of+robotic+mechanisms)
2. [[Feedback control of a Cassie bipedal robot - Walking, standing, and riding a segway|Cassie feedback control]] · Gong et al. · 2019 · ACC · [Scholar](https://scholar.google.com/scholar?q=Feedback+control+of+a+Cassie+bipedal+robot+Walking+standing+and+riding+a+segway)
3. [[Dynamic walking with compliance on a Cassie bipedal robot|Cassie compliant walking]] · Reher et al. · 2019 · ECC · [Scholar](https://scholar.google.com/scholar?q=Dynamic+walking+with+compliance+on+a+Cassie+bipedal+robot)
4. [[Optimizing bipedal locomotion for the 100m dash with comparison to human running|Cassie 100m dash]] · Crowley et al. · 2023 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Optimizing+bipedal+locomotion+for+the+100m+dash+with+comparison+to+human+running)
5. [[Experiments in balance with a 3D one-legged hopping machine|Raibert 3D hopper]] · Raibert et al. · 1984 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Experiments+in+balance+with+a+3d+one-legged+hopping+machine)
6. [[Real-time 3D walking pattern generation for a biped robot with telescopic legs|Meltran V]] · Kajita et al. · 2001 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Real-time+3d+walking+pattern+generation+for+a+biped+robot+with+telescopic+legs)
7. [[The ARL monopod II running robot - control and energetics|ARL Monopod II]] · Ahmadi & Buehler · 1999 · ICRA · [Scholar](https://scholar.google.com/scholar?q=The+ARL+monopod+II+running+robot+control+and+energetics)
8. [[Design and Control of SLIDER - An Ultra-lightweight, Knee-less, Low-cost Bipedal Walking Robot|SLIDER]] · Wang et al. · 2020 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+and+control+of+slider+An+ultra-lightweight+knee-less+low-cost+bipedal+walking+robot)
9. [[High Dynamic Bipedal Robot with Underactuated Telescopic Straight Legs|Telescopic straight legs biped]] · Mou et al. · 2024 · Mathematics · [DOI](https://doi.org/10.3390/math12040600)
10. [[On the hardware design and control architecture of the humanoid robot Kangaroo|Kangaroo]] · Roig et al. · 2022 · ICRA Workshop on Legged Robots · [Scholar](https://scholar.google.com/scholar?q=On+the+Hardware+Design+and+Control+Architecture+of+the+Humanoid+Robot+Kangaroo)
11. [[ATRIAS - Design and validation of a tether-free 3D-capable spring-mass bipedal robot|ATRIAS]] · Hubicki et al. · 2016 · IJRR · [Scholar](https://scholar.google.com/scholar?q=ATRIAS+Design+and+validation+of+a+tether-free+3d-capable+spring-mass+bipedal+robot)
12. [[Dynamic multi-domain bipedal walking with ATRIAS through SLIP based human-inspired control|ATRIAS multi-domain walking]] · Hereid et al. · 2014 · HSCC · [Scholar](https://scholar.google.com/scholar?q=Dynamic+multi-domain+bipedal+walking+with+ATRIAS+through+SLIP+based+human-inspired+control)
13. [[Passive-dynamic leg design for agile robots|Passive-dynamic leg design]] · Abate et al. · 2015 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Passive-dynamic+leg+design+for+agile+robots)
14. [[Proprioceptive actuator design in the MIT Cheetah - Impact mitigation and high-bandwidth physical interaction for dynamic legged robots|MIT Cheetah actuator]] · Wensing et al. · 2017 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Proprioceptive+actuator+design+in+the+MIT+Cheetah+Impact+mitigation+and+high-bandwidth+physical+interaction+for+dynamic+legged+robots)
15. [[Versatile locomotion planning and control for humanoid robots|Versatile humanoid locomotion]] · Ahn et al. · 2021 · Frontiers in Robotics and AI · [Scholar](https://scholar.google.com/scholar?q=Versatile+locomotion+planning+and+control+for+humanoid+robots)
16. [[Tello Leg - The Study of Design Principles and Metrics for Dynamic Humanoid Robots|Tello Leg]] · Sim & Ramos · 2022 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Tello+leg+The+study+of+design+principles+and+metrics+for+dynamic+humanoid+robots)
17. [[Comparative metrics of advanced serial-parallel biped design and characterization of the main contemporary architectures|Serial-parallel biped metrics]] · Batto et al. · 2023 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Comparative+metrics+of+advanced+serial+parallel+biped+design+and+characterization+of+the+main+contemporary+architectures)
18. [[Exploiting inherent robustness and natural dynamics in the control of bipedal walking robots|Pratt thesis]] · Pratt · 2000 · PhD thesis, MIT · [Scholar](https://scholar.google.com/scholar?q=Exploiting+inherent+robustness+and+natural+dynamics+in+the+control+of+bipedal+walking+robots)
19. [[Sequential linear quadratic optimal control for nonlinear switched systems|SLQ switched systems]] · Farshidian et al. · 2017 · IFAC World Congress · [Scholar](https://scholar.google.com/scholar?q=Sequential+linear+quadratic+optimal+control+for+nonlinear+switched+systems)
20. [[Implementing torque control with high-ratio gear boxes and without joint-torque sensors|TSID torque control]] · Del Prete et al. · 2015 · IJHR · [Scholar](https://scholar.google.com/scholar?q=Implementing+torque+control+with+high-ratio+gear+boxes+and+without+joint-torque+sensors)
21. [[The Pinocchio C++ library - A fast and flexible implementation of rigid body dynamics algorithms and their analytical derivatives|Pinocchio]] · Carpentier et al. · 2019 · SII · [GitHub](https://github.com/stack-of-tasks/pinocchio)
22. [[One-step ahead prediction of angular momentum about the contact point for control of bipedal locomotion - Validation in a LIP-inspired controller|ALIP controller]] · Gong & Grizzle · 2021 · ICRA · [Scholar](https://scholar.google.com/scholar?q=One-step+ahead+prediction+of+angular+momentum+about+the+contact+point+for+control+of+bipedal+locomotion)
23. [[Reinforcement Learning for Versatile, Dynamic, and Robust Bipedal Locomotion Control|Versatile bipedal RL]] · Li et al. · 2024 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Reinforcement+learning+for+versatile+dynamic+and+robust+bipedal+locomotion+control)

## See also

- [[Design, Control, and Validation of a Symmetrical Hip and Straight-Legged Vertically-Compliant Bipedal Robot|Symmetrical hip straight-leg biped]] · another telescopic-style biped leg
