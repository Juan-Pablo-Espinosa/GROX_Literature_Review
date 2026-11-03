---
title: "Conceptual Design of a Variable Stiffness Mechanism in a Humanoid Ankle using Parallel Redundant Actuation"
aliases:
  - Ankle VSM
authors:
  - "[[Christoph Stoeffler]]"
  - "[[Shivesh Kumar]]"
  - "[[Heiner Peters]]"
  - "[[Olivier Brüls]]"
  - "[[Andreas Müller]]"
  - "[[Frank Kirchner]]"
corresponding: ""
institutions:
  - "[[German Research Center for Artificial Intelligence]]"
  - "[[University of Liège]]"
  - "[[Johannes Kepler University Linz]]"
labs:
  - "[[DFKI Robotics Innovation Center]]"
  - "[[JKU Institute of Robotics]]"
countries:
  - Germany
  - Belgium
  - Austria
year: 2018
date: 2018-11-06
venue: IEEE-RAS Humanoids 2018
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: https://scholar.google.com/scholar?q=Conceptual+Design+of+a+Variable+Stiffness+Mechanism+in+a+Humanoid+Ankle+using+Parallel+Redundant+Actuation
pdf: ""
code: "No code (conceptual design paper, checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/leg-design
  - topic/parallel-mechanisms
  - topic/variable-stiffness
  - topic/actuators
  - topic/hardware-design
  - eval/sim-only
---

# Ankle VSM

> [!info] Quick view
> **Authors:** [[Christoph Stoeffler]], [[Shivesh Kumar]], [[Heiner Peters]], [[Olivier Brüls]], [[Andreas Müller]], [[Frank Kirchner]]
> **From:** [[German Research Center for Artificial Intelligence]] · [[DFKI Robotics Innovation Center]] · [[University of Liège]] · [[Johannes Kepler University Linz]] · [[JKU Institute of Robotics]]
> **Published:** IEEE-RAS Humanoids 2018 · Nov 2018 · conference
> **Keywords:** variable stiffness mechanism, parallel redundant actuation, variable impedance actuator, pneumatic spring, humanoid ankle, singularity, dexterity
> **Links:** [Scholar](https://scholar.google.com/scholar?q=Conceptual+Design+of+a+Variable+Stiffness+Mechanism+in+a+Humanoid+Ankle+using+Parallel+Redundant+Actuation)
> **One line:** a 2-DoF parallel ankle driven by 3 actuators with nonlinear pneumatic springs in series, so position and stiffness can be controlled at the same time

## Summary

Variable impedance actuators give independent position and stiffness for one joint but need two motors per DoF, which is heavy in a serial leg. The paper proposes a variable stiffness mechanism (VSM): a parallel mechanism with one extra actuator (3 legs for a 2-DoF roll/pitch ankle) and a nonlinear spring in series in every leg. The redundancy removes singularities, and antagonistic internal forces between the legs set the stiffness. They use double-acting pneumatic cylinders as nonlinear springs.

They derive the kinetostatic model, a workspace stiffness matrix with a Hessian term, and four quasi-static forward and inverse models. Compared with the non-redundant 2-leg ankle, singularities disappear from the workspace and dexterity around the neutral pose is better. Feeding human stance phase pitch angle and torque into the inverse model, they find feasible actuator configurations, and they show you can also impose a stiffness profile (1.5x the natural one) and still track the pitch angle. It is fully conceptual and quasi-static, no dynamics and no prototype, so honestly it is more a math framework than a design.

## How it works

- Three actuation legs with actuator plus spring in series, loop closure as distance constraints, redundant Jacobian inverted with Moore-Penrose pseudoinverse
- Pneumatic spring force from the isentropic relation, nearly linear only in ±20% of stroke, so the needed nonlinearity is there
- Stiffness K = Hᵀτ + J†ᵀ (∂τ/∂q) J†, so stiffness depends on configuration and on internal preload
- Forward model I: actuator positions to pose and spring deflections
- Inverse model III: pose and torque (human gait data) to actuator and spring states, non-unique because of internal forces
- Inverse model IV: pose plus desired stiffness to configuration, only possible for pure pitch
- Solved numerically with scipy least squares

## Results that matter

- Singularities completely removed from the workspace with the redundant design, while the non-redundant ankle has singularity curves
- Better dexterity (condition index) around the zero pose, which is where an ankle spends most time
- Moving actuators ±30% changes roll and pitch stiffness in different ways depending on which actuators move
- Human stance data is feasible, stiffness peaks around 75% of stance when ankle torque is highest
- Stiffness can be imposed as input (1.5x) while keeping the pitch trajectory, but then the output torque no longer matches the human torque

## Interesting stuff

- The idea of using the null space of a redundant parallel mechanism to set stiffness is elegant
- The piston stroke had to be reduced to 15 mm to find solutions, a hint that the design space is narrow
- Everything is quasi-static, no impacts, no dynamic gait, no friction of pneumatics
- The inverse models only work for pure pitch, roll is not tested with gait data
- No hardware, and the extra actuator adds mass that the paper does not evaluate

## Useful for me

- **As prior work.** Reference for variable stiffness joints in humanoid legs. A body that adapts its morphology could also adapt stiffness, and this shows one way to do it in a parallel ankle.
- **Watch out.** Stiffness here depends on configuration and preload. A policy controlling such a joint would need the stiffness state in its observation, otherwise the same command gives different responses.
- **Gap.** Stiffness is computed for quasi-static human data only. Learning when to change stiffness during dynamic walking is open.

## Key prior work

- [[Variable impedance actuators - A review|VIA review]] · the main review of variable impedance actuation
- [[Study of the antagonistic stiffness of parallel manipulators with actuation redundancy|Antagonistic stiffness]] · the idea of stiffness from redundant actuation in parallel manipulators
- [[Design and Kinematic Analysis of the Novel Almost Spherical Parallel Mechanism Active Ankle|ASPM ankle]] · the non-redundant DFKI ankle they compare against
- [[Kinematic Analysis of a Novel Parallel 2SPRR+1U Ankle Mechanism in Humanoid Robot|2SPRR+1U ankle]] · the other DFKI ankle mechanism
- [[A Parallel x-y Manipulator with Actuation Redundancy for High-Speed and Active-Stiffness Applications|Redundant x-y manipulator]] · active stiffness with redundancy
- [[Leg Design for a Humanoid Walking Robot|Lola leg]] · parallel actuated humanoid leg reference

## References

1. [[Wiley Encyclopedia of Electrical and Electronics Engineering|Wiley Encyclopedia EEE]] · Abu-Faraj et al. · 2015 · Wiley · [Scholar](https://scholar.google.com/scholar?q=Abu-Faraj+Harris+Smith+Hassani+Wiley+Encyclopedia+of+Electrical+and+Electronics+Engineering)
2. [[Variable impedance actuators - A review|VIA review]] · Vanderborght et al. · 2013 · Robotics and Autonomous Systems · [Scholar](https://scholar.google.com/scholar?q=Variable+impedance+actuators+A+review)
3. [[Soft robotics - From torque feedback-controlled lightweight robots to intrinsically compliant systems|Soft robotics, adaptable compliance]] · Albu-Schäffer et al. · 2008 · IEEE RAM · [Scholar](https://scholar.google.com/scholar?q=Albu-Schaffer+2008+Soft+Robotics+IEEE+Robotics+Automation+Magazine)
4. [[Study of the antagonistic stiffness of parallel manipulators with actuation redundancy|Antagonistic stiffness]] · Chakarov · 2004 · Mechanism and Machine Theory · [Scholar](https://scholar.google.com/scholar?q=Study+of+the+antagonistic+stiffness+of+parallel+manipulators+with+actuation+redundancy)
5. [[Dynamic Whole-Body Mobile Manipulation with a Torque Controlled Humanoid Robot via Impedance Control Laws|Whole-body impedance]] · Dietrich et al. · 2011 · IROS · [Scholar](https://scholar.google.com/scholar?q=Dynamic+Whole-Body+Mobile+Manipulation+with+a+Torque+Controlled+Humanoid+Robot+via+Impedance+Control+Laws)
6. [[A Spatial Impedance Controller for Robotic Manipulation|Spatial impedance controller]] · Fasse & Broenink · 1997 · IEEE T-RA · [Scholar](https://scholar.google.com/scholar?q=A+Spatial+Impedance+Controller+for+Robotic+Manipulation)
7. [[Singularity analysis of closed-loop kinematic chains|Singularity analysis]] · Gosselin & Angeles · 1990 · IEEE T-RA · [Scholar](https://scholar.google.com/scholar?q=Singularity+analysis+of+closed-loop+kinematic+chains)
8. [[Intrinsically Elastic Robots - The Key to Human Like Performance|Intrinsically elastic robots]] · Haddadin et al. · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=Intrinsically+Elastic+Robots+the+Key+to+Human+Like+Performance)
9. [[Compliant Actuator Designs|Compliant actuator designs]] · Van Ham et al. · 2009 · IEEE RAM · [Scholar](https://scholar.google.com/scholar?q=Compliant+Actuator+Designs+Van+Ham)
10. [[Impedance Control - An Approach to Manipulation - Part III - Applications|Impedance control part III]] · Hogan · 1985 · ASME J. Dyn. Syst. · [Scholar](https://scholar.google.com/scholar?q=Impedance+Control+An+Approach+to+Manipulation+Part+III+Applications)
11. [[Body size and lower limb posture during walking in humans|Body size and limb posture]] · Hora et al. · 1997 (as printed; likely PLOS ONE 2017) · Exp Brain Res · [Scholar](https://scholar.google.com/scholar?q=Body+size+and+lower+limb+posture+during+walking+in+humans)
12. [[Series Elastic Actuator - Design, Analysis and Comparison|SEA design]] · Leal Junior et al. · 2016 · INTECH book chapter · [Scholar](https://scholar.google.com/scholar?q=Series+Elastic+Actuator+Design+Analysis+and+Comparison)
13. [[A Singularly Valuable Decomposition - The SVD of a Matrix|SVD tutorial]] · Kalman · 2002 · tech note · [Scholar](https://scholar.google.com/scholar?q=A+Singularly+Valuable+Decomposition+The+SVD+of+a+Matrix)
14. [[Recupera-Reha - Exoskeleton Technology with Integrated Biosignal Analysis for Sensorimotor Rehabilitation|Recupera-Reha]] · Kirchner et al. · 2016 · SmartASSIST · [Scholar](https://scholar.google.com/scholar?q=Recupera-Reha+Exoskeleton+Technology+with+Integrated+Biosignal+Analysis+for+Sensorimotor+Rehabilitation)
15. [[Design and Kinematic Analysis of the Novel Almost Spherical Parallel Mechanism Active Ankle|ASPM ankle]] · Kumar et al. · 2018 · J. Intell. Robot. Syst. · [DOI](https://doi.org/10.1007/s10846-018-0792-x)
16. [[Kinematic Analysis of a Novel Parallel 2SPRR+1U Ankle Mechanism in Humanoid Robot|2SPRR+1U ankle]] · Kumar et al. · 2019 · ARK 2018 · [Scholar](https://scholar.google.com/scholar?q=Kinematic+Analysis+of+a+Novel+Parallel+2SPRR%2B1U+Ankle+Mechanism+in+Humanoid+Robot)
17. [[Leg Design for a Humanoid Walking Robot|Lola leg]] · Lohmeier et al. · 2006 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Leg+Design+for+a+Humanoid+Walking+Robot+Lohmeier)
18. [[Modular joint design for performance enhanced humanoid robot LOLA|LOLA modular joints]] · Lohmeier et al. · 2006 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Modular+joint+design+for+performance+enhanced+humanoid+robot+LOLA)
19. [[A Review of Redundant Parallel Kinematic Mechanisms|Redundant PKM review]] · Luces et al. · 2017 · J. Intell. Robot. Syst. · [Scholar](https://scholar.google.com/scholar?q=A+Review+of+Redundant+Parallel+Kinematic+Mechanisms)
20. [[Jacobian, manipulability, condition number, and accuracy of parallel robots|Parallel robot Jacobian]] · Merlet · 2006 · ASME J. Mech. Des. · [Scholar](https://scholar.google.com/scholar?q=Jacobian+manipulability+condition+number+and+accuracy+of+parallel+robots)
21. [[A generalized inverse for matrices|Penrose inverse]] · Penrose · 1955 · Math. Proc. Cambridge Phil. Soc. · [Scholar](https://scholar.google.com/scholar?q=A+generalized+inverse+for+matrices+Penrose)
22. [[Cartesian Impedance Control for a Variable Stiffness Robot Arm|VS arm Cartesian impedance]] · Petit & Albu-Schäffer · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=Cartesian+Impedance+Control+For+A+Variable+Stiffness+Robot+Arm)
23. [[DELTA - a simple and efficient parallel robot|DELTA]] · Pierrot et al. · 1990 · Robotica · [Scholar](https://scholar.google.com/scholar?q=DELTA+a+simple+and+efficient+parallel+robot)
24. [[Valkyrie - NASA's First Bipedal Humanoid Robot|Valkyrie]] · Radford et al. · 2015 · Journal of Field Robotics · [DOI](http://dx.doi.org/10.1002/rob.21560)
25. [[Active stiffness control of a manipulator in Cartesian coordinates|Active stiffness control]] · Salisbury · 1980 · CDC · [Scholar](https://scholar.google.com/scholar?q=Active+stiffness+control+of+a+manipulator+in+Cartesian+coordinates)
26. [[A Parallel x-y Manipulator with Actuation Redundancy for High-Speed and Active-Stiffness Applications|Redundant x-y manipulator]] · Kock & Schumacher · 1998 · ICRA · [Scholar](https://scholar.google.com/scholar?q=A+Parallel+x-y+Manipulator+with+Actuation+Redundancy+for+High-Speed+and+Active-Stiffness+Applications)
27. [[Conceptual Design of a Variable Stiffness Mechanism using Parallel Redundant Actuation|Stoeffler MSc thesis]] · Stoeffler · 2018 · MSc thesis, University of Liège · [Scholar](https://scholar.google.com/scholar?q=Conceptual+Design+of+a+Variable+Stiffness+Mechanism+using+Parallel+Redundant+Actuation+thesis)
28. [[Towards a Robust Variable Stiffness Actuator|Robust VSA]] · Wolf & Albu-Schäffer · 2013 · IROS · [Scholar](https://scholar.google.com/scholar?q=Towards+a+Robust+Variable+Stiffness+Actuator)
29. [[A New Variable Stiffness Design - Matching Requirements of the Next Robot Generation|New VS design]] · Wolf & Hirzinger · 2008 · ICRA · [Scholar](https://scholar.google.com/scholar?q=A+New+Variable+Stiffness+Design+Matching+Requirements+of+the+Next+Robot+Generation)

## See also

- [[A Framework for Optimal Ankle Design of Humanoid Robots|Optimal Ankle Design Framework]] · newer optimization approach for non-redundant parallel ankles
- [[On the Comprehensive Kinematics Analysis of a Humanoid Parallel Ankle Mechanism|Parallel ankle kinematics]] · kinematics of a standard 2-DoF parallel ankle
