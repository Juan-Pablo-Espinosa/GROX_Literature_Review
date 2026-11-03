---
title: "Design, Control, and Validation of a Symmetrical Hip and Straight-Legged Vertically-Compliant Bipedal Robot"
aliases:
  - L03 Straight-Legged Biped
  - L03
authors:
  - "[[Jun Tang]]"
  - "[[Yudi Zhu]]"
  - "[[Wencong Gan]]"
  - "[[Haiming Mou]]"
  - "[[Jie Leng]]"
  - "[[Qingdu Li]]"
  - "[[Zhiqiang Yu]]"
  - "[[Jianwei Zhang]]"
corresponding: "[[Qingdu Li]]"
institutions:
  - "[[University of Shanghai for Science and Technology]]"
  - "[[Beijing Institute of Technology]]"
  - "[[University of Hamburg]]"
labs:
  - "[[USST Institute of Machine Intelligence]]"
  - "[[BIT Key Laboratory of Biomimetic Robots and Systems]]"
countries:
  - China
  - Germany
year: 2023
date: 2023-08-01
venue: Biomimetics 2023 (Vol. 8, 340)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.3390/biomimetics8040340
pdf: https://www.mdpi.com/2313-7673/8/4/340/pdf
code: "No official repo (checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/legged-locomotion
  - topic/hardware-design
  - topic/leg-design
  - topic/variable-length-limbs
  - topic/actuators
  - method/mpc
  - sim/mujoco
  - eval/real-robot
---

# L03 Straight-Legged Biped

> [!info] Quick view
> **Authors:** [[Jun Tang]], [[Yudi Zhu]], [[Wencong Gan]], [[Haiming Mou]], [[Jie Leng]], [[Qingdu Li]] (corresponding), [[Zhiqiang Yu]], [[Jianwei Zhang]]
> **From:** [[University of Shanghai for Science and Technology]] · [[USST Institute of Machine Intelligence]] · [[Beijing Institute of Technology]] · [[BIT Key Laboratory of Biomimetic Robots and Systems]] · [[University of Hamburg]]
> **Published:** Biomimetics (MDPI) · Aug 2023 · journal
> **Keywords:** underactuated biped, kneeless telescopic leg, symmetrical hip, four-bar lateral hip, DCM, LIP model, five actuators
> **Links:** [DOI](https://doi.org/10.3390/biomimetics8040340) · [PDF](https://www.mdpi.com/2313-7673/8/4/340/pdf)
> **One line:** a 5.5 kg, 5-motor biped with kneeless telescopic legs and a symmetric four-bar lateral hip that walks forward and sideways with a DCM planner

## Summary

The question is how few actuators a biped needs to walk in all directions. L03 uses five: one hip yaw motor that drives a ball screw and a symmetric four-bar so both legs split laterally together (±10°), two hip pitch motors, and two leg slide motors that extend each straight kneeless leg through a two-stage timing belt (leg length 580 to 680 mm). Mass is concentrated in the body, each leg is 680 g with very low inertia, so the robot behaves very close to a linear inverted pendulum. Since the leg is a direct belt-driven prismatic joint, foot contact can be detected from motor current without force sensors.

Control is a classic DCM footstep and CoM planner with an invariant Kalman filter on the IMU. In MuJoCo and on the real robot it walks forward and sideways at 0.3 m/s with footstep errors within 2 cm, and it recovers from qualitative ball impacts and a foot slip. Honestly the experiments are qualitative and the "omnidirectional" claim is weak: without a hip roll the robot cannot turn in place in the usual way, and there is no turning experiment. It is a nice minimal research platform, not a performance paper.

## How it works

- Lateral hip: symmetric four-bar with the ball screw slide as one link, 25/35/45 mm links, about 90% linearity in ±10°, about 155°/s peak
- Hip pitch: Maxon RE35 with harmonic drive, ±90°, 540°/s, 56 Nm peak
- Leg slide: two-stage timing belt (6x reduction then a closed belt along the thigh), linear rail, carbon tubes, 1.5 m/s and 1120 N peak, 0.04 kg·m² leg inertia
- Kinematics solved geometrically for virtual legs, giving lateral angle, swing angles and leg lengths from footholds and CoM
- DCM planning: ξ = x + ẋ/ω, backward recursion from the final footstep, closed form offsets for forward and lateral walking
- IMU plus kinematics give the CoM, an invariant Kalman filter fuses the data, motor current used for contact detection

## Results that matter

- Total 5.5 kg, 0.92 m standing, legs 680 g each, CoM height fixed at 0.76 m
- Forward and lateral walking at 0.3 m/s in MuJoCo and on hardware, footstep tracking error within 2 cm
- Recovers from a 885 g ball hitting the leg, a 260 g ball hitting the torso, and stepping on a 10 mm tube, all qualitative
- No turning, no speed limits, no energy measurement reported

## Interesting stuff

- Contact detection from current through a stiff prismatic transmission is a neat side effect of the telescopic leg
- Telescopic straight legs make the LIP assumption almost true, which is why simple DCM control works
- The lateral hip couples both legs, so you cannot place the two feet laterally independently
- Some numbers in the table look off (lateral hip peak torque listed as 195 Nm on tiny motors), likely a missing decimal
- The authors are the same group as the LR04 telescopic straight-leg paper

## Useful for me

- **As prior work.** Another biped where leg length is an actuated variable with a direct prismatic drive. Useful for the design space of telescopic legs.
- **Apply.** Using actuator current as a contact sensor through a stiff prismatic leg could be used in a policy observation when force sensors are missing.
- **Watch out.** With only 100 mm of stroke this is gait-level length control, not morphology change. The range is tiny compared with a growing limb.

## Key prior work

- [[Design and Control of SLIDER - An Ultra-lightweight, Knee-less, Low-cost Bipedal Walking Robot|SLIDER]] · the kneeless sliding biped this design follows
- [[ATRIAS - Design and validation of a tether-free 3D-capable spring-mass bipedal robot|ATRIAS]] · low inertia leg reference
- [[Passive-dynamic leg design for agile robots|Passive-dynamic leg design]] · why mass should be near the hip
- [[Efficiency analysis of telescopic-legged bipedal robots|Telescopic leg efficiency]] · energy argument for telescopic legs
- [[A simple 2D straight-leg passive dynamic walking model without foot-scuffing problem|Straight-leg passive walker]] · their own passive walking roots
- [[Simultaneous Optimization of ZMP and Footsteps Based on the Analytical Solution of Divergent Component of Motion|DCM footstep optimization]] · the DCM planning basis

## References

1. [[Review and descriptive investigation of the connection between bipedal locomotion and non-prehensile manipulation|Bipedal locomotion and non-prehensile manipulation]] · Farid et al. · 2022 · Annual Reviews in Control · [Scholar](https://scholar.google.com/scholar?q=Review+and+descriptive+investigation+of+the+connection+between+bipedal+locomotion+and+non-prehensile+manipulation)
2. [[Humanoid robotics - History, current state of the art, and challenges|Humanoid robotics history]] · Kawato et al. (as printed) · 2017 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robotics+History+current+state+of+the+art+and+challenges)
3. [[Boston Dynamics Atlas|Atlas]] · Boston Dynamics · 2023 · web page · [Link](https://www.bostondynamics.com/atlas)
4. [[Walking dynamics for an ascending stair biped robot with telescopic legs and impulse thrust|Stair biped with telescopic legs]] · Chen et al. · 2022 · Electronic Research Archive · [Scholar](https://scholar.google.com/scholar?q=Walking+dynamics+for+an+ascending+stair+biped+robot+with+telescopic+legs+and+impulse+thrust)
5. [[Legged mobile robots for challenging terrains|Legged robots for challenging terrains]] · Meng & Song · 2022 · Biomimetic Intelligence and Robotics · [Scholar](https://scholar.google.com/scholar?q=Legged+mobile+robots+for+challenging+terrains)
6. [[Walking dynamics of a semi-passive compass-like robot with impulse thrust|Semi-passive compass robot]] · Zhou et al. · 2023 · Nonlinear Dynamics · [Scholar](https://scholar.google.com/scholar?q=Walking+dynamics+of+a+semi-passive+compass-like+robot+with+impulse+thrust)
7. [[Further analysis of the passive dynamics of the compass biped walker and control of chaos via two trajectory tracking approaches|Compass walker chaos control]] · Added et al. · 2021 · Complexity · [Scholar](https://scholar.google.com/scholar?q=Further+analysis+of+the+passive+dynamics+of+the+compass+biped+walker+and+control+of+chaos+via+two+trajectory+tracking+approaches)
8. [[Trajectory tracking-based control of the chaotic behavior in the passive bipedal compass-type robot|Compass robot trajectory tracking]] · Added et al. · 2022 · Eur. Phys. J. Spec. Top. · [Scholar](https://scholar.google.com/scholar?q=Trajectory+tracking-based+control+of+the+chaotic+behavior+in+the+passive+bipedal+compass-type+robot)
9. [[Bipedal Robots - Modeling, Design and Walking Synthesis|Bipedal Robots book]] · Chevallereau et al. · 2009 · Wiley · [Scholar](https://scholar.google.com/scholar?q=Bipedal+Robots+Modeling+Design+and+Walking+Synthesis)
10. [[Feedback Control of Dynamic Bipedal Robot Locomotion|Feedback control of bipedal locomotion]] · Aldrich (review) · 2008 · IEEE TAC · [Scholar](https://scholar.google.com/scholar?q=Feedback+Control+of+Dynamic+Bipedal+Robot+Locomotion)
11. [[Feedback control of a Cassie bipedal robot - Walking, standing, and riding a segway|Cassie feedback control]] · Gong et al. · 2019 · ACC · [Scholar](https://scholar.google.com/scholar?q=Feedback+control+of+a+cassie+bipedal+robot+Walking+standing+and+riding+a+segway)
12. [[Variable Horizon MPC with Swing Foot Dynamics for Bipedal Walking Control|Variable horizon MPC]] · Daneshmand et al. · 2021 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Variable+Horizon+MPC+with+Swing+Foot+Dynamics+for+Bipedal+Walking+Control)
13. [[Design and Control of SLIDER - An Ultra-lightweight, Knee-less, Low-cost Bipedal Walking Robot|SLIDER]] · Wang et al. · 2020 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+and+Control+of+SLIDER+An+Ultra-lightweight+Knee-less+Low-cost+Bipedal+Walking+Robot)
14. [[Design and control of Ranger - an energy-efficient, dynamic walking robot|Ranger]] · Bhounsule et al. · 2012 · CLAWAR · [Scholar](https://scholar.google.com/scholar?q=Design+and+control+of+ranger+An+energy-efficient+dynamic+walking+robot)
15. [[A simple 2D straight-leg passive dynamic walking model without foot-scuffing problem|Straight-leg passive walker]] · Li et al. · 2016 · IROS · [Scholar](https://scholar.google.com/scholar?q=A+simple+2D+straight-leg+passive+dynamic+walking+model+without+foot-scuffing+problem)
16. [[Mechanics of humanoid robot|Mechanics of humanoid robot]] · Hashimoto · 2020 · Advanced Robotics · [DOI](https://doi.org/10.1080/01691864.2020.1813624)
17. [[Design and Realization of a Humanoid Robot for Fast and Autonomous Bipedal Locomotion|Lohmeier thesis]] · Lohmeier · 2010 · PhD thesis, TU Munich · [Scholar](https://scholar.google.com/scholar?q=Design+and+Realization+of+a+Humanoid+Robot+for+Fast+and+Autonomous+Bipedal+Locomotion)
18. [[Efficiency analysis of telescopic-legged bipedal robots|Telescopic leg efficiency]] · Harata et al. · 2018 · Artificial Life and Robotics · [Scholar](https://scholar.google.com/scholar?q=Efficiency+analysis+of+telescopic-legged+bipedal+robots)
19. [[On the hardware design and control architecture of the humanoid robot Kangaroo|Kangaroo]] · Roig et al. · 2022 · ICRA Workshop on Legged Robots · [Scholar](https://scholar.google.com/scholar?q=On+the+Hardware+Design+and+Control+Architecture+of+the+Humanoid+Robot+Kangaroo)
20. [[Causeim|Causeim]] · Causeim · 2023 · web page · [Link](https://sites.google.com/view/causeim/home)
21. [[Experimental results for 3D bipedal robot walking based on systematic optimization of virtual constraints|3D virtual constraints walking]] · Buss et al. · 2016 · ACC · [Scholar](https://scholar.google.com/scholar?q=Experimental+results+for+3D+bipedal+robot+walking+based+on+systematic+optimization+of+virtual+constraints)
22. [[Passive-dynamic leg design for agile robots|Passive-dynamic leg design]] · Abate et al. · 2015 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Passive-dynamic+leg+design+for+agile+robots)
23. [[ATRIAS - Design and validation of a tether-free 3D-capable spring-mass bipedal robot|ATRIAS]] · Hubicki et al. · 2016 · IJRR · [Scholar](https://scholar.google.com/scholar?q=ATRIAS+Design+and+validation+of+a+tether-free+3D-capable+spring-mass+bipedal+robot)
24. [[Biped walking pattern generation by using preview control of zero-moment point|ZMP preview control]] · Kajita et al. · 2003 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Biped+walking+pattern+generation+by+using+preview+control+of+zero-moment+point)
25. [[Simultaneous Optimization of ZMP and Footsteps Based on the Analytical Solution of Divergent Component of Motion|DCM footstep optimization]] · Kamioka et al. · 2018 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Simultaneous+Optimization+of+ZMP+and+Footsteps+Based+on+the+Analytical+Solution+of+Divergent+Component+of+Motion)
26. [[Online Learning of Centroidal Angular Momentum towards Enhancing DCM-based Locomotion|Learned CAM for DCM]] · Schuller et al. · 2022 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Online+Learning+of+Centroidal+Angular+Momentum+towards+Enhancing+DCM-based+Locomotion)
27. [[A Unified Model with Inertia Shaping for Highly Dynamic Jumps of Legged Robots|Inertia shaping jumps]] · Wang et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2109.04581)
28. [[Robust Walking for Humanoid Robot Based on Divergent Component of Motion|DCM robust walking]] · Zhang et al. · 2022 · Micromachines · [Scholar](https://scholar.google.com/scholar?q=Robust+Walking+for+Humanoid+Robot+Based+on+Divergent+Component+of+Motion)

## See also

- [[High Dynamic Bipedal Robot with Underactuated Telescopic Straight Legs|Telescopic straight legs biped]] · the follow-up from the same group with springs and jumping
