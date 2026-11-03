---
title: "Time-Division Multiplexing Actuation in Tendon-Driven Arms: Lightweight Design and Fault Tolerance"
aliases:
  - MuxArm
  - TDMA
authors:
  - "[[Shoujie Li]]"
  - "[[Changqing Guo]]"
  - "[[Jianle Xu]]"
  - "[[Hong Luo]]"
  - "[[Xueqian Wang]]"
  - "[[Wenbo Ding]]"
  - "[[Bin Liang]]"
corresponding: "[[Xueqian Wang]]"
institutions:
  - "[[Tsinghua University]]"
  - "[[Nanyang Technological University]]"
labs:
  - "[[Tsinghua Shenzhen International Graduate School]]"
countries:
  - China
  - Singapore
year: 2026
date: ""
venue: IEEE/ASME Transactions on Mechatronics 2026 (accepted, early access)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1109/TMECH.2026.3694587
pdf: https://doi.org/10.1109/TMECH.2026.3694587
code: https://github.com/guo-cq/MuxArm
status: to-read
relevance: low
added: 2026-11-02
tags:
  - paper
  - topic/manipulation
  - topic/hardware-design
  - topic/actuators
  - topic/continuum-robots
  - method/trajectory-optimization
  - robot/muxarm
  - eval/real-robot
---

# MuxArm

> [!info] Quick view
> **Authors:** [[Shoujie Li]], [[Changqing Guo]], [[Jianle Xu]], [[Hong Luo]], [[Xueqian Wang]] (corresponding), [[Wenbo Ding]] (corresponding), [[Bin Liang]]
> **From:** [[Tsinghua University]] · [[Tsinghua Shenzhen International Graduate School]] · [[Nanyang Technological University]]
> **Published:** IEEE/ASME TMECH · 2026 (accepted May 2026) · journal
> **Keywords:** time-division multiplexing actuation, tendon-driven continuum arm, electromagnetic clutch, worm gear self-locking, fault tolerance, BeamStep planning, space robotics
> **Links:** [DOI](https://doi.org/10.1109/TMECH.2026.3694587) · [Code (simulator)](https://github.com/guo-cq/MuxArm)
> **One line:** a 2.17 kg, 6-DoF tendon continuum arm where only 3 servos plus 1 selector motor drive 9 tendons by switching through clutches, so it keeps working when servos fail

## Summary

For space arms, mass and fault tolerance matter more than speed. MuxArm uses "time-division multiplexing": an axial motor rotates a carousel of three radial servos to any of nine positions, an electromagnetic clutch (engages in under 0.1 s) couples the servo to one of nine stacked winding wheels through a 60:1 gear train with a self-locking worm gear, and dual encoders (servo and wheel) keep positioning accurate. The arm is three constant-curvature segments of disc-spring backbone (25 discs, 44 passive DoFs), each bent by three tendons. If a servo fails, any remaining servo can reach any tendon, just slower.

Planning is done in tendon space: a mixed-integer problem (which tendons in one layer move each step, how much) with a cost on travel, bending, and selector switches, solved by beam search ("BeamStep"), then a scheduler fuses steps that use the same axial position. Compared with sequential, reverse and greedy baselines, it is faster and has much less overshoot, and with fewer servos the time grows less than proportionally (35% and 40% slower going 3→2→1 servos). With 3, 2 or 1 working servos, target error stays around 4.7 mm (about 1% of length) and 6.6°. It lifts 10 kg at the drive module and costs about 540 USD. Honestly the time-for-mass trade-off is explicit and fine for space, but this is far from legged robots, the relevance for me is the actuator-sharing idea.

## How it works

- Vertically stacked rotational selection: one axial motor (Harmonic Drive HTM4538) carries three radial servos 120° apart through a slip ring
- Normally open solenoid clutches with chamfered plugs, energized only when transmitting torque
- 9 winding wheels, each with worm gear self-locking and a 14-bit magnetic encoder, CAN bus control on STM32
- Kinematics: constant-curvature segments, damped pseudoinverse IK with null-space term and multi-start, analytic tendon-to-configuration mapping
- BeamStep: beam search over discrete tendon sets per layer with step size limits, cost = tendon travel + bending energy + switch count
- Scheduler maps tendons to (axial position, servo), fuses consecutive steps, emits clutch and angle commands

## Results that matter

- 2.17 kg arm, 10 kg drive capacity (payload-to-weight 4.6), 4 motors for 6 DoF and 9 tendons (55% fewer actuators)
- Fault tolerance: mean target RMSE 4.6 mm and 7.2° across 5 trials with 3, 2 or 1 active servos
- About 0.5 mm tendon error per clutch switch from slip, accumulates with more switches
- BeamStep reduces transient tendon load up to 50% vs baselines and suppresses overshoot
- Total module cost about 542 USD, 88% cheaper than nine Dynamixel XM540 servos
- Demos: cluttered grasping and retrieving a ball from an S-shaped pipe on a mobile base

## Interesting stuff

- Treating actuators as shared resources scheduled in time is a nice systems idea, the same spirit as cooperative actuation but in time instead of space
- Self-locking worm gears let unactuated tendons hold position for free, needed for multiplexing
- The axial selector motor is a single point of failure, they argue it is reliable because it is lightly loaded
- Execution is slower by design, not suitable for dynamic tasks

## Useful for me

- **As prior work.** Minor reference for actuator sharing and fault tolerance in limbs.
- **Apply.** For slow DoFs like limb length change, a shared actuator with clutches could drive several length joints in sequence, saving a lot of mass, since length change does not need to be fast or simultaneous.
- **Watch out.** Clutch switching introduces small errors per switch that accumulate, a learned controller would need to observe or estimate them.

## Key prior work

- [[MuxHand - A Cost-Effective and Compact Dexterous Robotic Hand Using Time-Division Multiplexing Mechanism|MuxHand]] · their earlier TDM hand
- [[Design and kinematic modeling of constant curvature continuum robots - A review|Constant curvature review]] · continuum kinematics model
- [[Mechanics Modeling of Tendon-Driven Continuum Manipulators|Tendon continuum mechanics]] · tendon-driven continuum modeling
- [[Tendon-Driven Manipulator Actuated by Magnetorheological Clutches Exhibiting Both High-Power and Soft Motion Capabilities|MR clutch arm]] · clutch-based tendon actuation
- [[Electrostatic brakes enable individual joint control of underactuated, highly articulated robots|Electrostatic brakes]] · alternative joint-selection method
- [[A time division multiplexing inspired lightweight soft exoskeleton for hip and ankle joint assistance|TDM exoskeleton]] · TDM in exoskeletons

## References

1. [[Progress, challenges, and prospects of soft robotics for space applications|Soft robotics for space]] · Zhang et al. · 2023 · Advanced Intelligent Systems · [Scholar](https://scholar.google.com/scholar?q=Progress+challenges+and+prospects+of+soft+robotics+for+space+applications)
2. [[Tendon-driven hyper-redundant manipulators - A review on design solutions|Tendon hyper-redundant review]] · Manara et al. · 2025 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=Tendon-driven+hyper-redundant+manipulators+A+review+on+design+solutions)
3. [[A review of space robotics technologies for on-orbit servicing|On-orbit servicing review]] · Flores-Abad et al. · 2014 · Progress in Aerospace Sciences · [Scholar](https://scholar.google.com/scholar?q=A+review+of+space+robotics+technologies+for+on-orbit+servicing)
4. [[Design of lightweight and extensible tendon-driven continuum robots using origami patterns|Origami tendon continuum]] · Xu et al. · 2021 · RoboSoft · [Scholar](https://scholar.google.com/scholar?q=Design+of+lightweight+and+extensible+tendon-driven+continuum+robots+using+origami+patterns)
5. [[A lightweight modular segment design for tendon-driven continuum robots with pre-programmable stiffness|Modular stiffness segments]] · Dewi et al. · 2024 · RoboSoft · [Scholar](https://scholar.google.com/scholar?q=A+lightweight+modular+segment+design+for+tendon-driven+continuum+robots+with+pre-programmable+stiffness)
6. [[Helical routing - Decoupling segments of tendon-driven continuum robots|Helical routing]] · Russo et al. · 2025 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=Helical+routing+Decoupling+segments+of+tendon-driven+continuum+robots)
7. [[Development of fully tendon-driven lightweight manipulator with constant tendon-length routing|Constant tendon-length routing]] · Sakurai & Katsura · 2024 · AMC · [Scholar](https://scholar.google.com/scholar?q=Development+of+fully+tendon-driven+lightweight+manipulator+with+constant+tendon-length+routing)
8. [[Tendon-Driven Manipulator Actuated by Magnetorheological Clutches Exhibiting Both High-Power and Soft Motion Capabilities|MR clutch arm]] · Viau et al. · 2017 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=Tendon-driven+manipulator+actuated+by+magnetorheological+clutches+exhibiting+both+high-power+and+soft+motion+capabilities)
9. [[Safe & Accurate at Speed with Tendons - A Robot Arm for Exploring Dynamic Motion|Tendon arm at speed]] · Guist et al. · 2024 · RSS · [Scholar](https://scholar.google.com/scholar?q=Safe+%26+accurate+at+speed+with+tendons+A+robot+arm+for+exploring+dynamic+motion)
10. [[On the fail-safe design of tendon-driven manipulators with redundant tendons|Fail-safe redundant tendons]] · Sheu et al. · 2012 · J. Mech. Sci. Technol. · [Scholar](https://scholar.google.com/scholar?q=On+the+fail-safe+design+of+tendon-driven+manipulators+with+redundant+tendons)
11. [[Cable failure tolerant control and planning in a planar reconfigurable cable driven parallel robot|Cable failure tolerance]] · Raman et al. · 2023 · Frontiers in Robotics and AI · [Scholar](https://scholar.google.com/scholar?q=Cable+failure+tolerant+control+and+planning+in+a+planar+reconfigurable+cable+driven+parallel+robot)
12. [[Reconfigurable cable-driven parallel mechanism design - Physical constraints and control|Reconfigurable CDPM]] · Khoshbin et al. · 2024 · Robotica · [Scholar](https://scholar.google.com/scholar?q=Reconfigurable+cable-driven+parallel+mechanism+design+Physical+constraints+and+control)
13. [[Cable routing design and performance evaluation for multi-link cable-driven robots with minimal number of actuating cables|Minimal cable routing]] · Wang et al. · 2019 · IEEE Access · [Scholar](https://scholar.google.com/scholar?q=Cable+routing+design+and+performance+evaluation+for+multi-link+cable-driven+robots+with+minimal+number+of+actuating+cables)
14. [[Cable routing design and performance evaluation for multi-link cable-driven robots with minimal number of actuating cables|Minimal cable routing (duplicate ref)]] · Wang et al. · 2019 · IEEE Access · [Scholar](https://scholar.google.com/scholar?q=Cable+routing+design+and+performance+evaluation+for+multi-link+cable-driven+robots+with+minimal+number+of+actuating+cables)
15. [[A novel underactuated continuum robot with shape memory alloy clutches|SMA clutch continuum robot]] · Bishop et al. · 2022 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=A+novel+underactuated+continuum+robot+with+shape+memory+alloy+clutches)
16. [[An under actuated robotic arm with adjustable stiffness shape memory polymer joints|SMP joint arm]] · Firouzeh et al. · 2015 · ICRA · [Scholar](https://scholar.google.com/scholar?q=An+under+actuated+robotic+arm+with+adjustable+stiffness+shape+memory+polymer+joints)
17. [[A continuum manipulator with phase changing alloy|Phase changing alloy manipulator]] · Alambeigi et al. · 2016 · ICRA · [Scholar](https://scholar.google.com/scholar?q=A+continuum+manipulator+with+phase+changing+alloy)
18. [[New cable-driven continuum robot with only one actuator|One-actuator continuum robot]] · Jiang et al. · 2017 · CIS-RAM · [Scholar](https://scholar.google.com/scholar?q=New+cable-driven+continuum+robot+with+only+one+actuator)
19. [[Multi-joint actuation of robotic arm using a switchable tendon-sheath drive system|Switchable tendon-sheath]] · Zhang et al. · 2025 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Multi-joint+actuation+of+robotic+arm+using+a+switchable+tendon-sheath+drive+system)
20. [[Reconfigurable tendon-driven robots - Eliminating intersegmental coupling via independently lockable joints|Lockable joint tendon robots]] · Lin et al. · 2025 · IEEE/ASME TMECH · [DOI](https://doi.org/10.1109/TMECH.2025.3638772)
21. [[Electrostatic brakes enable individual joint control of underactuated, highly articulated robots|Electrostatic brakes]] · Lancaster et al. · 2024 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Electrostatic+brakes+enable+individual+joint+control+of+underactuated+highly+articulated+robots)
22. [[A time division multiplexing inspired lightweight soft exoskeleton for hip and ankle joint assistance|TDM exoskeleton]] · Ye et al. · 2021 · Micromachines · [Scholar](https://scholar.google.com/scholar?q=A+time+division+multiplexing+inspired+lightweight+soft+exoskeleton+for+hip+and+ankle+joint+assistance)
23. [[Design of a novel surgical robot with rigidity-adjustable joints based on time-division multiplexing actuation|TDM surgical robot]] · Zuo et al. · 2020 · BioRob · [Scholar](https://scholar.google.com/scholar?q=Design+of+a+novel+surgical+robot+with+rigidity-adjustable+joints+based+on+time-division+multiplexing+actuation)
24. [[MuxHand - A Cost-Effective and Compact Dexterous Robotic Hand Using Time-Division Multiplexing Mechanism|MuxHand]] · Xu et al. · 2025 · IROS · [Scholar](https://scholar.google.com/scholar?q=Muxhand+A+cost-effective+and+compact+dexterous+robotic+hand+using+time-division+multiplexing+mechanism)
25. [[Design and kinematic modeling of constant curvature continuum robots - A review|Constant curvature review]] · Webster & Jones · 2010 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Design+and+kinematic+modeling+of+constant+curvature+continuum+robots+A+review)
26. [[Mechanics Modeling of Tendon-Driven Continuum Manipulators|Tendon continuum mechanics]] · Camarillo et al. · 2008 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Mechanics+modeling+of+tendon-driven+continuum+manipulators)
27. [[How to model tendon-driven continuum robots and benchmark modelling performance|Tendon continuum modeling benchmark]] · Rao et al. · 2021 · Frontiers in Robotics and AI · [Scholar](https://scholar.google.com/scholar?q=How+to+model+tendon-driven+continuum+robots+and+benchmark+modelling+performance)
28. [[A tutorial on SE(3) transformation parameterizations and on-manifold optimization|SE(3) tutorial]] · Blanco-Claraco · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2103.15980)
29. [[Best-First Beam Search|Best-first beam search]] · Meister et al. · 2020 · TACL · [Scholar](https://scholar.google.com/scholar?q=Best-first+beam+search+Meister+Vieira+Cotterell)

## See also

- [[Tello Leg - The Study of Design Principles and Metrics for Dynamic Humanoid Robots|Tello Leg]] · actuator sharing in space (differentials) instead of in time
- [[Design and Modeling of an Extensible Soft Robotic Arm|ExtenSA]] · another lightweight continuum arm
