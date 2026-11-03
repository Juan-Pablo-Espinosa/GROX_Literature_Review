---
title: "Berkeley Humanoid: A Research Platform for Learning-based Control"
aliases:
  - Berkeley Humanoid
authors:
  - "[[Qiayuan Liao]]"
  - "[[Bike Zhang]]"
  - "[[Xuanyu Huang]]"
  - "[[Xiaoyu Huang]]"
  - "[[Zhongyu Li]]"
  - "[[Koushil Sreenath]]"
corresponding: ""
institutions:
  - "[[University of California, Berkeley]]"
labs:
  - "[[Hybrid Robotics Lab]]"
countries:
  - USA
year: 2024
date: 2024-07-31
venue: IEEE ICRA 2025 (as cited by later work) · arXiv 2024
venue_type: conference
arxiv_category: cs.RO
arxiv_id: "2407.21781"
url: https://arxiv.org/abs/2407.21781
pdf: https://arxiv.org/pdf/2407.21781
code: https://github.com/HybridRobotics/isaac_berkeley_humanoid
project_page: https://berkeley-humanoid.com/
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/hardware-design
  - topic/actuators
  - topic/legged-locomotion
  - topic/sim-to-real
  - topic/domain-randomization
  - method/reinforcement-learning
  - method/ppo
  - sim/isaac-lab
  - robot/berkeley-humanoid
  - eval/real-robot
---

# Berkeley Humanoid

> [!info] Quick view
> **Authors:** [[Qiayuan Liao]], [[Bike Zhang]], [[Xuanyu Huang]], [[Xiaoyu Huang]], [[Zhongyu Li]], [[Koushil Sreenath]]
> **From:** [[University of California, Berkeley]] · [[Hybrid Robotics Lab]]
> **Published:** ICRA 2025 · arXiv Jul 2024 · cs.RO
> **Keywords:** mid-scale humanoid, QDD actuators, simulation-friendly design, narrow sim-to-real gap, minimal RL controller, accurate domain randomization, hopping
> **Links:** [arXiv](https://arxiv.org/abs/2407.21781) · [PDF](https://arxiv.org/pdf/2407.21781) · [Code](https://github.com/HybridRobotics/isaac_berkeley_humanoid) · [Project](https://berkeley-humanoid.com/)
> **One line:** a 16 kg, 0.85 m humanoid whose hardware is designed to be easy to simulate, so a plain MLP PPO policy with small, identified randomization transfers zero-shot outdoors

## Summary

The thesis is that hardware should be designed for learning, not only for model-based control. Berkeley Humanoid has two 6-DoF legs (thigh 220 mm, calf 180 mm, like a 5-year-old), custom quasi-direct-drive actuators (9:1 planetary, four sizes up to 81 Nm peak), no springs or closed chains except a linear linkage for the ankle pitch, actuators mounted directly as joints with crossed-roller bearings, hollow shafts for cables, and EtherCAT so latency is 0.5 to 2 ms. Because of this the simulation only needs joint dynamics plus rotor armature, and runs above 90k steps/s on one GPU. It costs about 10k USD without arms, with a sub-dollar phone IMU.

The controller is deliberately minimal: an MLP actor-critic [512, 256, 128] trained with PPO in Isaac Lab, observation is only the current state (no history, no phase clock, no teacher), actions are joint targets at 50 Hz. Domain randomization is small and based on identified parameters (CAD inertias, measured joint friction), with wide randomization only for the environment (ground friction, pushes), and they avoid "lazy" randomization like motor strength or PD gains. The robot walks omnidirectionally outdoors, climbs a 20° dusty trail, walks 364 m around campus, recovers from kicks, and even hops on one leg. Velocity tracking error is 0.051 vs 0.058 m/s (sim vs real) forward and 0.086 vs 0.116 m/s sideways. Honestly the "narrow gap" evidence is just this one tracking comparison, but the design philosophy is very convincing and well argued.

## How it works

- Actuators 5013, 8513, 8518, 10413: 251 to 1011 g, peak torque 9.7 to 81.1 Nm, 9:1 planetary, motor drivers with 1 kHz torque loop
- Kinematics designed for simulation: actuators are the joints, only the ankle pitch uses a linkage with a linear joint-actuator map
- Communication via EtherCAT at 1 to 4 kHz, PD at 25 kHz on drivers, state estimator at 1 kHz
- Joint torque from motor current, generalized momentum observer for contact estimation, no force sensors
- Policy observations: base angular velocity, projected gravity, joint positions and velocities, estimated base linear velocity, commands, last action
- Randomization: ground friction 0.2 to 1.25, restitution, base mass ±1 kg, link mass and joint friction ±10%, armature +5%, small joint offsets and sensor noise
- Hopping uses the same pipeline with a positive vertical velocity reward

## Results that matter

- 16 kg, 0.85 m, about 10k USD (legs and torso only)
- Sim-to-real velocity error: forward 0.051 sim vs 0.058 real m/s, lateral 0.086 vs 0.116 m/s
- 364 m campus walk in 10 minutes, 96 m up a rough trail with 10.5 m elevation gain
- 4 cm rocky stairs, kicks while walking, double and single leg hopping
- 38 recorded falls with no hardware damage except loose screws and glue
- Joint ranges cover 78 to 100% of human ranges

## Interesting stuff

- The "design for simulation" principle is strong: every closed chain, spring or unmodeled transmission is a sim-to-real gap you pay for later
- Refusing to randomize PD gains and motor strength, and instead identifying the hardware, is a nice counterpoint to typical heavy DR
- No history in the policy, yet robust, which says the dynamics are well matched
- No arms, and quantitative evaluation is light (one tracking plot, qualitative terrain tests)

## Useful for me

- **Apply.** For a morphing humanoid this is the main lesson: keep the length-changing mechanism simple to simulate (direct prismatic joints, no hidden linkages or springs), otherwise every body configuration adds sim-to-real gap.
- **Apply.** Identify the hardware well and randomize narrowly, instead of hiding actuator uncertainty under wide DR. With a body that changes, wide DR on top of real morphology changes would make the policy too conservative.
- **As prior work.** Reference platform and training recipe for small humanoid RL, with open code in Isaac Lab.

## Key prior work

- [[Learning to Walk in Minutes Using Massively Parallel Deep Reinforcement Learning|legged_gym]] · massively parallel training recipe they build on
- [[Mini Cheetah - A Platform for Pushing the Limits of Dynamic Quadruped Control|Mini Cheetah]] · QDD actuator design reference
- [[Real-world humanoid locomotion with reinforcement learning|Real-world humanoid RL]] · same group, learning-based humanoid locomotion
- [[Reinforcement Learning for Versatile, Dynamic, and Robust Bipedal Locomotion Control|Versatile bipedal RL]] · Cassie RL with history, the opposite design choice
- [[Closing the sim-to-real loop - Adapting simulation randomization with real world experience|Closing the sim-to-real loop]] · why too much randomization hurts
- [[Learning agile and dynamic motor skills for legged robots|ANYmal agile skills]] · actuator networks for sim-to-real

## References

1. [[Overview of the torque-controlled humanoid robot TORO|TORO]] · Englsberger et al. · 2014 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Overview+of+the+torque-controlled+humanoid+robot+TORO)
2. [[LOLA v1.1 - An Upgrade in Hardware and Software Design for Dynamic Multi-Contact Locomotion|LOLA v1.1]] · Seiwald et al. · 2021 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Lola+v1.1+an+upgrade+in+hardware+and+software+design+for+dynamic+multi-contact+locomotion)
3. [[WALK-MAN - A High-Performance Humanoid Platform for Realistic Environments|WALK-MAN]] · Tsagarakis et al. · 2017 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=Walk-man+A+high-performance+humanoid+platform+for+realistic+environments)
4. [[Meet Digit - The newest robot from Agility Robotics|Digit]] · Agility Robotics · 2024 · web page · [Link](https://agilityrobotics.com/products/digit)
5. [[Unitree H1|Unitree H1]] · Unitree Robotics · 2024 · product page · [Link](https://www.unitree.com/h1/)
6. [[Facilitating model-based control through software-hardware co-design|Software-hardware co-design]] · Ramos et al. · 2018 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Facilitating+model-based+control+through+software-hardware+co-design)
7. [[A low cost modular actuator for dynamic robots|Katz actuator thesis]] · Katz · 2018 · MSc thesis, MIT · [Scholar](https://scholar.google.com/scholar?q=Katz+A+low+cost+modular+actuator+for+dynamic+robots)
8. [[The MIT Humanoid Robot - Design, Motion Planning, and Control for Acrobatic Behaviors|MIT Humanoid acrobatics]] · Chignoli et al. · 2021 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=The+MIT+humanoid+robot+Design+motion+planning+and+control+for+acrobatic+behaviors)
9. [[Mini Cheetah - A Platform for Pushing the Limits of Dynamic Quadruped Control|Mini Cheetah]] · Katz et al. · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Mini+cheetah+A+platform+for+pushing+the+limits+of+dynamic+quadruped+control)
10. [[Design and Control of a Miniature Bipedal Robot with Proprioceptive Actuation for Dynamic Behaviors|BRUCE]] · Liu et al. · 2022 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+and+control+of+a+miniature+bipedal+robot+with+proprioceptive+actuation+for+dynamic+behaviors)
11. [[An Open Torque-Controlled Modular Robot Architecture for Legged Locomotion Research|Open Dynamic Robot Initiative]] · Grimminger et al. · 2020 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=An+open+torque-controlled+modular+robot+architecture+for+legged+locomotion+research)
12. [[Dynamic Walking on Highly Underactuated Point Foot Humanoids - Closing the Loop between HZD and HLIP|HZD and HLIP]] · Ghansah et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2406.13115)
13. [[ANYmal - a highly mobile and dynamic quadrupedal robot|ANYmal]] · Hutter et al. · 2016 · IROS · [Scholar](https://scholar.google.com/scholar?q=Anymal+a+highly+mobile+and+dynamic+quadrupedal+robot)
14. [[Design of a high torque density modular actuator for dynamic robots|Hattori actuator thesis]] · Hattori · 2020 · thesis, MIT · [Scholar](https://scholar.google.com/scholar?q=Hattori+Design+of+a+high+torque+density+modular+actuator+for+dynamic+robots)
15. [[Cassie sets a Guinness world record|Cassie record]] · Agility Robotics · 2022 · web page · [Link](https://agilityrobotics.com/news/2022/cassie-sets-a-guinness-world-record)
16. [[Design of a highly dynamic humanoid robot|ARTEMIS thesis]] · Zhu · 2023 · thesis, UCLA · [Scholar](https://scholar.google.com/scholar?q=Zhu+Design+of+a+highly+dynamic+humanoid+robot+UCLA)
17. [[Dynamic loco-manipulation on HECTOR - Humanoid for Enhanced ConTrol and Open-source Research|HECTOR]] · Li et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2312.11868)
18. [[Design and Development of the MIT Humanoid - A Dynamic and Robust Research Platform|MIT Humanoid]] · SaLoutos et al. · 2023 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Design+and+development+of+the+mit+humanoid+A+dynamic+and+robust+research+platform)
19. [[Unitree G1|Unitree G1]] · Unitree Robotics · 2024 · product page · [Link](https://www.unitree.com/g1/)
20. [[The HERMES humanoid system - A platform for full-body teleoperation with balance feedback|HERMES]] · Wang et al. · 2015 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=The+hermes+humanoid+system+A+platform+for+full-body+teleoperation+with+balance+feedback)
21. [[Bio-inspired knee joint mechanism for a hydraulic quadruped robot|Bio-inspired hydraulic knee]] · Khan et al. · 2015 · ICARA · [Scholar](https://scholar.google.com/scholar?q=Bio-inspired+knee+joint+mechanism+for+a+hydraulic+quadruped+robot)
22. [[Boston Dynamics Atlas|Atlas]] · Boston Dynamics · 2024 · web page · [Link](https://www.bostondynamics.com/atlas)
23. [[Legged Robots That Balance|Legged Robots That Balance]] · Raibert · 1986 · MIT Press · [Scholar](https://scholar.google.com/scholar?q=Raibert+Legged+Robots+That+Balance)
24. [[The 3D linear inverted pendulum mode - A simple modeling for a biped walking pattern generation|3D-LIPM]] · Kajita et al. · 2001 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+3D+linear+inverted+pendulum+mode+A+simple+modeling+for+a+biped+walking+pattern+generation)
25. [[Optimization-based locomotion planning, estimation, and control design for the Atlas humanoid robot|Atlas optimization-based control]] · Kuindersma et al. · 2016 · Autonomous Robots · [Scholar](https://scholar.google.com/scholar?q=Optimization-based+locomotion+planning+estimation+and+control+design+for+the+atlas+humanoid+robot)
26. [[Orientation-Aware Model Predictive Control with Footstep Adaptation for Dynamic Humanoid Walking|Orientation-aware MPC]] · Ding et al. · 2022 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Orientation-aware+model+predictive+control+with+footstep+adaptation+for+dynamic+humanoid+walking)
27. [[Tailoring Solution Accuracy for Fast Whole-body Model Predictive Control of Legged Robots|Fast WB-MPC]] · Khazoom et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2407.10789)
28. [[Sim-to-Real Learning of All Common Bipedal Gaits via Periodic Reward Composition|Periodic reward composition]] · Siekmann et al. · 2021 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Sim-to-real+learning+of+all+common+bipedal+gaits+via+periodic+reward+composition)
29. [[Real-world humanoid locomotion with reinforcement learning|Real-world humanoid RL]] · Radosavovic et al. · 2024 · Science Robotics · [arXiv](https://arxiv.org/abs/2303.03381)
30. [[Learning Bipedal Walking for Humanoids with Current Feedback|Current feedback walking]] · Singh et al. · 2023 · IEEE Access · [Scholar](https://scholar.google.com/scholar?q=Learning+bipedal+walking+for+humanoids+with+current+feedback)
31. [[HumanMimic - Learning Natural Locomotion and Transitions for Humanoid Robot via Wasserstein Adversarial Imitation|HumanMimic]] · Tang et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2309.14225)
32. [[Humanoid Locomotion as Next Token Prediction|Next token locomotion]] · Radosavovic et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2402.19469)
33. [[Expressive Whole-Body Control for Humanoid Robots|ExBody]] · Cheng et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2402.16796)
34. [[Learning Human-to-Humanoid Real-Time Whole-Body Teleoperation|H2O]] · He et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2403.04436)
35. [[Sim-to-Real Learning for Humanoid Box Loco-Manipulation|Humanoid box loco-manipulation]] · Dao et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2310.03191)
36. [[HumanPlus - Humanoid Shadowing and Imitation from Humans|HumanPlus]] · Fu et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2406.10454)
37. [[Blind Bipedal Stair Traversal via Sim-to-Real Reinforcement Learning|Blind stair traversal]] · Siekmann et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2105.08328)
38. [[Advancing Humanoid Locomotion - Mastering Challenging Terrains with Denoising World Model Learning|Denoising world model]] · Gu et al. · 2024 · RSS · [Scholar](https://scholar.google.com/scholar?q=Advancing+Humanoid+Locomotion+Mastering+Challenging+Terrains+with+Denoising+World+Model+Learning)
39. [[Revisiting Reward Design and Evaluation for Robust Humanoid Standing and Walking|Reward design standing walking]] · van Marum et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2404.19173)
40. [[Optimizing bipedal locomotion for the 100m dash with comparison to human running|Cassie 100m dash]] · Crowley et al. · 2023 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Optimizing+bipedal+locomotion+for+the+100m+dash+with+comparison+to+human+running)
41. [[Humanoid Parkour Learning|Humanoid parkour]] · Zhuang et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2406.10759)
42. [[Closing the sim-to-real loop - Adapting simulation randomization with real world experience|Closing the sim-to-real loop]] · Chebotar et al. · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Closing+the+sim-to-real+loop+Adapting+simulation+randomization+with+real+world+experience)
43. [[Learning agile soccer skills for a bipedal robot with deep reinforcement learning|Bipedal soccer]] · Haarnoja et al. · 2024 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+agile+soccer+skills+for+a+bipedal+robot+with+deep+reinforcement+learning)
44. [[The design of the iCub humanoid robot|iCub]] · Parmiggiani et al. · 2012 · IJHR · [Scholar](https://scholar.google.com/scholar?q=The+design+of+the+icub+humanoid+robot)
45. [[Mechatronic design of NAO humanoid|NAO]] · Gouaillier et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Mechatronic+design+of+NAO+humanoid)
46. [[Development of open humanoid platform DARwIn-OP|DARwIn-OP]] · Ha et al. · 2011 · SICE · [Scholar](https://scholar.google.com/scholar?q=Development+of+open+humanoid+platform+darwin-op)
47. [[Design and implementation of small-sized 3D printed Surena-Mini humanoid platform|Surena-Mini]] · Nikkhah et al. · 2017 · ICRoM · [Scholar](https://scholar.google.com/scholar?q=Design+and+implementation+of+small-sized+3d+printed+surena-mini+humanoid+platform)
48. [[Sim-to-Real - Learning Agile Locomotion For Quadruped Robots|Sim-to-real agile locomotion]] · Tan et al. · 2018 · arXiv · [arXiv](https://arxiv.org/abs/1804.10332)
49. [[Reinforcement Learning for Robust Parameterized Locomotion Control of Bipedal Robots|Parameterized bipedal RL]] · Li et al. · 2021 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Reinforcement+learning+for+robust+parameterized+locomotion+control+of+bipedal+robots)
50. [[Learning Locomotion Skills for Cassie - Iterative Design and Sim-to-Real|Cassie iterative design]] · Xie et al. · 2020 · CoRL · [Scholar](https://scholar.google.com/scholar?q=Learning+locomotion+skills+for+cassie+Iterative+design+and+sim-to-real)
51. [[Learning agile and dynamic motor skills for legged robots|ANYmal agile skills]] · Hwangbo et al. · 2019 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+agile+and+dynamic+motor+skills+for+legged+robots)
52. [[K-Scale Labs|K-Scale]] · K-Scale Labs · 2024 · web page · [Link](https://kscale.dev/)
53. [[Design and characterization of 3D printed, open-source actuators for legged locomotion|3D printed actuators]] · Urs et al. · 2022 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+and+characterization+of+3d+printed+open-source+actuators+for+legged+locomotion)
54. [[DRPD, Dual Reduction Ratio Planetary Drive for Articulated Robot Actuators|DRPD]] · Song et al. · 2022 · IROS · [Scholar](https://scholar.google.com/scholar?q=Drpd+dual+reduction+ratio+planetary+drive+for+articulated+robot+actuators)
55. [[Design, development, and analysis of the lower body of next-generation 3D-printed humanoid research platform - PANDORA|PANDORA]] · Fuge et al. · 2023 · Robotica · [Scholar](https://scholar.google.com/scholar?q=Design+development+and+analysis+of+the+lower+body+of+next-generation+3d-printed+humanoid+research+platform+Pandora)
56. [[Collision detection and reaction - A contribution to safe physical human-robot interaction|Collision detection and reaction]] · Haddadin et al. · 2008 · IROS · [Scholar](https://scholar.google.com/scholar?q=Collision+detection+and+reaction+A+contribution+to+safe+physical+human-robot+interaction)
57. [[Spot - the agile mobile robot|Spot]] · Boston Dynamics · 2024 · product page · [Link](https://bostondynamics.com/products/spot)
58. [[Optimal degrees of freedom of the lower extremities for human walking and running|Lower limb DoF for walking]] · Kudo et al. · 2023 · Scientific Reports · [Scholar](https://scholar.google.com/scholar?q=Optimal+degrees+of+freedom+of+the+lower+extremities+for+human+walking+and+running)
59. [[Stability of surface contacts for humanoid robots - Closed-form formulae of the contact wrench cone for rectangular support areas|Contact wrench cone]] · Caron et al. · 2015 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Stability+of+surface+contacts+for+humanoid+robots+Closed-form+formulae+of+the+contact+wrench+cone)
60. [[Reinforcement Learning for Versatile, Dynamic, and Robust Bipedal Locomotion Control|Versatile bipedal RL]] · Li et al. · 2024 · arXiv · [arXiv](https://arxiv.org/abs/2401.16889)
61. [[RMA - Rapid Motor Adaptation for Legged Robots|RMA]] · Kumar et al. · 2021 · RSS · [arXiv](https://arxiv.org/abs/2107.04034)
62. [[Learning quadrupedal locomotion over challenging terrain|Challenging terrain locomotion]] · Lee et al. · 2020 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Learning+quadrupedal+locomotion+over+challenging+terrain)
63. [[Experimental evaluation of simple estimators for humanoid robots|Simple humanoid estimators]] · Flayols et al. · 2017 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Experimental+evaluation+of+simple+estimators+for+humanoid+robots)
64. [[Proximal Policy Optimization Algorithms|PPO]] · Schulman et al. · 2017 · arXiv · [arXiv](https://arxiv.org/abs/1707.06347)
65. [[Orbit - A Unified Simulation Framework for Interactive Robot Learning Environments|Orbit]] · Mittal et al. · 2023 · IEEE RA-L · [DOI](https://doi.org/10.1109/LRA.2023.3270034)
66. [[Learning Agile Robotic Locomotion Skills by Imitating Animals|Imitating animals]] · Peng et al. · 2020 · arXiv · [arXiv](https://arxiv.org/abs/2004.00784)
67. [[Actuator-Constrained Reinforcement Learning for High-Speed Quadrupedal Locomotion|Actuator-constrained RL]] · Shin et al. · 2023 · arXiv · [arXiv](https://arxiv.org/abs/2312.17507)
68. [[Learning to Walk in Minutes Using Massively Parallel Deep Reinforcement Learning|legged_gym]] · Rudin et al. · 2022 · CoRL · [arXiv](https://arxiv.org/abs/2109.11978)
69. [[Brunnstrom's Clinical Kinesiology|Brunnstrom's Clinical Kinesiology]] · Houglum & Bertoti · 2011 · FA Davis · [Scholar](https://scholar.google.com/scholar?q=Brunnstrom%27s+clinical+kinesiology)
70. [[Kinesiology - Scientific Basis of Human Motion|Kinesiology, Hamilton]] · Hamilton · 2011 · Brown & Benchmark · [Scholar](https://scholar.google.com/scholar?q=Hamilton+Kinesiology+Scientific+basis+of+human+motion)
71. [[Technique for the measurement of hindfoot inversion and eversion and its use to study a normal population|Hindfoot inversion measurement]] · Ball & Johnson · 1996 · Clinical Biomechanics · [Scholar](https://scholar.google.com/scholar?q=Technique+for+the+measurement+of+hindfoot+inversion+and+eversion+and+its+use+to+study+a+normal+population)

## See also

- [[Demonstrating Berkeley Humanoid Lite - An Open-source, Accessible, and Customizable 3D-printed Humanoid Robot|Berkeley Humanoid Lite]] · the open-source 3D-printed follow-up
- [[Control of Humanoid Robots with Parallel Mechanisms using Differential Actuation Models|Differential actuation control]] · the other way: keep linkages and model them
