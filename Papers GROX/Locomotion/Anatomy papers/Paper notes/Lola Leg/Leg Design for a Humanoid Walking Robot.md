---
title: "Leg Design for a Humanoid Walking Robot"
aliases:
  - Lola Leg
  - Lola leg
authors:
  - "[[Sebastian Lohmeier]]"
  - "[[Thomas Buschmann]]"
  - "[[Markus Schwienbacher]]"
  - "[[Heinz Ulbrich]]"
  - "[[Friedrich Pfeiffer]]"
corresponding: ""
institutions:
  - "[[Technical University of Munich]]"
labs:
  - "[[TUM Institute of Applied Mechanics]]"
countries:
  - Germany
year: 2006
date: 2006-12-04
venue: IEEE-RAS Humanoids 2006
venue_type: conference
arxiv_category: ""
arxiv_id: ""
url: https://scholar.google.com/scholar?q=Leg+Design+for+a+Humanoid+Walking+Robot+Lohmeier
pdf: ""
code: "No code (hardware paper, checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/humanoids
  - topic/leg-design
  - topic/hardware-design
  - topic/parallel-mechanisms
  - topic/actuators
  - method/topology-optimization
  - robot/lola
  - eval/sim-only
---

# Lola Leg

> [!info] Quick view
> **Authors:** [[Sebastian Lohmeier]], [[Thomas Buschmann]], [[Markus Schwienbacher]], [[Heinz Ulbrich]], [[Friedrich Pfeiffer]]
> **From:** [[Technical University of Munich]] · [[TUM Institute of Applied Mechanics]]
> **Published:** IEEE-RAS Humanoids 2006 · Dec 2006 · conference
> **Keywords:** Lola, 7-DoF leg, active toe, ballscrew knee, parallel ankle, topology optimization, modular joints, SERCOS III
> **Links:** [Scholar](https://scholar.google.com/scholar?q=Leg+Design+for+a+Humanoid+Walking+Robot+Lohmeier)
> **One line:** the design of the Lola humanoid legs, with a ballscrew linear actuator for the knee, a two-ballscrew parallel ankle, active toes and topology-optimized structure, all to get low leg inertia for fast walking

## Summary

Lola is the successor of Johnnie at TU Munich, a 22-DoF, 1.8 m humanoid aimed at fast human-like walking. This paper is the leg design. Each leg has 7 DoF: a spherical 3-DoF hip with the yaw axis tilted 15° for better power distribution, a knee driven by a ballscrew linear actuator mounted on the thigh so the mass sits near the hip, a 2-DoF ankle driven by two identical ballscrew "length variable steering rods" in parallel, and an actively driven toe joint for roll-off across the forefoot. Structural parts (pelvis, thigh, shank) are designed with topology optimization (OptiStruct) using loads from their multibody simulation.

The actuator argument is quantitative: the knee linkage has a nonlinear ratio that matches the torque peak around 50° of knee flexion, and the parallel ankle cuts peak motor torque about 35% compared with a serial ankle with similar reduction, at the cost of 20% higher top speed (only during swing). Ballscrews have about 90% efficiency and almost the same forward and back-driving efficiency, while harmonic drives have about 23% higher back-driving torque. At the time of writing nothing has walked yet, so all results are design studies. Honestly it is an old paper, but a lot of today's humanoid legs (linkage knees, two-rod ankles) follow exactly this logic.

## How it works

- 7-DoF legs: 3-DoF hip with intersecting axes and 15° tilted yaw, knee pitch, 2-DoF ankle, active toe
- Modular joints with frameless Parker Bayside AC motors, custom lightweight harmonic drives, absolute link encoders
- Knee: ballscrew drive with cardan joints acting like a muscle, nonlinear transmission with max torque near 50°
- Ankle: two identical ballscrew rods in parallel, loaded only in tension and compression, motors close to the knee
- Topology optimization: mockup from collision analysis, beam models of the ballscrews, loads from multibody simulation, design for milling
- Electronics: decentralized joint controllers and SERCOS III real-time Ethernet

## Results that matter

- Ankle requirements at 4 km/h: pitch up to 120 Nm and about 7 rad/s, roll up to 50 Nm and 2 rad/s
- Parallel ballscrew ankle: about 35% lower peak motor torque vs serial HD ankle with average reduction N = 80, top speed 20% higher
- Ballscrew efficiency ≥ 90%, forward and reverse efficiency differ by less than 0.5%, HD back-driving torque about 23% higher than starting torque
- Thigh inertia with the ballscrew knee only marginally higher than Johnnie's, despite a more powerful knee

## Interesting stuff

- The nonlinear knee transmission is matched to the torque profile of walking, a good example of designing the linkage for the task
- Hip yaw tilted 15° follows the Johnnie experience on power distribution, consistent with the Zorjan and Hugel studies
- Ballscrews limit range of motion and need care with singularities, the paper says this honestly
- No walking yet, so the claims on fast walking are not validated here

## Useful for me

- **As prior work.** Classic reference for proximal actuators, linkage knees and parallel ankles, the same design patterns modern humanoids use.
- **Apply.** Matching a transmission ratio curve to the task torque profile is a nice idea for a variable geometry limb, the linkage could be chosen so the ratio fits the expected motions over the length range.
- **Watch out.** Nonlinear transmissions make the effective gear ratio pose dependent, which matters for torque limits and impedance in RL training.

## Key prior work

- [[Modular joint design for performance enhanced humanoid robot LOLA|LOLA modular joints]] · the joint module design used in these legs
- [[Sensor and control aspects of biped robot Johnnie|Johnnie]] · the predecessor robot
- [[Towards the design of a biped jogging robot|Biped jogging robot design]] · earlier ballscrew ankle experience
- [[Toe joints that enhance bipedal and fullbody motion of humanoid robots|Toe joints]] · active toes on H6 and H7
- [[Dynamics simulation for a biped robot - Modeling and experimental verification|Biped dynamics simulation]] · the multibody simulation used for loads
- [[Gait Analysis - Normal and Pathological Function|Gait Analysis, Perry]] · human knee torque capacity reference

## References

1. [[The development of Honda humanoid robot|Honda humanoid]] · Hirai et al. · 1998 · ICRA · [Scholar](https://scholar.google.com/scholar?q=The+development+of+Honda+humanoid+robot)
2. [[Humanoid JSK-H7 - Research platform for autonomous behavior and whole body motion|JSK-H7]] · Nishiwaki et al. · 2002 · IARP Workshop · [Scholar](https://scholar.google.com/scholar?q=Humanoid+JSK-H7+Research+platform+for+autonomous+behavior+and+whole+body+motion)
3. [[Humanoid robot HRP-2|HRP-2]] · Kaneko et al. · 2004 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Humanoid+robot+HRP-2)
4. [[Mechanical system of a small biped entertainment robot|QRIO mechanics]] · Ishida et al. · 2003 · IROS · [Scholar](https://scholar.google.com/scholar?q=Mechanical+system+of+a+small+biped+entertainment+robot)
5. [[Development of a human-like walking robot having two 7-DOF legs and a 2-DOF waist|WABIAN 7-DoF legs]] · Ogura et al. · 2004 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Development+of+a+human-like+walking+robot+having+two+7-DOF+legs+and+a+2-DOF+waist)
6. [[System design and dynamic walking of humanoid robot KHR-2|KHR-2]] · Kim et al. · 2005 · ICRA · [Scholar](https://scholar.google.com/scholar?q=System+design+and+dynamic+walking+of+humanoid+robot+KHR-2)
7. [[A hop towards running humanoid biped|Hop towards running]] · Kajita et al. · 2004 · ICRA · [Scholar](https://scholar.google.com/scholar?q=A+hop+towards+running+humanoid+biped)
8. [[New ASIMO - running at 6km-h|New ASIMO running]] · Honda Motor Co. · 2005 · web page · [Link](http://world.honda.com/HDTV/ASIMO/New-ASIMO-run-6kmh/)
9. [[Sensor and control aspects of biped robot Johnnie|Johnnie]] · Pfeiffer et al. · 2004 · IJHR · [Scholar](https://scholar.google.com/scholar?q=Sensor+and+control+aspects+of+biped+robot+Johnnie)
10. [[Gait Analysis - Normal and Pathological Function|Gait Analysis, Perry]] · Perry · 1992 · SLACK book · [Scholar](https://scholar.google.com/scholar?q=Perry+Gait+Analysis+Normal+and+Pathological+Function)
11. [[Toe joints that enhance bipedal and fullbody motion of humanoid robots|Toe joints]] · Nishiwaki et al. · 2002 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Toe+joints+that+enhance+bipedal+and+fullbody+motion+of+humanoid+robots)
12. [[Modular joint design for performance enhanced humanoid robot LOLA|LOLA modular joints]] · Lohmeier et al. · 2006 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Modular+joint+design+for+performance+enhanced+humanoid+robot+LOLA)
13. [[Real-time Ethernet SERCOS III, IEC-PAS 62410|SERCOS III]] · IEC · 2005 · standard · [Scholar](https://scholar.google.com/scholar?q=SERCOS+III+IEC+PAS+62410)
14. [[Dynamics simulation for a biped robot - Modeling and experimental verification|Biped dynamics simulation]] · Buschmann et al. · 2006 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Dynamics+simulation+for+a+biped+robot+Modeling+and+experimental+verification)
15. [[Entwurf und Realisierung einer zweibeinigen Laufmaschine|Gienger thesis]] · Gienger · 2005 · VDI-Verlag · [Scholar](https://scholar.google.com/scholar?q=Gienger+Entwurf+und+Realisierung+einer+zweibeinigen+Laufmaschine)
16. [[Towards the design of a biped jogging robot|Biped jogging robot design]] · Gienger et al. · 2001 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Towards+the+design+of+a+biped+jogging+robot)
17. [[Optimization based gait pattern generation for a biped robot|Optimization-based gait generation]] · Buschmann et al. · 2005 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=Optimization+based+gait+pattern+generation+for+a+biped+robot)

## See also

- [[Humanoid robot LOLA|LOLA]] · the full robot paper
- [[A Framework for Optimal Ankle Design of Humanoid Robots|Optimal Ankle Design Framework]] · modern optimization of parallel ankles
- [[Design and control of the rapid legged platform GAZELLE|GAZELLE]] · later linkage-driven knee and parallel ankle biped
