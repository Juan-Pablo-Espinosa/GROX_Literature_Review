---
title: "On the Comprehensive Kinematics Analysis of a Humanoid Parallel Ankle Mechanism"
aliases:
  - Parallel Ankle Kinematics
  - CogIMon ankle
authors:
  - "[[Chengxu Zhou]]"
  - "[[Nikos Tsagarakis]]"
corresponding: "[[Chengxu Zhou]]"
institutions:
  - "[[Istituto Italiano di Tecnologia]]"
labs:
  - "[[IIT Humanoid and Human Centered Mechatronics]]"
countries:
  - Italy
year: 2018
date: 2018-08-06
venue: ASME Journal of Mechanisms and Robotics 2018 (Vol. 10, 051015)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1115/1.4040886
pdf: https://doi.org/10.1115/1.4040886
code: "No code (checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/leg-design
  - topic/parallel-mechanisms
  - topic/hardware-design
  - method/screw-theory
  - robot/cogimon
  - eval/real-robot
---

# Parallel Ankle Kinematics

> [!info] Quick view
> **Authors:** [[Chengxu Zhou]] (corresponding), [[Nikos Tsagarakis]]
> **From:** [[Istituto Italiano di Tecnologia]] · [[IIT Humanoid and Human Centered Mechatronics]]
> **Published:** ASME Journal of Mechanisms and Robotics · Aug 2018 · journal
> **Keywords:** 2-RSS-1-U parallel ankle, screw theory mobility, closed-form inverse kinematics, Jacobian, Newton-Raphson forward kinematics, workspace, motor limits, CogIMon
> **Links:** [DOI](https://doi.org/10.1115/1.4040886) · [Video](https://youtu.be/UsQDMN6KJnI)
> **One line:** complete kinematics of the CogIMon humanoid ankle (two rotary-crank RSS legs plus a universal joint), with analytic IK, Jacobian, fast numerical FK and motor limits chosen from the workspace

## Summary

Many humanoids use parallel ankles, but few papers give their full kinematics. This one does it for CogIMon's 2-RSS-1-U ankle: two motors on the shank turn cranks (85 mm), connected by rods (135 mm) through ball joints to the foot, while a universal joint links shank and foot. They argue RSS legs (rotary crank) are better than the common UPS/UPU linear actuator legs because the transmission between motor and ankle pitch torque stays constant over the range of motion, while linear legs lose torque when the foot rotates away from neutral.

Screw theory confirms the mechanism has exactly two rotational DoF (roll and pitch). They derive closed-form IK, the Jacobian between ankle and motor velocities, and solve FK with Newton-Raphson using the Jacobian (at most two iterations). A motor limit scan finds the smallest motor ranges [−64°, 50°] that cover the target workspace of roll ±20° and pitch [−58°, 42°]. Tests on the real tibia module show IK-FK round-trip errors below 1e-6 degree and sub-microsecond IK. Honestly it is a kinematics paper, no torque or walking results on the full robot, but it is the reference everyone cites for this ankle type.

## How it works

- Mechanism: two RSS limbs (revolute motor crank, spherical, spherical) and one universal joint limb at the ankle center
- Mobility via screw theory: reciprocal screw systems give 2 rotational DoF about x and y
- IK: closed form for each crank angle from foot roll and pitch
- Jacobian: maps ankle joint velocities to motor velocities, used for velocity and torque control
- FK: Newton-Raphson with the Jacobian, warm-started, ≤ 2 iterations in tests
- Workspace scan over motor limits to pick the minimum motor range for the desired ankle range

## Results that matter

- Desired ankle range roll ±20°, pitch [−58°, 42°], covered with motor limits [−64°, 50°]
- IK-FK errors below 1e-6 degree in all four tests
- IK about 5e-7 s per call, FK still real-time on a laptop
- A walking pattern with 0.3 m forward and 0.2 m side steps stays inside motor limits

## Interesting stuff

- The constant transmission argument for crank (RSS) vs linear (UPS) legs is a strong design point
- Very complete for a 2-DoF ankle, a good template for analyzing any closed-chain joint
- No dynamic tests, no torque measurement, the leg was not yet mounted on the robot
- The paper mostly organizes known math well, the novelty is the completeness

## Useful for me

- **As prior work.** The standard reference for parallel ankle kinematics, cited by the optimal ankle design and differential actuation papers.
- **Apply.** The workspace scan to choose minimum motor limits is simple and useful when designing joints for a body whose proportions change.
- **Watch out.** If the shank length changes, the rod geometry changes the ankle kinematics, so a growing leg with a parallel ankle needs the IK and Jacobian as a function of leg length.

## Key prior work

- [[Leg Design for a Humanoid Walking Robot|Lola leg]] · serial vs parallel ankle torque comparison
- [[Mechanism and control of whole-body electro-hydrostatic actuator driven humanoid robot Hydra|Hydra]] · closed-form parallel ankle on a hydraulic humanoid
- [[Team VALOR's ESCHER - A Novel Electromechanical Biped for the DARPA Robotics Challenge|ESCHER]] · ballscrew parallel ankle
- [[Parallel Elastic Elements Improve Energy Efficiency on the STEPPR Bipedal Walking Robot|STEPPR]] · parallel ankle with elastic elements
- [[Mobility of Overconstrained Parallel Mechanisms|Overconstrained mobility]] · screw theory method used

## References

1. [[A Humanoid Shoulder Complex and the Humeral Pointing Kinematics|Humanoid shoulder complex]] · Lenarcic & Stanisic · 2003 · IEEE T-RA · [Scholar](https://scholar.google.com/scholar?q=A+Humanoid+Shoulder+Complex+and+the+Humeral+Pointing+Kinematics)
2. [[A Novel Fully Decoupled Two-Degrees-of-Freedom Parallel Wrist|Decoupled 2-DoF wrist]] · Carricato & Parenti-Castelli · 2004 · IJRR · [Scholar](https://scholar.google.com/scholar?q=A+Novel+Fully+Decoupled+2DOF+Parallel+Wrist)
3. [[An Approach to Biped Robot With Parallel Mechanism|Parallel mechanism biped]] · Morisawa et al. · 2000 · AMC Workshop · [Scholar](https://scholar.google.com/scholar?q=An+Approach+to+Biped+Robot+With+Parallel+Mechanism)
4. [[Design of a Battery-Powered Multi-Purpose Bipedal Locomotor with Parallel Mechanism|Parallel bipedal locomotor]] · Sugahara et al. · 2002 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+of+a+Battery-Powered+Multi-Purpose+Bipedal+Locomotor+with+Parallel+Mechanism)
5. [[A High-Performance Redundantly Actuated Parallel Mechanism for Ankle Rehabilitation|ARBOT]] · Saglia et al. · 2009 · IJRR · [Scholar](https://scholar.google.com/scholar?q=A+High-Performance+Redundantly+Actuated+Parallel+Mechanism+for+Ankle+Rehabilitation)
6. [[Robot-Aided Neurorehabilitation - A Novel Robot for Ankle Rehabilitation|Anklebot]] · Roy et al. · 2009 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Robot-Aided+Neurorehabilitation+A+Novel+Robot+for+Ankle+Rehabilitation)
7. [[Leg Design for a Humanoid Walking Robot|Lola leg]] · Lohmeier et al. · 2006 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Leg+Design+for+a+Humanoid+Walking+Robot+Lohmeier)
8. [[Design and Experimental Evaluation of a Fast Torque-Controlled Hydraulic Humanoid Robot|TaeMu]] · Hyon et al. · 2017 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=Design+and+Experimental+Evaluation+of+a+Fast+Torque-Controlled+Hydraulic+Humanoid+Robot)
9. [[Optimization Based Controller Design and Implementation for the Atlas Robot in the DARPA Robotics Challenge Finals|Atlas DRC controller]] · Feng et al. · 2015 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Optimization+Based+Controller+Design+and+Implementation+for+the+Atlas+Robot+in+the+Darpa+Robotics+Challenge+Finals)
10. [[Mechanism and control of whole-body electro-hydrostatic actuator driven humanoid robot Hydra|Hydra]] · Kaminaga et al. · 2016 · ISER · [Scholar](https://scholar.google.com/scholar?q=Mechanism+and+Control+of+Whole-Body+Electro-Hydrostatic+Actuator+Driven+Humanoid+Robot+Hydra)
11. [[Team VALOR's ESCHER - A Novel Electromechanical Biped for the DARPA Robotics Challenge|ESCHER]] · Knabe et al. · 2017 · Journal of Field Robotics · [Scholar](https://scholar.google.com/scholar?q=Team+VALOR%27s+ESCHER+A+Novel+Electromechanical+Biped+for+the+DARPA+Robotics+Challenge)
12. [[Development of life-sized humanoid robot platform with robustness for falling down, long time working and error occurrence|RHP2]] · Kakiuchi et al. · 2017 · IROS · [Scholar](https://scholar.google.com/scholar?q=Development+of+Life-Sized+Humanoid+Robot+Platform+With+Robustness+for+Falling+Down+Long+Time+Working+and+Error+Occurrence)
13. [[Realizing Dynamic and Efficient Bipedal Locomotion on the Humanoid Robot DURUS|DURUS]] · Reher et al. · 2016 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Realizing+Dynamic+and+Efficient+Bipedal+Locomotion+on+the+Humanoid+Robot+DURUS)
14. [[Parallel Elastic Elements Improve Energy Efficiency on the STEPPR Bipedal Walking Robot|STEPPR]] · Mazumdar et al. · 2017 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=Parallel+Elastic+Elements+Improve+Energy+Efficiency+on+the+STEPPR+Bipedal+Walking+Robot)
15. [[Mechanical Design of Robot Lower Body Based on Four-Bar Linkage Structure for Energy Efficient Bipedal Walking|KAIST G-robot]] · Han et al. · 2016 · SSRR · [Scholar](https://scholar.google.com/scholar?q=Mechanical+Design+of+Robot+Lower+Body+Based+on+Four-Bar+Linkage+Structure+for+Energy+Efficient+Bipedal+Walking)
16. [[New three DOF ankle mechanism for humanoid robotic application - Modeling, design and realization|3-DoF ankle]] · Alfayad et al. · 2009 · IROS · [Scholar](https://scholar.google.com/scholar?q=New+Three+DoF+Ankle+Mechanism+for+Humanoid+Robotic+Application+Modeling+Design+and+Realization)
17. [[Type Synthesis of Symmetrical Lower-Mobility Parallel Mechanisms Using the Constraint-Synthesis Method|Constraint-synthesis method]] · Huang & Li · 2003 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Type+Synthesis+of+Symmetrical+Lower-Mobility+Parallel+Mechanisms+Using+the+Constraint-Synthesis+Method)
18. [[Kinematic Geometry of Mechanisms|Kinematic Geometry of Mechanisms]] · Hunt · 1978 · Oxford University Press · [Scholar](https://scholar.google.com/scholar?q=Hunt+Kinematic+Geometry+of+Mechanisms)
19. [[A Mathematical Introduction to Robotic Manipulation|Mathematical Introduction to Robotic Manipulation]] · Murray et al. · 1994 · CRC Press · [Scholar](https://scholar.google.com/scholar?q=A+Mathematical+Introduction+to+Robotic+Manipulation)
20. [[Mobility of Overconstrained Parallel Mechanisms|Overconstrained mobility]] · Dai et al. · 2006 · ASME J. Mech. Des. · [Scholar](https://scholar.google.com/scholar?q=Mobility+of+Overconstrained+Parallel+Mechanisms)
21. [[Overview of Gait Synthesis for the Humanoid COMAN|COMAN gait synthesis]] · Zhou et al. · 2017 · J. Bionic Eng. · [Scholar](https://scholar.google.com/scholar?q=Overview+of+Gait+Synthesis+for+the+Humanoid+COMAN)
22. [[A Generic Optimization-Based Framework for Reactive Collision Avoidance in Bipedal Locomotion|Reactive collision avoidance]] · Zhou et al. · 2016 · CASE · [Scholar](https://scholar.google.com/scholar?q=A+Generic+Optimization-Based+Framework+for+Reactive+Collision+Avoidance+in+Bipedal+Locomotion)
23. [[Direct Kinematic Analysis of a 3-PRS Parallel Mechanism|3-PRS kinematics]] · Tsai et al. · 2003 · Mechanism and Machine Theory · [Scholar](https://scholar.google.com/scholar?q=Direct+Kinematic+Analysis+of+a+3-PRS+Parallel+Mechanism)

## See also

- [[A Framework for Optimal Ankle Design of Humanoid Robots|Optimal Ankle Design Framework]] · optimizes the geometry of this same RSU-type ankle
- [[Control of Humanoid Robots with Parallel Mechanisms using Differential Actuation Models|Differential actuation control]] · using such ankle models in TO and RL
