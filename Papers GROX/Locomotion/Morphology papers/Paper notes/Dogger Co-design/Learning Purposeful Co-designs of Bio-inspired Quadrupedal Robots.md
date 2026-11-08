---
title: "Learning Purposeful Co-designs of Bio-inspired Quadrupedal Robots"
aliases:
  - Dogger
  - Dogger Co-design
authors:
  - "[[Fabio Boekel]]"
  - "[[Jiatao Ding]]"
  - "[[Cosimo Della Santina]]"
corresponding: "[[Jiatao Ding]]"
institutions:
  - "[[Delft University of Technology]]"
  - "[[Beijing Zhongguancun Academy]]"
  - "[[German Aerospace Center]]"
labs:
  - "[[DLR Institute of Robotics and Mechatronics]]"
countries:
  - Netherlands
  - China
  - Germany
year: 2026
date: 2026-11-02
venue: arXiv preprint
venue_type: preprint
arxiv_category: cs.RO
arxiv_id: "2611.02462"
url: https://arxiv.org/abs/2611.02462
pdf: https://arxiv.org/pdf/2611.02462v1
code: "No official repo linked in the paper (checked 2026-11-03)"
status: to-read
relevance: high
added: 2026-11-03
tags:
  - paper
  - topic/co-design
  - topic/morphology
  - topic/legged-locomotion
  - topic/leg-design
  - topic/variable-stiffness
  - topic/hardware-design
  - topic/sim-to-real
  - topic/domain-randomization
  - method/reinforcement-learning
  - method/ppo
  - method/bayesian-optimization
  - sim/brax
  - sim/isaac-lab
  - eval/real-robot
---

# Dogger Co-design

> [!info] Quick view
> **Authors:** [[Fabio Boekel]], [[Jiatao Ding]] (corresponding), [[Cosimo Della Santina]]
> **From:** [[Delft University of Technology]] · [[Beijing Zhongguancun Academy]] · [[German Aerospace Center]] · [[DLR Institute of Robotics and Mechatronics]]
> **Published:** arXiv preprint · Nov 2026 · cs.RO
> **Keywords:** morphology-control co-design, morphology-conditioned policy, Bayesian optimization, parallel elastic actuator, three-segment leg, cost of transport, modular quadruped
> **Links:** [arXiv](https://arxiv.org/abs/2611.02462) · [PDF](https://arxiv.org/pdf/2611.02462v1)
> **One line:** one PPO policy trained across many leg lengths and knee-spring settings is frozen and used to pick a task-specific design with BO, and the picked designs cut real cost of transport by 18.6% and 35.7%.

## Summary

Most co-design papers either stay in simulation or only search a small piece of the design, like the knee spring in the ANYmal PEA paper from Bjelonic et al. This paper builds Dogger, a cheap 6.5 kg quadruped with 3D printed three-segment legs (four-bar linkage) and a knee parallel elastic actuator with a wire over a swappable wheel. That gives five design parameters: three link scale factors from 0.7 to 1.3, the wheel diameter (stiffness) and the spring offset (engagement angle). Changing a design is only reprinting parts and moving a clamp.

The method is two steps. First a single PPO policy with the design vector in its observation is trained across the whole design space. Then the policy is frozen and a TPE-based BO searches the design that minimizes cost of transport for a task. For inspection walking the chosen design cuts real CoT by 18.6%, and for carrying a 2 kg payload by 35.7%, both zero-shot with no retraining or distillation. Joint geometry plus compliance beats searching only one of them. Honestly it's one of the most complete real-hardware co-design papers I've seen, but the policy is never re-optimized for the chosen design, and a simple CPG + CMA-ES baseline actually wins on the inspection task.

## How it works

- Design d = [λ1, λ2, λ3, wheel diameter, spring offset], link lengths are scale factors on nominal thigh, shank and foot lengths
- Knee PEA: spring in the thigh, wire over a wheel at the knee, the wheel diameter sets rotational stiffness (0.03 to 2.62 Nm/rad) and clamps on the wire set where it engages
- Policy input is d plus commands, gravity vector, yaw rate, joint positions and previous action. Output is joint offsets from a standing pose, tracked by the motor PD at 50 Hz
- The reward avoids terms that favor one design (no base height term) and includes an energy term that matches the CoT objective of the design search
- An analytical feedforward spring torque is added on the knee, so the policy doesn't need to learn to cancel the spring
- Energy model counts motor copper losses plus mechanical power plus a constant electronics power, which is about 50% of the total on the real robot
- PPO in Brax (8192 envs) for flat ground, Isaac Lab for rough terrain, 15 min to 2 h on an RTX 4070. Design search uses Optuna TPE, 100 random warmup designs plus 45 iterations of 20, around 45 min
- Domain randomization on design parameters, friction, PD gains, damping and torso mass, inertia and CoM, but none during design selection so differences come from the design

## Results that matter

- Inspection (straight walking at 0.5 m/s with stops): chosen design λ = (0.86, 1.02, 1.28), wheel 33 mm, offset 6 mm. CoT goes down 12.6% in sim and 18.6% ± 1.7% on hardware vs the nominal springless design
- Payload (2 kg, 16 omnidirectional commands): λ = (1.30, 0.70, 1.30), wheel 38 mm, offset 2 mm. CoT goes down 34.0% in sim and 35.7% ± 3.1% on hardware
- Cross-task: each design still helps on the other task (13.5% and 23.7% on hardware), but less than the matching design, so the optimum depends on the task
- Design-space ablation in sim: for inspection, link lengths only give 1.1% and springs only 7.3%. For payload it flips, link lengths give 24.0% and springs 16.3%. Joint search is best for both
- Against other co-design methods: CPG + CMA-ES gets 4% lower CoT than theirs on inspection, privileged RL + BO (Bjelonic style) is 12.8% worse. On payload theirs wins by about 3%
- Outdoor tests on gravel, grass and forest floor, including a 17 min run without falls, are qualitative only

## Interesting stuff

- The frozen universal policy is both the design evaluator and the deployed controller. No teacher-student and no fine-tuning per design, which keeps sim-to-real simple
- Making stiffness a printed wheel instead of a new spring is a really practical trick for exploring compliance on hardware
- The sim to real numbers line up in ranking but not in absolute CoT, which is the right claim to make and they make it
- Weak points: the design found depends on what the frozen policy can do (they admit it), only five parameters and two flat-ground tasks, four hardware trials per design, and the CPG baseline winning on inspection is a bit uncomfortable for the "learning is needed" message

## Useful for me

- **As prior work.** This is very close to what we need: one policy conditioned on link lengths, trained over a range of 0.7 to 1.3 per segment, that controls every design zero-shot. It's direct evidence that a morphology-conditioned policy can transfer across leg geometries on real hardware.
- **Apply.** The pipeline to choose a body per task (train a universal policy, freeze it, search the design with BO) fits a humanoid that changes its limbs. The search output could even become a lookup of target morphology per task.
- **Apply.** The tricks for a fair comparison of designs are worth copying: no randomization and the same seed during evaluation, a reward that doesn't favor one design, and an energy model that includes the static electronics.
- **Watch out.** The body here is fixed during each run. When limbs change during the task the policy also needs to handle the transition, and that is not tested.
- **Gap.** Quadruped and flat ground only. Nobody shows this for a humanoid or with a morphology that changes online.

## Key prior work

- [[Learning-Based Design and Control for Quadrupedal Robots With Parallel-Elastic Actuators|PEA co-design on ANYmal]] · the closest work, design-conditioned RL + BO but only on knee elasticity and with a distilled student
- [[Meta Reinforcement Learning for Optimal Design of Legged Robots|Meta-RL design]] · meta-RL to evaluate many designs with one policy, the baseline idea they simplify
- [[Quadrupedal Locomotion with Parallel Compliance - E-Go Design, Modeling, and Control|E-Go]] · their own earlier robot with the wheel-wire knee PEA
- [[Embodied intelligence via learning and evolution|DERL]] · learning plus evolution of bodies, the big-picture co-design reference
- [[Jointly learning to construct and control agents using deep reinforcement learning|Construct and control]] · the classic concurrent design-and-control RL paper
- [[Co-Design of a Differential Three-Segment Leg Enhancing Payload and Efficiency in Quadrupeds|Differential three-segment leg]] · task-oriented three-segment leg, closest hardware idea
- [[A quadruped robot designed to complete a marathon on a single battery charge|RAIBO2]] · system-level energy efficiency as the motivation
- [[Completely Derandomized Self-Adaptation in Evolution Strategies|CMA-ES]] · the optimizer behind the CPG baseline that wins on inspection

## References

1. [[Synergy-based robotic quadruped leveraging passivity for natural intelligence and behavioural diversity|Synergy quadruped]] · Stella et al. · 2025 · Nature Machine Intelligence · [Scholar](https://scholar.google.com/scholar?q=Synergy-based+robotic+quadruped+leveraging+passivity+for+natural+intelligence+and+behavioural+diversity)
2. [[Design principles for energy-efficient legged locomotion and implementation on the MIT Cheetah robot|MIT Cheetah energy efficiency]] · Seok et al. · 2014 · IEEE/ASME TMech · [Scholar](https://scholar.google.com/scholar?q=Design+principles+for+energy-efficient+legged+locomotion+and+implementation+on+the+MIT+Cheetah+robot)
3. [[Quadrupedal Locomotion with Parallel Compliance - E-Go Design, Modeling, and Control|E-Go]] · Ding et al. · 2024 · IEEE/ASME TMech · [Scholar](https://scholar.google.com/scholar?q=Quadrupedal+Locomotion+with+Parallel+Compliance+E-Go+Design+Modeling+and+Control)
4. [[Learning to adapt through bio-inspired gait strategies for versatile quadruped locomotion|Bio-inspired gait strategies]] · Humphreys and Zhou · 2025 · Nature Machine Intelligence · [Scholar](https://scholar.google.com/scholar?q=Learning+to+adapt+through+bio-inspired+gait+strategies+for+versatile+quadruped+locomotion)
5. [[Finding Resonance - Adaptive Frequency Oscillators for Dynamic Legged Locomotion|Finding resonance]] · Buchli et al. · 2006 · IROS · [Scholar](https://scholar.google.com/scholar?q=Finding+Resonance+Adaptive+Frequency+Oscillators+for+Dynamic+Legged+Locomotion)
6. [[Towards dynamic trot gait locomotion - Design, control, and experiments with Cheetah-cub, a compliant quadruped robot|Cheetah-cub]] · Spröwitz et al. · 2013 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Towards+dynamic+trot+gait+locomotion+Design+control+and+experiments+with+Cheetah-cub+a+compliant+quadruped+robot)
7. [[Oncilla Robot - A Versatile Open-Source Quadruped Research Robot With Compliant Pantograph Legs|Oncilla]] · Spröwitz et al. · 2018 · Frontiers in Robotics and AI · [Scholar](https://scholar.google.com/scholar?q=Oncilla+Robot+A+Versatile+Open-Source+Quadruped+Research+Robot+With+Compliant+Pantograph+Legs)
8. [[Learning-Based Design and Control for Quadrupedal Robots With Parallel-Elastic Actuators|PEA co-design on ANYmal]] · Bjelonic et al. · 2023 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Learning-Based+Design+and+Control+for+Quadrupedal+Robots+With+Parallel-Elastic+Actuators)
9. [[SpaceBok - A Dynamic Legged Robot for Space Exploration|SpaceBok]] · Arm et al. · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=SpaceBok+A+Dynamic+Legged+Robot+for+Space+Exploration)
10. [[Learning plastic matching of robot dynamics in closed-loop central pattern generators|Plastic CPG matching]] · Ruppert and Badri-Spröwitz · 2022 · Nature Machine Intelligence · [Scholar](https://scholar.google.com/scholar?q=Learning+plastic+matching+of+robot+dynamics+in+closed-loop+central+pattern+generators)
11. [[Learning robust perceptive locomotion for quadrupedal robots in the wild|ANYmal in the wild]] · Miki et al. · 2022 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+robust+perceptive+locomotion+for+quadrupedal+robots+in+the+wild)
12. [[High-speed control and navigation for quadrupedal robots on complex and discrete terrain|Raibo high-speed navigation]] · Kim et al. · 2025 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=High-speed+control+and+navigation+for+quadrupedal+robots+on+complex+and+discrete+terrain)
13. [[Learning quadrupedal locomotion over challenging terrain|ANYmal challenging terrain]] · Lee et al. · 2020 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+quadrupedal+locomotion+over+challenging+terrain)
14. [[Learning quadrupedal locomotion on deformable terrain|Deformable terrain]] · Choi et al. · 2023 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+quadrupedal+locomotion+on+deformable+terrain)
15. [[Learning agile and dynamic motor skills for legged robots|Actuator net]] · Hwangbo et al. · 2019 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+agile+and+dynamic+motor+skills+for+legged+robots)
16. [[Lifelike agility and play in quadrupedal robots using reinforcement learning and generative pre-trained models|Lifelike agility]] · Han et al. · 2024 · Nature Machine Intelligence · [Scholar](https://scholar.google.com/scholar?q=Lifelike+agility+and+play+in+quadrupedal+robots+using+reinforcement+learning+and+generative+pre-trained+models)
17. [[Agile perceptive multi-skill locomotion for quadrupedal robots in the wild|Perceptive multi-skill]] · Kang et al. · 2026 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Agile+perceptive+multi-skill+locomotion+for+quadrupedal+robots+in+the+wild)
18. [[Multi-expert learning of adaptive legged locomotion|MELA]] · Yang et al. · 2020 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Multi-expert+learning+of+adaptive+legged+locomotion)
19. [[BirdBot achieves energy-efficient gait with minimal control using avian-inspired leg clutching|BirdBot]] · Badri-Spröwitz et al. · 2022 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=BirdBot+achieves+energy-efficient+gait+with+minimal+control+using+avian-inspired+leg+clutching)
20. [[A quadruped robot designed to complete a marathon on a single battery charge|RAIBO2]] · Lee et al. · 2026 · Nature · [Scholar](https://scholar.google.com/scholar?q=A+quadruped+robot+designed+to+complete+a+marathon+on+a+single+battery+charge)
21. [[Concurrent Optimization of Mechanical Design and Locomotion Control of a Legged Robot|Concurrent design and control]] · Digumarti et al. · 2014 · Mobile Service Robotics (CLAWAR) · [Scholar](https://scholar.google.com/scholar?q=Concurrent+Optimization+of+Mechanical+Design+and+Locomotion+Control+of+a+Legged+Robot)
22. [[Joint Optimization of Robot Design and Motion Parameters using the Implicit Function Theorem|Implicit function co-design]] · Ha et al. · 2017 · RSS · [Scholar](https://scholar.google.com/scholar?q=Joint+Optimization+of+Robot+Design+and+Motion+Parameters+using+the+Implicit+Function+Theorem)
23. [[Control-Aware Design Optimization for Bio-Inspired Quadruped Robots|Control-aware design]] · De Vincenti et al. · 2021 · IROS · [Scholar](https://scholar.google.com/scholar?q=Control-Aware+Design+Optimization+for+Bio-Inspired+Quadruped+Robots)
24. [[A Versatile Co-Design Approach for Dynamic Legged Robots|Versatile co-design]] · Dinev et al. · 2022 · IROS · [Scholar](https://scholar.google.com/scholar?q=A+Versatile+Co-Design+Approach+for+Dynamic+Legged+Robots)
25. [[Jointly learning to construct and control agents using deep reinforcement learning|Construct and control]] · Schaff et al. · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Jointly+learning+to+construct+and+control+agents+using+deep+reinforcement+learning)
26. [[Reinforcement Learning for Improving Agent Design|Improving agent design]] · Ha · 2019 · Artificial Life · [Scholar](https://scholar.google.com/scholar?q=Reinforcement+Learning+for+Improving+Agent+Design)
27. [[Learning Body Shape Variation in Physics-based Characters|Body shape variation]] · Won and Lee · 2019 · ACM TOG · [Scholar](https://scholar.google.com/scholar?q=Learning+Body+Shape+Variation+in+Physics-based+Characters)
28. [[Data-efficient Co-Adaptation of Morphology and Behaviour with Deep Reinforcement Learning|Co-adaptation]] · Luck et al. · 2020 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Data-efficient+Co-Adaptation+of+Morphology+and+Behaviour+with+Deep+Reinforcement+Learning)
29. [[C2 - Co-design of Robots via Concurrent-Network Coupling Online and Offline Reinforcement Learning|C2]] · Chen et al. · 2023 · IROS · [Scholar](https://scholar.google.com/scholar?q=C2+Co-design+of+Robots+via+Concurrent-Network+Coupling+Online+and+Offline+Reinforcement+Learning)
30. [[Reinforcement Learning and Trajectory Optimization for the Concurrent Design of High-Performance Robotic Systems|Grandesso thesis]] · Grandesso · 2023 · PhD thesis, University of Trento · [Scholar](https://scholar.google.com/scholar?q=Reinforcement+Learning+and+Trajectory+Optimization+for+the+Concurrent+Design+of+High-Performance+Robotic+Systems)
31. [[Embodied intelligence via learning and evolution|DERL]] · Gupta et al. · 2021 · Nature Communications · [Scholar](https://scholar.google.com/scholar?q=Embodied+intelligence+via+learning+and+evolution)
32. [[Co-designing versatile quadruped robots for dynamic and energy-efficient motions|Versatile quadruped co-design]] · Fadini et al. · 2024 · Robotica · [Scholar](https://scholar.google.com/scholar?q=Co-designing+versatile+quadruped+robots+for+dynamic+and+energy-efficient+motions)
33. [[Meta Reinforcement Learning for Optimal Design of Legged Robots|Meta-RL design]] · Belmonte-Baeza et al. · 2022 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Meta+Reinforcement+Learning+for+Optimal+Design+of+Legged+Robots)
34. [[Co-Design of a Differential Three-Segment Leg Enhancing Payload and Efficiency in Quadrupeds|Differential three-segment leg]] · Song et al. · 2026 · IEEE/ASME TMech · [Scholar](https://scholar.google.com/scholar?q=Co-Design+of+a+Differential+Three-Segment+Leg+Enhancing+Payload+and+Efficiency+in+Quadrupeds)
35. [[A Switchable Parallel Elastic Actuator and Its Application to Leg Design for Running Robots|Switchable PEA]] · Liu et al. · 2018 · IEEE/ASME TMech · [Scholar](https://scholar.google.com/scholar?q=A+Switchable+Parallel+Elastic+Actuator+and+Its+Application+to+Leg+Design+for+Running+Robots)
36. [[Robust Quadruped Jumping via Deep Reinforcement Learning|Robust quadruped jumping]] · Bellegarda et al. · 2024 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Robust+Quadruped+Jumping+via+Deep+Reinforcement+Learning)
37. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
38. [[Brax - A Differentiable Physics Engine for Large Scale Rigid Body Simulation|Brax]] · Freeman et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2106.13281)
39. [[Isaac Lab - A GPU-Accelerated Simulation Framework for Multi-Modal Robot Learning|Isaac Lab]] · Mittal et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2511.04831)
40. [[Multiobjective Tree-Structured Parzen Estimator|MOTPE]] · Ozaki et al. · 2022 · JAIR · [Scholar](https://scholar.google.com/scholar?q=Multiobjective+Tree-Structured+Parzen+Estimator)
41. [[Optuna - A Next-generation Hyperparameter Optimization Framework|Optuna]] · Akiba et al. · 2019 · KDD · [Scholar](https://scholar.google.com/scholar?q=Optuna+A+Next-generation+Hyperparameter+Optimization+Framework)
42. [[Completely Derandomized Self-Adaptation in Evolution Strategies|CMA-ES]] · Hansen and Ostermeier · 2001 · Evolutionary Computation · [Scholar](https://scholar.google.com/scholar?q=Completely+Derandomized+Self-Adaptation+in+Evolution+Strategies)
43. [[Implementation of a Reactive Walking Controller for the New Open-Hardware Quadruped Solo-12|Solo-12]] · Léziart et al. · 2021 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Implementation+of+a+Reactive+Walking+Controller+for+the+New+Open-Hardware+Quadruped+Solo-12)
44. [[Unitree Go1|Unitree Go1]] · Unitree · 2024 · Website · [Link](https://www.unitree.com/en/go1/)
45. [[mjbots power distribution board|mjbots power dist]] · mjbots · 2024 · Website · [Link](https://mjbots.com/products/mjbots-power-dist-r4-5b)
46. [[NVIDIA Jetson Orin|Jetson Orin]] · NVIDIA · 2024 · Website · [Link](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/)
47. [[PEAK PCAN-M.2|PCAN-M.2]] · PEAK-System · 2024 · Website · [Link](https://www.peak-system.com/PCAN-M-2.473.0.html?&L=1)
48. [[CANable - An open-source USB to CAN adapter|CANable]] · CANable · 2024 · Website · [Link](https://canable.io/)
49. [[InvenSense MPU-6050|MPU-6050]] · InvenSense · 2024 · Website · [Link](https://invensense.tdk.com/products/motion-tracking/6-axis/mpu-6050/)
