---
title: "Demonstrating Berkeley Humanoid Lite: An Open-source, Accessible, and Customizable 3D-printed Humanoid Robot"
aliases:
  - Berkeley Humanoid Lite
authors:
  - "[[Yufeng Chi]]"
  - "[[Qiayuan Liao]]"
  - "[[Junfeng Long]]"
  - "[[Xiaoyu Huang]]"
  - "[[Sophia Shao]]"
  - "[[Borivoje Nikolic]]"
  - "[[Zhongyu Li]]"
  - "[[Koushil Sreenath]]"
corresponding: ""
institutions:
  - "[[University of California, Berkeley]]"
labs:
  - "[[Hybrid Robotics Lab]]"
countries:
  - USA
year: 2025
date: 2025-04-24
venue: Robotics - Science and Systems (RSS) 2025
venue_type: conference
arxiv_category: cs.RO
arxiv_id: "2504.17249"
url: https://arxiv.org/abs/2504.17249
pdf: https://arxiv.org/pdf/2504.17249
code: "All hardware, firmware and training code linked from https://lite.berkeley-humanoid.org (checked 2026-11-02)"
project_page: https://lite.berkeley-humanoid.org
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/hardware-design
  - topic/actuators
  - topic/reconfigurable-robots
  - topic/morphology
  - topic/teleoperation
  - topic/sim-to-real
  - method/reinforcement-learning
  - method/ppo
  - sim/isaac-gym
  - robot/berkeley-humanoid-lite
  - eval/real-robot
---

# Berkeley Humanoid Lite

> [!info] Quick view
> **Authors:** [[Yufeng Chi]], [[Qiayuan Liao]], [[Junfeng Long]], [[Xiaoyu Huang]], [[Sophia Shao]], [[Borivoje Nikolic]], [[Zhongyu Li]], [[Koushil Sreenath]]
> **From:** [[University of California, Berkeley]] · [[Hybrid Robotics Lab]]
> **Published:** RSS 2025 · arXiv Apr 2025 · cs.RO
> **Keywords:** open-source humanoid, 3D-printed cycloidal actuator, low cost, modular joints, reconfigurable morphology, RL locomotion, VR teleoperation
> **Links:** [arXiv](https://arxiv.org/abs/2504.17249) · [PDF](https://arxiv.org/pdf/2504.17249) · [Project and code](https://lite.berkeley-humanoid.org)
> **One line:** a 16 kg, 0.8 m humanoid built from PLA-printed cycloidal actuators and online-store parts for under 5,000 USD, fully open source, and easy to reconfigure into other bodies

## Summary

The goal is to make humanoid hardware accessible. Every joint is a self-contained actuator: a drone BLDC motor, a cheap B-G431B-ESC1 driver, an AS5600 encoder and a PLA cycloidal gearbox printed on a normal desktop printer (all parts fit 200 mm cubes, with brass standoffs and through-screws against layer failure). Cycloidal gears are chosen because they spread load over many teeth and tolerate printer resolution better than planetary gears. Two sizes (6512 for legs, 5010 for arms) cost about 188 and 136 USD each, and the whole robot with 22 actuators, mini PC, IMU and battery costs 4,312 USD in the US. Communication is 4 CAN buses at 250 Hz.

Actuator tests show about 90% gearbox efficiency in most of the range, 319 Nm/rad stiffness, backlash up to 0.023 rad, stable efficiency over a 60-hour pendulum durability test (backlash grows slowly), and consistent behavior across actuators printed on different printers. A PPO policy trained in Isaac Gym with only proprioception transfers zero-shot for velocity-tracking walking at 25 Hz using only about 30% of the actuator torque limit, and VR teleoperation with two 5-DoF arms does writing, packing, Rubik's cube and pick-and-place. Because joints are self-contained, link lengths can be changed with carbon tubes (an adult-size leg is shown) and the same actuators build a quadruped, centaur or mobile base. Honestly the locomotion part is shown only qualitatively (flat walking), but as a cheap, modifiable platform it is very interesting.

## How it works

- Actuator: M6C12 or 5010 drone motor, 3D-printed cycloidal disks and housing, embedded brass hex for the input shaft, 6811ZZ bearing defines the size
- Robot: 16 kg, 0.8 m, legs with 6 DoF and arms with 5 DoF plus grippers, aluminum extrusion torso, Intel N95 PC, BNO085 IMU, 6S LiPo (about 30 min)
- Performance factor: average peak joint torque normalized by height and weight, and performance per dollar to compare robots
- Customization: link lengths changed with carbon tubes, hip joint order swappable, actuators reused for other morphologies on the same CAN bus
- RL: PPO, observations are base angular velocity, gravity, joint positions and velocities, command, last action, outputs joint targets
- Teleoperation: SteamVR trackers, headless or VR mode, IK with Pink and Pinocchio

## Results that matter

- Cost 4,312 USD (US) or 3,236 USD (China), assembly in about 3 days
- Gearbox efficiency about 90% in most of the range, lower at high torque and speed due to heat
- Transmission stiffness 319 Nm/rad (vs 1468 Nm/rad for PA-CF prints in prior work)
- 60 h durability test passed with slowly increasing backlash, torque error within ±0.5 Nm across six units from two printers
- 5-DoF arm repeatability std 3.4 mm over 100 reaches
- Zero-shot walking with about 30% of actuator torque used

## Interesting stuff

- Self-contained joints with no linkages are what make link length changes trivial, a nice design rule
- PLA stiffness is low, so compliance and backlash are real and should be in the sim
- Thermal effects on printed parts are not studied yet, they admit it
- Locomotion evaluation is minimal compared with the actuator tests

## Useful for me

- **Apply.** A very practical base for prototyping a morphing humanoid: cheap actuators, open CAD, and link lengths that are easy to change with tubes. Testing different body sizes on real hardware becomes affordable.
- **Apply.** The 30% torque usage suggests the same actuators could drive a bigger body, which matters if the limbs grow.
- **Watch out.** Printed gearboxes have low stiffness and growing backlash, so randomize backlash and compliance and re-identify over time.
- **As prior work.** Reference for low-cost open humanoid platforms and reconfigurable morphologies from the same modules.

## Key prior work

- [[Berkeley Humanoid - A Research Platform for Learning-based Control|Berkeley Humanoid]] · the metal predecessor
- [[3D-printable low-reduction cycloidal gearing for robotics|3D-printable cycloidal gearing]] · the printed cycloidal gear basis
- [[Experimental comparison of pinwheel and non-pinwheel designs of 3D-printed cycloidal gearing for robotics|Pinwheel vs non-pinwheel]] · printed cycloid design comparison
- [[Design and characterization of 3D printed, open-source actuators for legged locomotion|3D printed actuators]] · printed planetary alternative
- [[ToddlerBot - Open-Source ML-Compatible Humanoid Platform for Loco-Manipulation|ToddlerBot]] · other cheap open humanoid and origin of the performance factor
- [[Rapid morphological exploration with the Poppy humanoid platform|Poppy morphological exploration]] · earlier open printed humanoid for morphology exploration

## References

1. [[Meet Digit - The newest robot from Agility Robotics|Digit]] · Agility Robotics · 2024 · web page · [Link](https://agilityrobotics.com/products/digit)
2. [[Development and Real-Time Optimization-based Control of a Full-sized Humanoid for Dynamic Walking and Running|THEMIS thesis]] · Ahn · 2023 · thesis, UCLA · [Scholar](https://scholar.google.com/scholar?q=Development+and+Real-Time+Optimization-based+Control+of+a+Full-sized+Humanoid+for+Dynamic+Walking+and+Running)
3. [[From Anime To Reality - Embodying An Anime Character As A Humanoid Robot|Anime to reality]] · Al-Sada et al. · 2021 · CHI Extended Abstracts · [Scholar](https://scholar.google.com/scholar?q=From+Anime+To+Reality+Embodying+An+Anime+Character+As+A+Humanoid+Robot)
4. [[Design and characterization of an open-source robotic leg prosthesis|Open-source leg prosthesis]] · Azocar et al. · 2018 · BioRob · [Scholar](https://scholar.google.com/scholar?q=Design+and+characterization+of+an+open-source+robotic+leg+prosthesis)
5. [[Pink - Python inverse kinematics based on Pinocchio|Pink]] · Caron et al. · 2024 · software · [GitHub](https://github.com/stephane-caron/pink)
6. [[Pinocchio - fast forward and inverse dynamics for poly-articulated systems|Pinocchio software]] · Carpentier et al. · 2015-2021 · software · [Link](https://stack-of-tasks.github.io/pinocchio)
7. [[The Pinocchio C++ library - A fast and flexible implementation of rigid body dynamics algorithms and their analytical derivatives|Pinocchio]] · Carpentier et al. · 2019 · SII · [Scholar](https://scholar.google.com/scholar?q=The+Pinocchio+C%2B%2B+library+A+fast+and+flexible+implementation+of+rigid+body+dynamics+algorithms)
8. [[NimbRo-OP2 - Grown-up 3D printed open humanoid platform for research|NimbRo-OP2]] · Ficht et al. · 2017 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=NimbRo-OP2+Grown-up+3D+printed+open+humanoid+platform+for+research)
9. [[Fourier GR-1|Fourier GR-1]] · Fourier · 2024 · product page · [Link](https://www.fftai.com/products-gr1)
10. [[Quasi-direct drive for low-cost compliant robotic manipulation|Blue arm QDD]] · Gealy et al. · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Quasi-direct+drive+for+low-cost+compliant+robotic+manipulation)
11. [[Design and Control of a Bipedal Robotic Character|Disney bipedal character]] · Grandia et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2501.05204)
12. [[An Open Torque-Controlled Modular Robot Architecture for Legged Locomotion Research|Open Dynamic Robot Initiative]] · Grimminger et al. · 2020 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=An+open+torque-controlled+modular+robot+architecture+for+legged+locomotion+research)
13. [[Humanoid robot HRP-5P - An electrically actuated humanoid robot with high-power and wide-range joints|HRP-5P]] · Kaneko et al. · 2019 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+HRP-5P+An+electrically+actuated+humanoid+robot+with+high-power+and+wide-range+joints)
14. [[A low cost modular actuator for dynamic robots|Katz actuator thesis]] · Katz · 2018 · thesis, MIT · [Scholar](https://scholar.google.com/scholar?q=Katz+A+low+cost+modular+actuator+for+dynamic+robots)
15. [[Stanford Doggo - An Open-Source, Quasi-Direct-Drive Quadruped|Stanford Doggo]] · Kau et al. · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Stanford+doggo+An+open-source+quasi-direct-drive+quadruped)
16. [[Rapid morphological exploration with the Poppy humanoid platform|Poppy morphological exploration]] · Lapeyre et al. · 2014 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Rapid+morphological+exploration+with+the+Poppy+humanoid+platform)
17. [[Empirical characterization of a high-performance exterior-rotor type brushless DC motor and drive|Exterior-rotor BLDC characterization]] · Lee et al. · 2019 · IROS · [Scholar](https://scholar.google.com/scholar?q=Empirical+characterization+of+a+high-performance+exterior-rotor+type+brushless+DC+motor+and+drive)
18. [[Dynamic loco-manipulation on HECTOR - Humanoid for Enhanced ConTrol and Open-source Research|HECTOR]] · Li et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2312.11868)
19. [[Animated Cassie - A Dynamic Relatable Robotic Character|Animated Cassie]] · Li et al. · 2020 · IROS · [Scholar](https://scholar.google.com/scholar?q=Animated+cassie+A+dynamic+relatable+robotic+character)
20. [[Berkeley Humanoid - A Research Platform for Learning-based Control|Berkeley Humanoid]] · Liao et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2407.21781)
21. [[Design and Control of a Miniature Bipedal Robot with Proprioceptive Actuation for Dynamic Behaviors|BRUCE]] · Liu et al. · 2022 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+and+control+of+a+miniature+bipedal+robot+with+proprioceptive+actuation+for+dynamic+behaviors)
22. [[Humanoid robot LOLA|LOLA]] · Lohmeier et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+LOLA)
23. [[Moteus r4.11|Moteus]] · mjbots · 2025 · product page · [Link](https://mjbots.com/products/moteus-r4-11)
24. [[NAO H25|NAO H25]] · Aldebaran · 2024 · documentation · [Link](http://doc.aldebaran.com/2-1/family/nao_h25/index_h25.html)
25. [[ODrive Robotics - High Performance Motor Control|ODrive]] · ODrive Robotics · 2025 · web page · [Link](https://odriverobotics.com)
26. [[The universal humanoid robot FXV2750|OpenLoong FXV2750]] · OpenLoong · 2024 · web page · [Link](https://www.openloong.org.cn/en/developer/robot)
27. [[Mechanical design of humanoid robot platform KHR-3 (KAIST Humanoid Robot 3 - HUBO)|KHR-3 HUBO]] · Park et al. · 2005 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Mechanical+design+of+humanoid+robot+platform+KHR-3+KAIST+humanoid+robot+3+HUBO)
28. [[The design of the iCub humanoid robot|iCub]] · Parmiggiani et al. · 2012 · IJHR · [Scholar](https://scholar.google.com/scholar?q=The+design+of+the+icub+humanoid+robot)
29. [[ROBOTIS OP3 e-Manual|OP3]] · ROBOTIS · 2018 · manual · [Link](https://emanual.robotis.com/docs/en/platform/op3/introduction/)
30. [[3D-printable low-reduction cycloidal gearing for robotics|3D-printable cycloidal gearing]] · Roozing & Roozing · 2022 · IROS · [Scholar](https://scholar.google.com/scholar?q=3D-printable+low-reduction+cycloidal+gearing+for+robotics)
31. [[Experimental comparison of pinwheel and non-pinwheel designs of 3D-printed cycloidal gearing for robotics|Pinwheel vs non-pinwheel]] · Roozing & Roozing · 2024 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Experimental+comparison+of+pinwheel+and+non-pinwheel+designs+of+3D-printed+cycloidal+gearing+for+robotics)
32. [[Design and Development of the MIT Humanoid - A Dynamic and Robust Research Platform|MIT Humanoid]] · SaLoutos et al. · 2023 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Design+and+development+of+the+mit+humanoid+A+dynamic+and+robust+research+platform)
33. [[ToddlerBot - Open-Source ML-Compatible Humanoid Platform for Loco-Manipulation|ToddlerBot]] · Shi et al. · 2025 · arXiv · [arXiv](https://arxiv.org/abs/2502.00893)
34. [[Unitree G1|Unitree G1]] · Unitree Robotics · 2024 · product page · [Link](https://www.unitree.com/g1)
35. [[Unitree H1|Unitree H1]] · Unitree Robotics · 2024 · product page · [Link](https://www.unitree.com/h1)
36. [[Design and characterization of 3D printed, open-source actuators for legged locomotion|3D printed actuators]] · Urs et al. · 2022 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+and+characterization+of+3d+printed+open-source+actuators+for+legged+locomotion)
37. [[VESC Project - Open Source ESC|VESC]] · Vedder & VESC community · 2025 · web page · [Link](https://vesc-project.com)
38. [[Hybrid Bipedal Locomotion Based on Reinforcement Learning and Heuristics|Wukong hybrid locomotion]] · Wang et al. · 2022 · Micromachines · [Scholar](https://scholar.google.com/scholar?q=Hybrid+bipedal+locomotion+based+on+reinforcement+learning+and+heuristics)
39. [[TriFinger - An Open-Source Robot for Learning Dexterity|TriFinger]] · Wüthrich et al. · 2020 · arXiv · [arXiv](https://arxiv.org/abs/2008.03596)
40. [[The Duke Humanoid - Design and Control For Energy Efficient Bipedal Locomotion Using Passive Dynamics|Duke Humanoid]] · Xia et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2409.19795)
41. [[Design of a highly dynamic humanoid robot|ARTEMIS thesis]] · Zhu · 2023 · thesis, UCLA · [Scholar](https://scholar.google.com/scholar?q=Zhu+Design+of+a+highly+dynamic+humanoid+robot+UCLA)
42. [[Influence of Hip Joint Axes Change of Orientation on Power Distribution in Humanoid Motion|ICARA 2011 hip axes]] · Zorjan et al. · 2011 · ICARA · [Scholar](https://scholar.google.com/scholar?q=Influence+of+hip+joint+axes+change+of+orientation+on+power+distribution+in+humanoid+motion)
43. [[Influence of rotation of humanoid hip joint axes on joint power during locomotion|Hip Axes Rotation]] · Zorjan et al. · 2015 · Advanced Robotics · [DOI](https://doi.org/10.1080/01691864.2015.1013057)

## See also

- [[Poppy - Open Source 3D Printed Robot for Experiments in Developmental Robotics|Poppy ICDL]] · earlier open 3D-printed humanoid
