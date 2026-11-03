---
title: "Tello Leg: The Study of Design Principles and Metrics for Dynamic Humanoid Robots"
aliases:
  - Tello Leg
  - Centroidal Inertia Isotropy
authors:
  - "[[Youngwoo Sim]]"
  - "[[Joao Ramos]]"
corresponding: "[[Youngwoo Sim]]"
institutions:
  - "[[University of Illinois Urbana-Champaign]]"
labs: []
countries:
  - USA
year: 2022
date: 2022-07-04
venue: IEEE Robotics and Automation Letters 2022 (Vol. 7, No. 4)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1109/LRA.2022.3188122
pdf: https://doi.org/10.1109/LRA.2022.3188122
code: "No official repo (hardware paper), CII details at https://t.ly/Of7P (checked 2026-11-02)"
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/leg-design
  - topic/hardware-design
  - topic/actuators
  - topic/parallel-mechanisms
  - topic/benchmark
  - topic/legged-locomotion
  - robot/tello
  - eval/real-robot
---

# Tello Leg

> [!info] Quick view
> **Authors:** [[Youngwoo Sim]] (corresponding), [[Joao Ramos]]
> **From:** [[University of Illinois Urbana-Champaign]]
> **Published:** IEEE RA-L · Jul 2022 · journal (open access)
> **Keywords:** cooperative actuation, differential actuation, reflected inertia, proximal actuation, Centroidal Inertia Isotropy (CII), Tello humanoid, jumping
> **Links:** [DOI](https://doi.org/10.1109/LRA.2022.3188122) · [CII details](https://t.ly/Of7P)
> **One line:** a 5-DoF humanoid leg with all motors at the hip, paired in differentials so two motors share load on whichever joint needs it, plus a new metric (CII) for how much limb mass breaks centroidal models

## Summary

The paper gives two ideas. First, Cooperative Actuation (CA): instead of raising the gear ratio (torque ∝ N but reflected inertia ∝ N²), combine two motors in a differential to drive two joints that rarely peak at the same time, like knee and ankle in walking. Then each joint can get the torque of both motors with half the gear ratio, so the reflected inertia is half of an equivalent serial design. The cost is that actuators may need up to twice the joint speed. Tello uses CA for knee and ankle (spur gear differential, hamstring-like linkage to the knee, 4-bar to the ankle) and for hip abduction and flexion (non-planar 4-bars), plus direct hip yaw, with all five quasi-direct-drive motors near the hip.

Second, Centroidal Inertia Isotropy (CII): how much the robot's rotational centroidal inertia changes when the joints move, CII = |det(I_G(q)⁻¹ I_G(q0) − I)|, evaluated over a grid of hip angles. Massless limbs give zero. Its range (rCII) separates robot families: proximally actuated robots (Mini Cheetah, Cassie, Tello, Atlas DRC) vs collocated-actuator humanoids (Hubo, Tello with collocated motors), and humanoids vs quadrupeds. In a boom-mounted jump, the leg (6.9 kg setup) jumps 171 mm with −20 Nm at the knee from two 10 Nm-limited motors. Honestly the hardware result is one jump on a boom, and they admit Tello's rCII is only slightly better than Cassie's despite a much more complex transmission.

## How it works

- Differential topology Jacobian: q1 = (ψ1+ψ2)/2N_d, q2 = (ψ1−ψ2)/2N_d, torque polytope rotated 45° so it covers single-joint torque peaks with half the gearing
- Joint-space reflected inertia 2N0² for differential vs 4N0² for serial at equal peak joint torque
- Knee-ankle CA via spur gear differential (γ = 1/2), hip CA via non-planar 4-bars approximated by fifth-order polynomials (β = 0.465)
- CII computed from the rotational part of the centroidal composite rigid body inertia, momentum conservation between nominal and test poses
- rCII = max − min CII over 50x50 grid of hip abduction [−15°, 50°] and flexion [−60°, 0°]
- Jump controller: state machine (ground impedance, thrust with feedforward Bézier force, aerial joint PD), 1 kHz, motor torque capped at 10 Nm

## Results that matter

- Knee torque of about −20 Nm commanded from two motors capped at 10 Nm each
- Jump height 171 mm, thrust 220 ms, flight 275 ms
- Actuator speeds [−29.6, 34.7] rad/s vs knee-ankle joint speeds [−19.5, 26.7] rad/s, the 2x speed penalty did not appear in the jump
- rCII clearly separates proximal vs collocated actuation and humanoids vs quadrupeds, Tello rCII close to Cassie
- Minimum CII poses are crouched, maximum CII poses are fully extended

## Interesting stuff

- CII is a nice, simple, model-agnostic number for "how much do limbs matter for the dynamics", and it naturally grows when limbs are extended
- CA is the same idea as the TUlip differential ankle and modern humanoid parallel ankles, but formalized with polytopes
- Only one boom jump, no walking, no measured torque (commanded torque assumed equal to exerted)
- The mechanical complexity limits ankle range, as they admit

## Useful for me

- **Apply.** CII is directly useful for a morphing humanoid: compute it for each limb-length configuration to see how much the body dynamics change with size, and so how hard the control problem gets. Longer limbs should have higher CII.
- **Apply.** Cooperative actuation is a good principle if extra actuators are needed for length change: pair motors so they share load between joints that do not peak together.
- **As prior work.** Strong reference for proximal actuation and reflected inertia arguments in humanoid leg design.
- **Watch out.** Differential pairs need up to 2x motor speed, so velocity limits in simulation should be in actuator space, not only joint space.

## Key prior work

- [[Proprioceptive actuator design in the MIT Cheetah - Impact mitigation and high-bandwidth physical interaction for dynamic legged robots|MIT Cheetah actuator]] · proprioceptive actuation and impact mitigation factor
- [[Centroidal momentum matrix of a humanoid robot - Structure and properties|Centroidal momentum matrix]] · the inertia CII is built on
- [[Actuator design for high force proprioceptive control in fast legged locomotion|High-force proprioceptive actuator]] · large gap radius low gearing principle
- [[Mini Cheetah - A Platform for Pushing the Limits of Dynamic Quadruped Control|Mini Cheetah]] · reference proximal-actuation robot and actuator
- [[The dynamic effect of mechanical losses of transmissions on the equation of motion of legged robots|Transmission losses]] · their own prior work on transmissions
- [[A robot design method for weight saving aimed at dynamic motions - Design of humanoid JAXON3-P and realization of jump motions|JAXON3-P]] · another design index for dynamic humanoids

## References

1. [[Actuator design for high force proprioceptive control in fast legged locomotion|High-force proprioceptive actuator]] · Seok et al. · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=Actuator+design+for+high+force+proprioceptive+control+in+fast+legged+locomotion)
2. [[Anthropomorphic low-inertia high-stiffness manipulator for high-speed safe interaction|LIMS manipulator]] · Kim · 2017 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Anthropomorphic+low-inertia+high-stiffness+manipulator+for+high-speed+safe+interaction)
3. [[The dynamic effect of mechanical losses of transmissions on the equation of motion of legged robots|Transmission losses]] · Sim & Ramos · 2021 · ICRA · [Scholar](https://scholar.google.com/scholar?q=The+dynamic+effect+of+mechanical+losses+of+transmissions+on+the+equation+of+motion+of+legged+robots)
4. [[Dynamics modeling of gear transmissions with asymmetric load-dependent friction|Asymmetric gear friction]] · Kikuuwe · 2022 · TechRxiv · [Scholar](https://scholar.google.com/scholar?q=Dynamics+modeling+of+gear+transmissions+with+asymmetric+load-dependent+friction)
5. [[Design of a weight-compensated and coupled tendon-driven articulated long-reach manipulator|Coupled tendon long-reach arm]] · Horigome et al. · 2016 · SII · [Scholar](https://scholar.google.com/scholar?q=Design+of+a+weight-compensated+and+coupled+tendon-driven+articulated+long-reach+manipulator)
6. [[Proprioceptive actuator design in the MIT Cheetah - Impact mitigation and high-bandwidth physical interaction for dynamic legged robots|MIT Cheetah actuator]] · Wensing et al. · 2017 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Proprioceptive+actuator+design+in+the+MIT+cheetah+Impact+mitigation+and+high-bandwidth+physical+interaction+for+dynamic+legged+robots)
7. [[A robot design method for weight saving aimed at dynamic motions - Design of humanoid JAXON3-P and realization of jump motions|JAXON3-P]] · Kojima et al. · 2019 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=A+robot+design+method+for+weight+saving+aimed+at+dynamic+motions+Design+of+humanoid+JAXON3-P+and+realization+of+jump+motions)
8. [[Centroidal momentum matrix of a humanoid robot - Structure and properties|Centroidal momentum matrix]] · Orin & Goswami · 2008 · IROS · [Scholar](https://scholar.google.com/scholar?q=Centroidal+momentum+matrix+of+a+humanoid+robot+Structure+and+properties)
9. [[Fast online trajectory optimization for the bipedal robot Cassie|Cassie fast TO]] · Apgar et al. · 2018 · RSS · [Scholar](https://scholar.google.com/scholar?q=Fast+online+trajectory+optimization+for+the+bipedal+robot+cassie)
10. [[Global position control on underactuated bipedal robots - Step-to-step dynamics approximation for step planning|Step-to-step dynamics]] · Xiong et al. · 2021 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Global+position+control+on+underactuated+bipedal+robots+Step-to-step+dynamics+approximation+for+step+planning)
11. [[Terrain-aware foot placement for bipedal locomotion combining model predictive control, virtual constraints, and the ALIP|Terrain-aware ALIP]] · Gibson et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2109.14862)
12. [[Dynamic walking in a humanoid robot based on a 3D actuated dual-SLIP model|3D dual-SLIP]] · Liu et al. · 2015 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Dynamic+walking+in+a+humanoid+robot+based+on+a+3D+actuated+dual-slip+model)
13. [[Dynamic Locomotion in the MIT Cheetah 3 Through Convex Model-Predictive Control|Cheetah 3 convex MPC]] · Di Carlo et al. · 2018 · IROS · [Scholar](https://scholar.google.com/scholar?q=Dynamic+locomotion+in+the+MIT+cheetah+3+through+convex+model-predictive+control)
14. [[Policy-regularized model predictive control to stabilize diverse quadrupedal gaits for the MIT cheetah|Policy-regularized MPC]] · Bledt et al. · 2017 · IROS · [Scholar](https://scholar.google.com/scholar?q=Policy-regularized+model+predictive+control+to+stabilize+diverse+quadrupedal+gaits+for+the+MIT+cheetah)
15. [[Multi-contact walking pattern generation based on model preview control of 3D COM accelerations|Multi-contact preview control]] · Caron & Kheddar · 2016 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Multi-contact+walking+pattern+generation+based+on+model+preview+control+of+3D+com+accelerations)
16. [[MPC-based locomotion control of bipedal robots with line-feet contact using centroidal dynamics|Line-feet centroidal MPC]] · García et al. · 2021 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=MPC-based+locomotion+control+of+bipedal+robots+with+line-feet+contact+using+centroidal+dynamics)
17. [[Highly dynamic quadruped locomotion via whole-body impulse control and model predictive control|WBIC + MPC]] · Kim et al. · 2019 · arXiv · [arXiv](https://arxiv.org/abs/1909.06586)
18. [[An evaluation of the approaches of optimization models in the prediction of muscle forces during human gait|Muscle force prediction]] · Patriarco et al. · 1981 · Journal of Biomechanics · [Link](https://www.sciencedirect.com/science/article/pii/0021929081900014)
19. [[Are joint torques the holy grail of human gait analysis|Joint torques in gait analysis]] · Vaughan · 1996 · Human Movement Science · [Link](https://www.sciencedirect.com/science/article/pii/0167945796000097)
20. [[Energetic and peak power advantages of series elastic actuators in an actuated prosthetic leg for walking and running|SEA prosthetic leg]] · Grimmer et al. · 2014 · Actuators · [Link](https://www.mdpi.com/2076-0825/3/1/1)
21. [[Planning walking patterns for a biped robot|Biped walking pattern planning]] · Huang et al. · 2001 · IEEE T-RA · [Scholar](https://scholar.google.com/scholar?q=Planning+walking+patterns+for+a+biped+robot)
22. [[An optimal control approach to reconstruct human gait dynamics from kinematic data|Human gait reconstruction]] · Felis et al. · 2015 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=An+optimal+control+approach+to+reconstruct+human+gait+dynamics+from+kinematic+data)
23. [[A direct method for trajectory optimization of rigid bodies through contact|Contact-implicit TO]] · Posa et al. · 2014 · IJRR · [DOI](https://doi.org/10.1177/0278364913506757)
24. [[Ensemble-CIO - Full-body dynamic motion planning that transfers to physical humanoids|Ensemble-CIO]] · Mordatch et al. · 2015 · IROS · [Scholar](https://scholar.google.com/scholar?q=Ensemble-CIO+Full-body+dynamic+motion+planning+that+transfers+to+physical+humanoids)
25. [[Whole-body motion planning with centroidal dynamics and full kinematics|Centroidal + full kinematics planning]] · Dai et al. · 2014 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Whole-body+motion+planning+with+centroidal+dynamics+and+full+kinematics)
26. [[Structured contact force optimization for kino-dynamic motion generation|Kino-dynamic force optimization]] · Herzog et al. · 2016 · IROS · [Scholar](https://scholar.google.com/scholar?q=Structured+contact+force+optimization+for+kino-dynamic+motion+generation)
27. [[A Unified Model with Inertia Shaping for Highly Dynamic Jumps of Legged Robots|Inertia shaping jumps]] · Wang et al. · 2021 · arXiv · [arXiv](https://arxiv.org/abs/2109.04581)
28. [[Mini Cheetah - A Platform for Pushing the Limits of Dynamic Quadruped Control|Mini Cheetah]] · Katz et al. · 2019 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Mini+cheetah+A+platform+for+pushing+the+limits+of+dynamic+quadruped+control)

## See also

- [[DecARt Leg - Design and Evaluation of a Novel Humanoid Robot Leg with Decoupled Actuation for Agile Locomotion|DecARt Leg]] · another leg-level metric (FAST) that compares Tello-style principles
- [[New Ankle Actuation Mechanism for a Humanoid Robot|TUlip Ankle]] · earlier differential ankle with series elasticity
