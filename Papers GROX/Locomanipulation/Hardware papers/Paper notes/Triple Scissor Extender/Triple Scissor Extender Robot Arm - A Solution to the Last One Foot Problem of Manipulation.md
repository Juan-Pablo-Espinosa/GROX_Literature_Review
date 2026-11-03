---
title: "Triple Scissor Extender Robot Arm: A Solution to the Last One Foot Problem of Manipulation"
aliases:
  - Triple Scissor Extender
  - TSERA
authors:
  - "[[Abbas Shikari]]"
  - "[[Harry Asada]]"
corresponding: "[[Abbas Shikari]]"
institutions:
  - "[[Massachusetts Institute of Technology]]"
labs:
  - "[[d'Arbeloff Laboratory for Information Systems and Technology]]"
countries:
  - USA
year: 2018
date: 2018-07-24
venue: IEEE Robotics and Automation Letters 2018 (Vol. 3, No. 4)
venue_type: journal
arxiv_category: ""
arxiv_id: ""
url: https://doi.org/10.1109/LRA.2018.2859022
pdf: https://doi.org/10.1109/LRA.2018.2859022
code: "No official repo (hardware paper, checked 2026-11-02)"
status: to-read
relevance: medium
added: 2026-11-02
tags:
  - paper
  - topic/manipulation
  - topic/hardware-design
  - topic/variable-length-limbs
  - topic/parallel-mechanisms
  - topic/reconfigurable-robots
  - method/motion-planning
  - eval/real-robot
---

# Triple Scissor Extender

> [!info] Quick view
> **Authors:** [[Abbas Shikari]] (corresponding), [[Harry Asada]]
> **From:** [[Massachusetts Institute of Technology]] · [[d'Arbeloff Laboratory for Information Systems and Technology]]
> **Published:** IEEE RA-L · Jul 2018 · journal
> **Keywords:** expandable arm, scissor linkage, zero-torsion parallel mechanism, 3-RPS, sequential expansion, plant growth, confined space access
> **Links:** [DOI](https://doi.org/10.1109/LRA.2018.2859022)
> **One line:** a segmented arm where each segment is a three-scissor parallel mechanism that extends and tilts, grown one segment at a time like a plant stem to reach through narrow winding channels

## Summary

The "last one foot problem" is reaching a target in a cluttered, narrow space inside machinery, where normal arms are too bulky and planning is expensive. TSERA stacks segments that each have 3 DoF (extend, and tilt about two axes), built as a zero-torsion 3-RPS parallel mechanism whose three legs are scissor linkages driven by lead screws. Inspired by plant growth, the arm is extended sequentially: the proximal segment extends first while all others stay contracted as one rigid body, so each step only needs collision checking for one rigid body, and waypoints along the channel centerline are found with a binary search on the extension length.

They analyze the scissor kinematics (an offset in the bottom link removes the fully-contracted singularity and bounds actuator force), the full segment inverse kinematics, and the parasitic sideways translation of zero-torsion mechanisms, which they compensate by solving for the segment pose whose top-plate normal points at the next waypoint. The prototype segment extends to 240 mm with an extension ratio of 5.3, weighs 560 g, tilts up to 75°, has 80 µm repeatability and 57 N/mm axial stiffness, and the arm threads through a confined channel. Honestly the experiments are mostly qualitative, but the sequential-growth planning idea is very neat.

## How it works

- Segment: top and bottom triangular plates connected by three scissor legs, prismatic via a bidirectional lead screw and two nuts, revolute at the base and spherical at the top
- Equal extension of all three scissors lifts the plate, differences tilt it about any horizontal axis
- Scissor kinematics: L = le sin θ + lb cos θ, w = 2l0 cos θ − lb sin θ, actuator force F < Mg le/(2 lb), so the offset lb bounds force near the singularity
- Parasitic translation r = ½ R_t (1 − cos γ) from Bonev, compensated by gradient descent on the alignment between the plate normal and the direction to the next waypoint
- Sequential Expansion Algorithm: configuration-space collision-free tube, binary search for the furthest reachable waypoint on the centerline, extend segment k, freeze it, repeat
- Hardware: ABS printed plates and nuts, waterjet aluminum scissors, Arduino Mega with PID on each scissor

## Results that matter

- Segment max height 240 mm, extension ratio 5.3, 560 g, 155 mm diameter (slenderness 1.5)
- Max tilt 75° about any horizontal axis
- Z repeatability 80 µm, axial stiffness 57 N/mm
- Successful traversal of a confined channel following the planned trajectory
- No payload, speed or full-arm accuracy numbers

## Interesting stuff

- Growing from the base and freezing proximal segments reduces a high-DoF planning problem to a sequence of 3-DoF problems, a really elegant use of morphology to simplify control
- After reaching the target, dexterity depends on how many segments are still unexpanded, so the planner should extend proximal segments as much as possible
- Scissor linkages give large extension ratios with good stiffness, better than pneumatic growing robots for payload
- The parasitic sideways motion of zero-torsion mechanisms is a real nuisance that must be modeled

## Useful for me

- **As prior work.** Strong example of a limb that grows segment by segment, very relevant as mechanical inspiration for extending limbs.
- **Apply.** The scissor offset trick that removes the contracted singularity and bounds actuator force is useful for any scissor-based extension mechanism.
- **Apply.** "Extend proximal first, then distal" is a nice idea for a humanoid that changes limb length to reach: change length in a sequence that keeps planning simple.
- **Gap.** Quasi-static and only for manipulation in free space, no dynamics or locomotion.

## Key prior work

- [[Design and analysis of 6-DOF triple scissor extender robots with applications in aircraft assembly|6-DoF triple scissor extender]] · the original triple scissor extender
- [[Triple scissor extender - A 6-DOF lifting and positioning robot|Triple scissor extender ICRA]] · first version of the mechanism
- [[Direct kinematics of zero-torsion parallel mechanisms|Zero-torsion kinematics]] · the parallel mechanism family used
- [[Singularity analysis of zero-torsion parallel mechanisms|Zero-torsion singularities]] · tilt limits
- [[A soft robot that navigates its environment through growth|Vine robot]] · growing soft robot alternative
- [[Design of a spherical robot arm with the spiral zipper prismatic joint|Spiral zipper arm]] · high extension ratio prismatic joint

## References

1. [[An inductance-based sensing system for bellows-driven continuum joints in soft robots|Bellows sensing]] · Felt et al. · 2017 · RSS · [Scholar](https://scholar.google.com/scholar?q=An+inductance-based+sensing+system+for+bellows-driven+continuum+joints+in+soft+robots)
2. [[A soft robot that navigates its environment through growth|Vine robot]] · Hawkes et al. · 2017 · Science Robotics · [Link](http://robotics.sciencemag.org/content/robotics/2/8/eaan3028.full.pdf)
3. [[Pneumatic reel actuator - Design, modeling, and implementation|Pneumatic reel actuator]] · Hammond et al. · 2017 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Pneumatic+reel+actuator+Design+modeling+and+implementation)
4. [[Series Pneumatic Artificial Muscles (sPAMs) and application to a soft continuum robot|sPAMs]] · Greer et al. · 2017 · ICRA · [Link](http://ieeexplore.ieee.org/document/7989648/)
5. [[Rescue robots and systems in Japan|Rescue robots Japan]] · Matsuno & Tadokoro · 2004 · ROBIO · [Scholar](https://scholar.google.com/scholar?q=Rescue+robots+and+systems+in+Japan)
6. [[Design and architecture of the unified modular snake robot|Unified modular snake]] · Wright et al. · 2012 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+and+architecture+of+the+unified+modular+snake+robot)
7. [[Snake-like robots - Machine design of biologically inspired robots|Snake-like robots]] · Hirose & Yamada · 2009 · IEEE RAM · [Scholar](https://scholar.google.com/scholar?q=Snake-like+robots+Machine+design+of+biologically+inspired+robots)
8. [[Design of a modular snake robot|Modular snake robot]] · Wright et al. · 2007 · IROS · [Scholar](https://scholar.google.com/scholar?q=Design+of+a+modular+snake+robot+Wright)
9. [[The second generation prototype of a Duct Climbing Tensegrity robot, DuCTTv2|DuCTTv2]] · Friesen et al. · 2016 · ICRA · [Scholar](https://scholar.google.com/scholar?q=The+second+generation+prototype+of+a+Duct+Climbing+Tensegrity+robot+DuCTTv2)
10. [[Modular continuum robotic endoscope design and path planning|Continuum endoscope]] · Chen et al. · 2014 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Modular+continuum+robotic+endoscope+design+and+path+planning)
11. [[The development of a robotic endoscope|Robotic endoscope]] · Grundfest et al. · 1995 · IROS · [Scholar](https://scholar.google.com/scholar?q=The+development+of+a+robotic+endoscope+Grundfest)
12. [[Spatial planning - A configuration space approach|Configuration space]] · Lozano-Pérez · 1983 · IEEE Trans. Computers · [Link](http://lis.csail.mit.edu/pubs/tlp/spatial-planning.pdf)
13. [[Robot Motion Planning|Robot Motion Planning]] · Latombe · 1991 · Kluwer · [Scholar](https://scholar.google.com/scholar?q=Latombe+Robot+Motion+Planning)
14. [[Analysis and stiffness evaluation of a micro-parallel kinematic|Micro-parallel stiffness]] · Yañez-Valdez · 2017 · DYNA · [Scholar](https://scholar.google.com/scholar?q=Analysis+and+stiffness+evaluation+of+a+microparallel+kinematic)
15. [[A unified algorithm to determine the reachable and dexterous workspace of parallel manipulators|Parallel workspace algorithm]] · Wang et al. · 2010 · Robot. Comput.-Integr. Manuf. · [Scholar](https://scholar.google.com/scholar?q=A+unified+algorithm+to+determine+the+reachable+and+dexterous+workspace+of+parallel+manipulators)
16. [[Design and analysis of 6-DOF triple scissor extender robots with applications in aircraft assembly|6-DoF triple scissor extender]] · Gonzalez & Asada · 2017 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Design+and+analysis+of+6-DOF+triple+scissor+extender+robots+with+applications+in+aircraft+assembly)
17. [[Triple scissor extender - A 6-DOF lifting and positioning robot|Triple scissor extender ICRA]] · Gonzalez & Asada · 2016 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Triple+scissor+extender+A+6-DOF+lifting+and+positioning+robot)
18. [[Direct kinematics of zero-torsion parallel mechanisms|Zero-torsion kinematics]] · Bonev · 2008 · ICRA · [Link](http://ieeexplore.ieee.org/document/4543802/)
19. [[Singularity analysis of zero-torsion parallel mechanisms|Zero-torsion singularities]] · Briot & Bonev · 2008 · IROS · [Scholar](https://scholar.google.com/scholar?q=Singularity+analysis+of+zero-torsion+parallel+mechanisms)
20. [[Design of a spherical robot arm with the spiral zipper prismatic joint|Spiral zipper arm]] · Collins & Yim · 2016 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Design+of+a+spherical+robot+arm+with+the+Spiral+Zipper+prismatic+joint)
21. [[Zipper mast for enhanced communications and surveillance|Zipper mast]] · Woodruff et al. · 2011 · Proc. SPIE · [Scholar](https://scholar.google.com/scholar?q=Zipper+mast+for+enhanced+communications+and+surveillance)

## See also

- [[Analysis of Task Feasibility for a Home Robot using Prismatic Joints|PRISM]] · earlier scissor (lazy tong) prismatic arm
- [[Bioinspired growable humanoid robot with bone-mimetic linkages for versatile mobility|GrowHR]] · growable limbs on a humanoid
