---
title: "Allometric Scaling Laws for Bipedal Robots"
aliases:
  - Allometric Scaling Laws
authors:
  - "[[Naomi Oke]]"
  - "[[Aja M. Carter]]"
  - "[[Ben Gu]]"
  - "[[Cordelia Pride]]"
  - "[[Steven Man]]"
  - "[[Sarah Bergbreiter]]"
  - "[[Aaron M. Johnson]]"
corresponding: "[[Naomi Oke]]"
institutions:
  - "[[Carnegie Mellon University]]"
labs:
  - "[[CMU Robotics Institute]]"
countries:
  - USA
year: 2026
date: 2026-04-02
venue: arXiv preprint
venue_type: preprint
arxiv_category: cs.RO
arxiv_id: "2603.22560"
url: https://arxiv.org/abs/2603.22560
pdf: https://arxiv.org/pdf/2603.22560v2
code: "Robot survey data said to be in an open-source repository, no link printed (checked 2026-11-02)"
status: to-read
relevance: high
added: 2026-11-02
tags:
  - paper
  - topic/morphology
  - topic/legged-locomotion
  - topic/hardware-design
  - topic/biomechanics
  - topic/actuators
  - topic/scaling-laws
  - sim/drake
  - eval/sim-only
---

# Allometric Scaling Laws

> [!info] Quick view
> **Authors:** [[Naomi Oke]] (corresponding), [[Aja M. Carter]], [[Ben Gu]], [[Cordelia Pride]], [[Steven Man]], [[Sarah Bergbreiter]], [[Aaron M. Johnson]]
> **From:** [[Carnegie Mellon University]] · [[CMU Robotics Institute]]
> **Published:** arXiv preprint · Mar 2026 (v2 Apr 2026) · cs.RO
> **Keywords:** allometric scaling, isometric scaling, leg length, dynamic similarity, Froude number, torque scaling, foot shape, quasi-passive biped, Drake
> **Links:** [arXiv](https://arxiv.org/abs/2603.22560) · [PDF](https://arxiv.org/pdf/2603.22560v2)
> **One line:** across 47 real legged robots and scaled simulations of two tiny bipeds, robot mass grows like L² (not L³), walking speed like √L, and required torque like m·L, so robots do not scale like geometry or animals predict

## Summary

The question is how a robot design changes when you make it bigger or smaller. First they survey 47 legged robots (10 "lower" bipeds, 17 full bipeds, 14 quadrupeds, 6 hexapods) and fit power laws against leg length L. Mass scales about m ∝ L^2.1 globally and just under L² for bipeds, much slower than the L³ of isometric scaling, so small robots are relatively heavy and big ones relatively light. Body length grows like L^0.86, so bigger robots have relatively longer legs. Speed showed no clear trend because papers report speed in inconsistent ways.

Then they scale two real quasi-passive one-actuator bipeds, Mugatu (15 cm legs) and Zippy (2.5 cm), plus a Zippy with spherical feet, in Drake from 2 cm to 1.2 m leg length, with mass scaled as L² or L³ and stride frequency as L^-1/2. Walking speed follows √L (constant Froude number) for both mass laws, minimum hip torque follows about m·L (L³ or L⁴) instead of the isometric m·L² (L⁵), which fits what motors can deliver at each size, and the feasible foot radii scale linearly with L, but the fastest foot shape changes with scale. Honestly the simulation only covers single-actuator passive walkers, so the torque and foot results are a narrow slice, but the survey table is a useful reference.

## How it works

- Survey: leg length = hip height when standing, body length = longest dimension, regressions per morphology class
- Simulation: Drake hydroelastic contact tuned against real Mugatu and Zippy hardware at 1x scale, mesh resolution scaled down for small sizes
- Scaling rules: all lengths scaled together, mass ∝ L² or L³, stride frequency ∝ L^-1/2, minimum torque tuned by hand until stable walking
- Metrics after convergence to a pitch limit cycle: velocity, hip torque, roll/pitch/yaw amplitude
- Foot study: foot semi-axes scaled 0.5x to 2x along each axis at three body scales, 25 s per simulation

## Results that matter

- Survey: m ∝ L^2.12 globally, bipeds about L^1.9, quadrupeds L^2.7, body length ∝ L^0.86
- Velocity ∝ L^0.47 to L^0.52 for all robots and both mass laws, close to √L
- Minimum torque ∝ L^2.9 to L^3.5 with m ∝ L², ∝ L^3.9 to L^4.1 with m ∝ L³, overall about τ ∝ mL
- Feasible foot semi-axes ∝ L^0.99 to L^1.01, but the optimal foot aspect ratio changes with scale
- Mugatu at 1 m beats Froude prediction by 24%, Zippy by about 9%
- A 1 m version of the simple biped would weigh about 33 kg with m ∝ L² instead of 1600 kg with L³

## Interesting stuff

- The L² mass scaling comes from components (motors, electronics, batteries) not scaling with size, a design reality, not physics
- Torque demand τ ∝ mL sits inside the τ ∝ L^3.5 to L^4 range of motor torque, so scaled designs remain feasible
- Only one-actuator passive walkers in simulation, results may not hold for full humanoids with active control
- Speed data in the literature is a mess, which is a fair point for the whole field

## Useful for me

- **Apply.** For a humanoid that changes limb length, use these laws as sanity checks: expected walking speed ∝ √L, required hip torque ∝ m·L. Lengthening legs should increase achievable speed but also torque demand.
- **Apply.** Command ranges in RL training could be scaled with Froude number (v ∝ √L) when the leg length changes, so each body is asked for comparable gaits.
- **Watch out.** The optimal foot shape changes with scale, so a fixed foot may not be optimal over the whole range of leg lengths.
- **As prior work.** Good reference for the physics of changing size in legged robots.

## Key prior work

- [[Principles of Animal Locomotion|Principles of Animal Locomotion]] · dynamic similarity and Froude scaling
- [[The simplest walking robot - A bipedal robot with one actuator and two rigid bodies|Mugatu]] · the larger robot scaled here
- [[Zippy - The smallest power-autonomous bipedal robot|Zippy]] · the smaller robot that motivated the study
- [[Why animals can outrun robots|Why animals outrun robots]] · biology vs robot performance
- [[Scaling laws for mesoscale and microscale systems|Spletzer scaling laws]] · isometric torque scaling baseline
- [[Scaling down an insect-size microrobot, HAMR-VI into HAMR-Jr|HAMR-Jr]] · elastic similarity in scaled robots

## References

1. [[Principles of Animal Locomotion|Principles of Animal Locomotion]] · Alexander · 2003 · Princeton University Press · [Scholar](https://scholar.google.com/scholar?q=Alexander+Principles+of+Animal+Locomotion)
2. [[Muscles, Reflexes, and Locomotion|Muscles, Reflexes, and Locomotion]] · McMahon · 1984 · Princeton University Press · [Scholar](https://scholar.google.com/scholar?q=McMahon+Muscles+Reflexes+and+Locomotion)
3. [[Size and shape in biology - Elastic criteria impose limits on biological proportions, and consequently on metabolic rates|Size and shape in biology]] · McMahon · 1973 · Science · [Scholar](https://scholar.google.com/scholar?q=Size+and+shape+in+biology+Elastic+criteria+impose+limits+on+biological+proportions)
4. [[Elastic and-or geometric similarity in mammalian design|Elastic or geometric similarity]] · Economos · 1983 · Journal of Theoretical Biology · [Scholar](https://scholar.google.com/scholar?q=Elastic+and+or+geometric+similarity+in+mammalian+design)
5. [[On the origin of biological similarity|Origin of biological similarity]] · Economos · 1982 · Journal of Theoretical Biology · [Scholar](https://scholar.google.com/scholar?q=On+the+origin+of+biological+similarity+Economos)
6. [[Allometry of the limb bones of mammals from shrews (Sorex) to elephant (Loxodonta)|Limb bone allometry]] · Alexander et al. · 1979 · Journal of Zoology · [Scholar](https://scholar.google.com/scholar?q=Allometry+of+the+limb+bones+of+mammals+from+shrews+to+elephant)
7. [[Why animals can outrun robots|Why animals outrun robots]] · Burden et al. · 2024 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=Why+animals+can+outrun+robots)
8. [[Design of a bio-inspired dynamical vertical climbing robot|Dynamical climbing robot]] · Clark et al. · 2007 · RSS · [Scholar](https://scholar.google.com/scholar?q=Design+of+a+bio-inspired+dynamical+vertical+climbing+robot)
9. [[Vertical dry adhesive climbing with a 100x bodyweight payload|Dry adhesive climbing]] · Hawkes et al. · 2015 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Vertical+dry+adhesive+climbing+with+a+100x+bodyweight+payload)
10. [[MiniRHex - A small, open-source, fully programmable walking hexapod|MiniRHex]] · Barragan et al. · 2018 · RSS Workshop · [Scholar](https://scholar.google.com/scholar?q=MiniRHex+A+small+open-source+fully+programmable+walking+hexapod)
11. [[Scaling down an insect-size microrobot, HAMR-VI into HAMR-Jr|HAMR-Jr]] · Jayaram et al. · 2020 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Scaling+down+an+insect-size+microrobot+HAMR-VI+into+HAMR-Jr)
12. [[The microDelta - Downscaling robot mechanisms enables ultrafast and high-precision movement|microDelta]] · Man et al. · 2025 · Science Robotics · [Scholar](https://scholar.google.com/scholar?q=The+microDelta+Downscaling+robot+mechanisms+enables+ultrafast+and+high-precision+movement)
13. [[The simplest walking robot - A bipedal robot with one actuator and two rigid bodies|Mugatu]] · Kyle et al. · 2023 · Humanoids · [Scholar](https://scholar.google.com/scholar?q=The+simplest+walking+robot+A+bipedal+robot+with+one+actuator+and+two+rigid+bodies)
14. [[Zippy - The smallest power-autonomous bipedal robot|Zippy]] · Man et al. · 2025 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Zippy+The+smallest+power-autonomous+bipedal+robot)
15. [[A dynamic similarity hypothesis for the gaits of quadrupedal mammals|Dynamic similarity hypothesis]] · Alexander & Jayes · 1983 · Journal of Zoology · [Scholar](https://scholar.google.com/scholar?q=A+dynamic+similarity+hypothesis+for+the+gaits+of+quadrupedal+mammals)
16. [[Allometric scaling of body length - Elastic or geometric similarity in mammalian design|Body length allometry]] · Silva · 1998 · Journal of Mammalogy · [Scholar](https://scholar.google.com/scholar?q=Allometric+scaling+of+body+length+Elastic+or+geometric+similarity+in+mammalian+design)
17. [[Scale effects between body size and limb design in quadrupedal mammals|Body size and limb design]] · Kilbourne & Hoffman · 2013 · PLOS ONE · [Scholar](https://scholar.google.com/scholar?q=Scale+effects+between+body+size+and+limb+design+in+quadrupedal+mammals)
18. [[Scaling of avian bipedal locomotion reveals independent effects of body mass and leg posture on gait|Avian bipedal scaling]] · Daley & Birn-Jeffery · 2018 · Journal of Experimental Biology · [Scholar](https://scholar.google.com/scholar?q=Scaling+of+avian+bipedal+locomotion+reveals+independent+effects+of+body+mass+and+leg+posture+on+gait)
19. [[A general scaling law reveals why the largest animals are not the fastest|Largest animals not fastest]] · Hirt et al. · 2017 · Nature Ecology & Evolution · [Scholar](https://scholar.google.com/scholar?q=A+general+scaling+law+reveals+why+the+largest+animals+are+not+the+fastest)
20. [[Dynamic similarity and the peculiar allometry of maximum running speed|Max running speed allometry]] · Labonte et al. · 2024 · Nature Communications · [Scholar](https://scholar.google.com/scholar?q=Dynamic+similarity+and+the+peculiar+allometry+of+maximum+running+speed)
21. [[Analyzing modeled torque profiles to understand scale-dependent active muscle responses in the hip joint|Scale-dependent hip torque]] · Young et al. · 2022 · Biomimetics · [Scholar](https://scholar.google.com/scholar?q=Analyzing+modeled+torque+profiles+to+understand+scale-dependent+active+muscle+responses+in+the+hip+joint)
22. [[From cockroaches to tanks - The same power-mass-speed relation describes both biological and artificial ground-mobile systems|Cockroaches to tanks]] · Kott et al. · 2021 · PLOS ONE · [Scholar](https://scholar.google.com/scholar?q=From+cockroaches+to+tanks+The+same+power-mass-speed+relation)
23. [[Energetics and mechanics of terrestrial locomotion. IV. Total mechanical energy changes as a function of speed and body size in birds and mammals|Terrestrial locomotion energetics IV]] · Heglund et al. · 1982 · Journal of Experimental Biology · [Scholar](https://scholar.google.com/scholar?q=Energetics+and+mechanics+of+terrestrial+locomotion+IV+total+mechanical+energy+changes+as+a+function+of+speed+and+body+size)
24. [[Scaling laws for mesoscale and microscale systems|Spletzer scaling laws]] · Spletzer · 1999 · IEEE Trans. CPMT · [Scholar](https://scholar.google.com/scholar?q=Spletzer+Scaling+laws+for+mesoscale+and+microscale+systems)
25. [[Fascination of down scaling - Alice the sugar cube robot|Alice sugar cube robot]] · Caprari et al. · 2000 · ICRA Workshop · [Scholar](https://scholar.google.com/scholar?q=Fascination+of+down+scaling+Alice+the+sugar+cube+robot)
26. [[Rigid vs. elastic actuation - Requirements & performance|Rigid vs elastic actuation]] · Haddadin et al. · 2012 · IROS · [Scholar](https://scholar.google.com/scholar?q=Rigid+vs+elastic+actuation+Requirements+performance)
27. [[Sizing equations for electrical machinery|Electrical machine sizing]] · Honsinger · 1987 · IEEE Trans. Energy Conversion · [Scholar](https://scholar.google.com/scholar?q=Honsinger+Sizing+equations+for+electrical+machinery)
28. [[The influence of DC electric drives on sizing quadruped robots|DC drives and quadruped sizing]] · Chatzakos & Papadopoulos · 2008 · ICRA · [Scholar](https://scholar.google.com/scholar?q=The+influence+of+DC+electric+drives+on+sizing+quadruped+robots)
29. [[Torque scaling laws for interior and exterior rotor permanent magnet machines|PM machine torque scaling]] · Reichert et al. · 2009 · Intermag · [Scholar](https://scholar.google.com/scholar?q=Torque+scaling+laws+for+interior+and+exterior+rotor+permanent+magnet+machines)
30. [[Scaling laws in robotics|Scaling laws in robotics]] · Dermitzakis et al. · 2011 · Procedia Computer Science · [Scholar](https://scholar.google.com/scholar?q=Dermitzakis+Scaling+laws+in+robotics)
31. [[Scaling of robotic mechanisms|Scaling of robotic mechanisms]] · Waldron & Hubert · 2000 · ICRA · [Scholar](https://scholar.google.com/scholar?q=Waldron+Hubert+Scaling+of+robotic+mechanisms)
32. [[Design principles for energy-efficient legged locomotion and implementation on the MIT Cheetah robot|MIT Cheetah energy efficiency]] · Seok et al. · 2015 · IEEE/ASME TMECH · [Scholar](https://scholar.google.com/scholar?q=Design+principles+for+energy-efficient+legged+locomotion+and+implementation+on+the+mit+cheetah+robot)
33. [[Scaling laws for parallel motor-gearbox arrangements|Parallel motor-gearbox scaling]] · Saerens et al. · 2020 · IROS · [Scholar](https://scholar.google.com/scholar?q=Scaling+laws+for+parallel+motor-gearbox+arrangements)
34. [[Design Principles for a Family of Direct-Drive Legged Robots|Direct-drive legged robots]] · Kenneally et al. · 2016 · IEEE RA-L · [Scholar](https://scholar.google.com/scholar?q=Design+Principles+for+a+Family+of+Direct-Drive+Legged+Robots)
35. [[Scaling laws for robotic transmissions|Transmission scaling laws]] · Saerens et al. · 2019 · Mechanism and Machine Theory · [Scholar](https://scholar.google.com/scholar?q=Scaling+laws+for+robotic+transmissions)
36. [[Drake - Model-based design and verification for robotics|Drake]] · Tedrake et al. · 2019 · software · [Link](https://drake.mit.edu/)

## See also

- [[Structural Optimization of Lightweight Bipedal Robot via SERL|SERL]] · RL-based choice of leg lengths for a biped
- [[Self-Modifying Morphology Experiments with DyRET - Dynamic Robot for Embodied Testing|DyRET self-modifying]] · leg length as a speed-torque gear on real hardware
