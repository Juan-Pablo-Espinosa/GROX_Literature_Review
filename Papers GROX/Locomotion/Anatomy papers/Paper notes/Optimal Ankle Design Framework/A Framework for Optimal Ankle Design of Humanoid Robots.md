---
title: "A Framework for Optimal Ankle Design of Humanoid Robots"
aliases:
  - Optimal Ankle Design Framework
authors:
  - "[[Guglielmo Cervettini]]"
  - "[[Roberto Mauceri]]"
  - "[[Alex Coppola]]"
  - "[[Fabio Bergonti]]"
  - "[[Luca Fiorio]]"
  - "[[Marco Maggiali]]"
  - "[[Daniele Pucci]]"
corresponding: ""
institutions:
  - "[[Istituto Italiano di Tecnologia]]"
  - "[[University of Manchester]]"
labs:
  - "[[Artificial and Mechanical Intelligence Laboratory]]"
  - "[[iCub Tech Facility]]"
countries:
  - Italy
  - UK
year: 2025
date: 2025-09-30
venue: IEEE-RAS Humanoids 2025
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1109/HUMANOIDS65713.2025.11203090
pdf: https://doi.org/10.1109/HUMANOIDS65713.2025.11203090
code: "No official repo found (checked 2026-11-02)"
related_code: https://github.com/hrntsm/Tunny
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/hardware-design
  - topic/leg-design
  - topic/parallel-mechanisms
  - topic/actuators
  - topic/co-design
  - method/multi-objective-optimization
  - method/evolutionary-algorithms
  - method/trajectory-optimization
  - eval/sim-only
---

# Optimal Ankle Design Framework

> [!info] Quick view
> **Authors:** [[Guglielmo Cervettini]], [[Roberto Mauceri]], [[Alex Coppola]], [[Fabio Bergonti]], [[Luca Fiorio]], [[Marco Maggiali]], [[Daniele Pucci]]
> **From:** [[Istituto Italiano di Tecnologia]] · [[Artificial and Mechanical Intelligence Laboratory]] · [[iCub Tech Facility]] · [[University of Manchester]]
> **Published:** IEEE-RAS Humanoids 2025 · 2025 · conference
> **Keywords:** parallel ankle, RSU, SPU, closed-form inverse kinematics, NSGA-II, Pareto front, actuator selection, scalar cost function
> **Links:** [DOI](https://doi.org/10.1109/HUMANOIDS65713.2025.11203090) · [Tunny (optimizer plugin they use)](https://github.com/hrntsm/Tunny)
> **One line:** a task-driven pipeline that optimizes the geometry of SPU and RSU parallel ankles for each candidate actuator and then ranks all designs with one scalar cost

## Summary

Almost every new humanoid (G1, Optimus, Digit, Adam) uses a parallel ankle now, but there is no systematic way to choose between architectures and set the geometry, people basically iterate in CAD. This paper gives a two stage framework. First, for each actuator and architecture pair (SPU with linear actuators, RSU with rotary actuators) they run NSGA-II over the geometric parameters to minimize peak actuator force and peak velocity over reference tasks (walking, 20% ramp, 20 cm step). Then all Pareto solutions are ranked with one scalar cost made of seven normalized metrics, like ankle speed, torque, backdrive torque, manipulability, compactness, mass and CoM height.

The nicest contribution is a reparameterization of the RSU so that any parameter values guarantee the IK has a solution over the whole desired workspace, which makes the optimization much easier. On a redesign of an existing serial ankle the optimized RSU lowers the cost 41% vs the serial one and 14% vs a hand engineered RSU. Honestly the result is only a cost number, nothing was built or tested on hardware, but the method is clean and reusable.

## How it works

- Both mechanisms have a central universal joint between shin and foot, plus two legs (S-P-U or R-S-U) that close the loops, 2 DoF total
- Closed-form IK for both, for RSU it reduces to a sine plus cosine equation with an existence condition |k/ρ| ≤ 1
- Reparameterization: crank length c(γ) = c_min/(1-γ) and rod length as a convex combination of r_min and r_max, so any γ, δ in [0,1] are feasible by construction
- Reference tasks come from experimental walking and trajectory optimized ramp and step motions, which define the operational region of roll [-35°, 35°] and pitch [-70°, 30°]
- Stage 1: NSGA-II (Tunny plugin in Grasshopper plus Python) minimizes peak force and peak velocity for each actuator (Wittenstein AL32 linear, Maxon, MyActuator, Synapticon rotary)
- Stage 2: metrics that vary over the workspace are averaged with a raised cosine weight map, min-max normalized, and summed with weights that you can retune without rerunning the optimization

## Results that matter

- RSU parameterization verified for operational regions from ±15° up to ±150°, the region always stays inside the feasible configuration space
- Synapticon actuator designs have the lowest average cost and the tightest variance
- Best RSU beats best SPU in most metrics, but SPU has less variation across the workspace
- Optimized RSU: 41% lower cost than the original serial ankle and 14% lower than the conventionally engineered RSU
- Every optimized population contains designs strictly better than the hand-made RSU

## Interesting stuff

- Decoupling the expensive optimization from the cheap ranking is smart, you can change priorities (backdrivability vs compactness) without recomputing
- The cost is relative, min-max normalized over the candidate set, so "41% better" depends on which candidates are in the pool
- Collision checking over the whole workspace is not done yet, which matters a lot for real ankles
- No dynamics of the linkage mass on the gait, CoM height is a static metric, and nothing is validated on a real robot
- The robot being redesigned is not named in the paper

## Useful for me

- **Apply.** The task-driven idea is good for any limb design: get reference trajectories from simulated policies, take the union of joint ranges as the required workspace, then optimize the geometry for peak force and speed over those trajectories.
- **As prior work.** Good reference for parallel ankle design and for why parallel ankles help keep distal mass low, which matters even more if the leg length changes.
- **Watch out.** A parallel ankle means joint-space and actuator-space are different, and the Jacobian changes over the workspace. If the policy is trained on a serial ankle model the transfer will suffer, see the paper on differential actuation models.
- **Gap.** The framework assumes one fixed leg geometry. For a body that changes its proportions, the ankle requirements would change with the morphology, nobody optimizes for that range here.

## Key prior work

- [[On the Comprehensive Kinematics Analysis of a Humanoid Parallel Ankle Mechanism|Parallel ankle kinematics]] · the standard kinematic analysis of humanoid parallel ankles
- [[Mechanics of humanoid robot|Mechanics of humanoid robot]] · survey of humanoid mechanical structures they use to motivate
- [[Bipedal humanoid hardware design - A technology review|Humanoid hardware review]] · the other survey of leg and ankle architectures
- [[Humanoid robot LOLA|LOLA]] · classic humanoid with parallel ankle actuation
- [[A fast and elitist multiobjective genetic algorithm - NSGA-II|NSGA-II]] · the optimizer used for the geometry search
- [[Design of KAIST HOUND, a quadruped robot platform for fast and efficient locomotion with mixed-integer nonlinear optimization of a gear train|KAIST HOUND]] · example of optimization-driven leg design
- [[Manipulability of robotic mechanisms|Manipulability]] · where the manipulability ratio metric comes from

## References

1. [[HyperLeg - Biomechanics-inspired high-DoF leg and toe mechanism for highly dynamic motions|HyperLeg]] · Kim et al. · 2024 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Hyperleg+Biomechanics-inspired+high-dof+leg+and+toe+mechanism+for+highly+dynamic+motions)
2. [[On the walking control for humanoid robot based on the kinematic resolution of CoM Jacobian with embedded motion|CoM Jacobian walking]] · Choi et al. · 2006 · ICRA · [Scholar](https://scholar.google.com/scholar?q=On+the+walking+control+for+humanoid+robot+based+on+the+kinematic+resolution+of+com+jacobian+with+embedded+motion)
3. [[Implementation of a large-scale biped robot using serial-parallel hybrid leg mechanism|Serial-parallel biped]] · Gim & Kim · 2022 · UR · [Scholar](https://scholar.google.com/scholar?q=Implementation+of+a+large-scale+biped+robot+using+serial-parallel+hybrid+leg+mechanism)
4. [[On the Comprehensive Kinematics Analysis of a Humanoid Parallel Ankle Mechanism|Parallel ankle kinematics]] · Zhou & Tsagarakis · 2018 · ASME JMR · [Scholar](https://scholar.google.com/scholar?q=On+the+comprehensive+kinematics+analysis+of+a+humanoid+parallel+ankle+mechanism)
5. [[New three DOF ankle mechanism for humanoid robotic application - Modeling, design and realization|3-DoF ankle]] · Alfayad et al. · 2009 · IROS · [Scholar](https://scholar.google.com/scholar?q=New+three+dof+ankle+mechanism+for+humanoid+robotic+application+Modeling+design+and+realization)
6. [[Humanoid robot LOLA|LOLA]] · Lohmeier et al. · 2009 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+LOLA)
7. [[Design, development, and analysis of the lower body of next-generation 3D-printed humanoid research platform - PANDORA|PANDORA]] · Fuge et al. · 2023 · Robotica · [Scholar](https://scholar.google.com/scholar?q=Design+development+and+analysis+of+the+lower+body+of+next-generation+3d-printed+humanoid+research+platform+Pandora)
8. [[On the hardware design and control architecture of the humanoid robot Kangaroo|Kangaroo]] · Roig et al. · 2022 · ICRA Workshop on Legged Robots · [Scholar](https://scholar.google.com/scholar?q=On+the+hardware+design+and+control+architecture+of+the+humanoid+robot+kangaroo)
9. [[StaccaToe - A Single-Leg Robot that Mimics the Human Leg and Toe|StaccaToe]] · Perera et al. · 2024 · IROS · [Scholar](https://scholar.google.com/scholar?q=Staccatoe+A+single-leg+robot+that+mimics+the+human+leg+and+toe)
10. [[Design of KAIST HOUND, a quadruped robot platform for fast and efficient locomotion with mixed-integer nonlinear optimization of a gear train|KAIST HOUND]] · Shin et al. · 2022 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+of+kaist+hound+a+quadruped+robot+platform+for+fast+and+efficient+locomotion+with+mixed-integer+nonlinear+optimization+of+a+gear+train)
11. [[Dynamic modeling and design optimization of a 3-DOF spherical parallel manipulator|3-DoF spherical PM]] · Wu et al. · 2014 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Dynamic+modeling+and+design+optimization+of+a+3-dof+spherical+parallel+manipulator)
12. [[Review of the performance optimization of parallel manipulators|PM optimization review]] · Yang et al. · 2022 · Mechanism and Machine Theory · [Scholar](https://scholar.google.com/scholar?q=Review+of+the+performance+optimization+of+parallel+manipulators)
13. [[Design of the hydraulically actuated, torque-controlled quadruped robot HyQ2Max|HyQ2Max]] · Semini et al. · 2016 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=Design+of+the+hydraulically+actuated+torque-controlled+quadruped+robot+hyq2max)
14. [[Computational design of closed-chain linkages - Respawn algorithm for generative design|Respawn algorithm]] · Ivolga et al. · 2023 · IROS · [Scholar](https://scholar.google.com/scholar?q=Computational+design+of+closed-chain+linkages+Respawn+algorithm+for+generative+design)
15. [[Optimal design of a 4-DOF parallel manipulator - From academia to industry|4-DoF PM design]] · Pierrot et al. · 2009 · IEEE T-RO · [Scholar](https://scholar.google.com/scholar?q=Optimal+design+of+a+4-dof+parallel+manipulator+From+academia+to+industry)
16. [[Mechanics of humanoid robot|Mechanics of humanoid robot]] · Hashimoto · 2020 · Advanced Robotics · [DOI](https://doi.org/10.1080/01691864.2020.1813624)
17. [[Bipedal humanoid hardware design - A technology review|Humanoid hardware review]] · Ficht & Behnke · 2021 · Current Robotics Reports · [Scholar](https://scholar.google.com/scholar?q=Bipedal+humanoid+hardware+design+A+technology+review)
18. [[Theory of Parallel Mechanisms|Theory of Parallel Mechanisms]] · Huang et al. · 2012 · Springer book · [Scholar](https://scholar.google.com/scholar?q=Theory+of+parallel+mechanisms+Huang+Li+Ding)
19. [[Closed-Loop Dynamics|Closed-Loop Dynamics]] · Yamane · 2019 · Springer chapter · [Scholar](https://scholar.google.com/scholar?q=Yamane+Closed-Loop+Dynamics)
20. [[Parallel Kinematics|Parallel Kinematics]] · Liu & Wang · 2014 · Springer Tracts in Mechanical Engineering · [Scholar](https://scholar.google.com/scholar?q=Parallel+kinematics+Liu+Wang+Springer)
21. [[Modern Robotics - Mechanics, Planning, and Control|Modern Robotics]] · Lynch & Park · 2017 · Cambridge University Press · [Scholar](https://scholar.google.com/scholar?q=Modern+Robotics+Mechanics+Planning+and+Control)
22. [[Singularity analysis of closed-loop kinematic chains|Singularity analysis]] · Gosselin & Angeles · 1990 · IEEE T-RA · [Scholar](https://scholar.google.com/scholar?q=Singularity+analysis+of+closed-loop+kinematic+chains)
23. [[Manipulability of robotic mechanisms|Manipulability]] · Yoshikawa · 1985 · IJRR · [Scholar](https://scholar.google.com/scholar?q=Manipulability+of+robotic+mechanisms)
24. [[Chebychev-Grübler-Kutzbach's criterion for mobility calculation of multi-loop mechanisms revisited via theory of linear transformations|Grübler-Kutzbach revisited]] · Gogu · 2005 · European Journal of Mechanics A/Solids · [Scholar](https://scholar.google.com/scholar?q=Chebychev+grubler+kutzbach+criterion+for+mobility+calculation+of+multi-loop+mechanisms+revisited+via+theory+of+linear+transformations)
25. [[A fast and elitist multiobjective genetic algorithm - NSGA-II|NSGA-II]] · Deb et al. · 2002 · IEEE TEVC · [Scholar](https://scholar.google.com/scholar?q=A+fast+and+elitist+multiobjective+genetic+algorithm+NSGA-II)
26. [[Tunny - The Grasshopper optimization tool|Tunny]] · Natsume · 2024 · software · [GitHub](https://github.com/hrntsm/Tunny)

## See also

- [[Control of Humanoid Robots with Parallel Mechanisms using Differential Actuation Models|Differential actuation control]] · what to do on the control side once you have parallel ankles
